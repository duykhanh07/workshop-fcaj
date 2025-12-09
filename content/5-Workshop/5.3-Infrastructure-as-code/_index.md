---
title : "Infrastructure as Code"
#date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

Instead of manually clicking hundreds of times on the AWS Console to create Databases, Lambda, API Gateway, etc., we will use the **Infrastructure as Code (IaC)** method.

With **AWS SAM (Serverless Application Model)**, the entire system architecture is defined in a single file: `template.yaml`.

---

## 1. Global Configuration (Globals)

To avoid code repetition (Don't Repeat Yourself), we define common parameters for all Lambda functions in the `Globals` section.

```
YAML

Globals:
  Function:
    Timeout: 60 # Tăng lên 60s vì gọi AI Bedrock có thể lâu
    MemorySize: 2048 # Java cần RAM khá để chạy mượt
    Runtime: java17
    Architectures: [x86_64]
    Environment:
      Variables:
        TABLE_NAME: !Ref CoreTable
        # ID của model Bedrock (Claude 3 Haiku)
        BEDROCK_MODEL_ID: "anthropic.claude-3-haiku-20240307-v1:0"
```

With Java, more RAM equates to a stronger CPU and shorter startup time (Cold Start). Combined with the SnapStart feature, the application will respond instantly.

---

## 2. Backend Definition (Lambda Functions)
Each of our features corresponds to an `AWS::Serverless::Function` resource.

You can view the Lambda definitions in `template.yaml` [at this github link](https://github.com/duykhanh07/ai-career-coach)

Example with the **Cover Letter** function:
```
YAML

    CoverLetterFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: backend/target/backend-0.0.1-SNAPSHOT-aws.jar
      Handler: org.springframework.cloud.function.adapter.aws.FunctionInvoker::handleRequest
      Timeout: 60 # Gọi AI nên cần timeout lâu
      MemorySize: 2048
      SnapStart:
        ApplyOn: PublishedVersions
      Policies:
        - DynamoDBCrudPolicy:
            TableName: !Ref CoreTable
        - Statement:
            - Effect: Allow
              Action:
                - bedrock:InvokeModel
                - bedrock:InvokeModelWithResponseStream
                # Thêm quyền Marketplace để sửa lỗi AccessDenied
                - aws-marketplace:ViewSubscriptions
                - aws-marketplace:Subscribe
                - aws-marketplace:Unsubscribe
              Resource: "*"
      Environment:
        Variables:
          TABLE_NAME: !Ref CoreTable
          BEDROCK_MODEL_ID: "anthropic.claude-3-haiku-20240307-v1:0"
          # Chạy hàm coverLetterHandler
          SPRING_CLOUD_FUNCTION_DEFINITION: coverLetterHandler
      Events:
        # 1. Generate (POST)
        GenerateCoverLetter:
          Type: HttpApi
          Properties:
            ApiId: !Ref HttpApi
            Path: /cover-letters
            Method: POST
            Auth:
              Authorizer: CognitoAuthorizer

        # 2. List All (GET)
        ListCoverLetters:
          Type: HttpApi
          Properties:
            ApiId: !Ref HttpApi
            Path: /cover-letters
            Method: GET
            Auth:
              Authorizer: CognitoAuthorizer

        # 3. Get One (GET /{id})
        GetOneCoverLetter:
          Type: HttpApi
          Properties:
            ApiId: !Ref HttpApi
            Path: /cover-letters/{id}
            Method: GET
            Auth:
              Authorizer: CognitoAuthorizer

        # 4. Delete (DELETE /{id})
        DeleteCoverLetter:
          Type: HttpApi
          Properties:
            ApiId: !Ref HttpApi
            Path: /cover-letters/{id}
            Method: DELETE
            Auth:
              Authorizer: CognitoAuthorizer
```
The most important note regarding Lambda functions is: does the code run with the correct logic but still encounter an `AccessDenied` error? That is because Lambda by default has no permissions to do anything. We must grant specific permissions:
1. `bedrock:InvokeModel`: Allows Lambda to send prompts to the Claude 3 model.
2. `bedrock:InvokeModelWithResponseStream`: Allows calling the model in streaming mode (data returned in a stream — token by token). Used for applications that need to receive output incrementally (streaming responses).
3. `aws-marketplace:ViewSubscriptions`: Allows viewing the subscription status of products on the AWS Marketplace for the account (lists existing subscriptions). Needed to check if the account has subscribed to third-party models.
4. `aws-marketplace:Subscribe`: Because Claude 3 is a third-party model (Anthropic), Lambda needs permission to check if your AWS account has subscribed to this model. Without this permission, Bedrock will refuse service.
5. `aws-marketplace:Unsubscribe`: Allows unsubscribing from a product on the Marketplace.

---

## 3. Database & Auth Definition

DynamoDB (Single Table)
We declare the table with the PAY_PER_REQUEST (Serverless) billing mode — pay only for what you use, with no maintenance costs when no one is using it.

```
YAML

    CoreTable:
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: AICareerCoachDB
      BillingMode: PAY_PER_REQUEST
      AttributeDefinitions:
        - AttributeName: PK
          AttributeType: S
        - AttributeName: SK
          AttributeType: S
        - AttributeName: GSI1_PK
          AttributeType: S
        - AttributeName: GSI1_SK
          AttributeType: S
      KeySchema:
        - AttributeName: PK
          KeyType: HASH
        - AttributeName: SK
          KeyType: RANGE
      GlobalSecondaryIndexes:
        - IndexName: GSI1
          KeySchema:
            - AttributeName: GSI1_PK
              KeyType: HASH
            - AttributeName: GSI1_SK
              KeyType: RANGE
          Projection:
            ProjectionType: ALL
```

---

## 4. Cognito User Pool
Where users are managed. We configure AutoVerifiedAttributes: [email] to automatically send verification emails upon registration.

```
YAML

  UserPool:
    Type: AWS::Cognito::UserPool
    Properties:
      UserPoolName: CareerCoachUsers
      AutoVerifiedAttributes: [email]
      UsernameAttributes: [email]
      Policies:
        PasswordPolicy:
          MinimumLength: 8
      # Kích hoạt Trigger sau khi đăng ký để lưu vào DynamoDB
      LambdaConfig:
        PostConfirmation: !GetAtt PostSignupTriggerFunction.Arn

  UserPoolClient:
    Type: AWS::Cognito::UserPoolClient
    Properties:
      ClientName: ReactClient
      UserPoolId: !Ref UserPool
      GenerateSecret: false # Web App (React) không dùng Secret
      ExplicitAuthFlows: [ALLOW_USER_SRP_AUTH, ALLOW_REFRESH_TOKEN_AUTH]

  # Lambda nhỏ (NodeJS) để hứng sự kiện đăng ký và lưu user vào DB
  # (Dùng Nodejs cho cái này vì nó khởi động cực nhanh và nhẹ)
  PostSignupTriggerFunction:
    Type: AWS::Serverless::Function
    Properties:
      Runtime: nodejs18.x
      MemorySize: 128
      Timeout: 5
      Handler: index.handler
      InlineCode: |
        const { DynamoDBClient, PutItemCommand } = require("@aws-sdk/client-dynamodb");
        const client = new DynamoDBClient({});
        exports.handler = async (event) => {
          if (event.triggerSource === "PostConfirmation_ConfirmSignUp") {
            const userId = event.request.userAttributes.sub;
            const email = event.request.userAttributes.email;
            const params = {
              TableName: process.env.TABLE_NAME,
              Item: {
                PK: { S: `USER#${userId}` },
                SK: { S: "METADATA" },
                email: { S: email },
                createdAt: { S: new Date().toISOString() }
              }
            };
            try { await client.send(new PutItemCommand(params)); } 
            catch (err) { console.error(err); }
          }
          return event;
        };
      Environment:
        Variables:
          TABLE_NAME: !Ref CoreTable
      Policies:
        - DynamoDBCrudPolicy:
            TableName: !Ref CoreTable

  # Cấp quyền cho Cognito được gọi Lambda Trigger
  PostSignupTriggerPermission:
    Type: AWS::Lambda::Permission
    Properties:
      Action: lambda:InvokeFunction
      FunctionName: !Ref PostSignupTriggerFunction
      Principal: cognito-idp.amazonaws.com
      SourceArn: !GetAtt UserPool.Arn
```

## 5. Frontend Hosting (S3 + CloudFront OAC)
This is currently the most modern and secure hosting architecture for Static Web.

![S3 + CloudFront OAC](/images/5-Workshop/5.3-Infrastructure-as-code/cloudfront-oac.png)

Components:
* S3 Bucket: Stores index.html, js, css files. PublicAccessBlock mode is enabled (Blocks direct access from the internet).
* CloudFront Distribution: CDN that helps cache content and provides HTTPS.
* Origin Access Control (OAC): Uses OAC. This acts as the intermediary bodyguard. It signs requests sent from CloudFront to S3.

Result: Users are forced to go through CloudFront (with HTTPS, with Cache). If they intentionally try to access the S3 link directly, they will be blocked (403 Forbidden).

```
YAML

    # Cấu hình bảo mật mới (OAC)
  CloudFrontOriginAccessControl:
    Type: AWS::CloudFront::OriginAccessControl
    Properties:
      OriginAccessControlConfig:
        Description: "Access Control for CV Analyzer"
        Name: !Sub "OAC-${FrontendBucket}"
        OriginAccessControlOriginType: s3
        SigningBehavior: always
        SigningProtocol: sigv4
```

```
YAML

    FrontendBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub "career-coach-frontend-${AWS::AccountId}" # Thêm ID tài khoản để chắc chắn không trùng tên
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true

  FrontendBucketPolicy:
    Type: AWS::S3::BucketPolicy
    Properties:
      Bucket: !Ref FrontendBucket
      PolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Sid: "AllowCloudFrontServicePrincipal"
            Effect: Allow
            Principal:
              Service: "cloudfront.amazonaws.com" # Dùng Service Principal chuẩn (Luôn tồn tại)
            Action: "s3:GetObject"
            Resource: !Sub "${FrontendBucket.Arn}/*"
            Condition:
              StringEquals:
                "AWS:SourceArn": !Sub "arn:aws:cloudfront::${AWS::AccountId}:distribution/${FrontendDistribution}"

  FrontendDistribution:
    Type: AWS::CloudFront::Distribution
    Properties:
      DistributionConfig:
        Enabled: true
        DefaultRootObject: index.html
        Origins:
          - DomainName: !GetAtt FrontendBucket.RegionalDomainName # Lưu ý dùng RegionalDomainName
            Id: S3Origin
            OriginAccessControlId: !GetAtt CloudFrontOriginAccessControl.Id # Trỏ vào OAC mới
            S3OriginConfig:
              OriginAccessIdentity: "" # Để trống dòng này vì đã dùng OAC
        DefaultCacheBehavior:
          TargetOriginId: S3Origin
          ViewerProtocolPolicy: redirect-to-https
          AllowedMethods: [GET, HEAD, OPTIONS]
          CachedMethods: [GET, HEAD, OPTIONS]
          ForwardedValues:
            QueryString: false
            Cookies:
              Forward: none
        CustomErrorResponses:
          - ErrorCode: 403
            ResponseCode: 200
            ResponsePagePath: /index.html
          - ErrorCode: 404
            ResponseCode: 200
            ResponsePagePath: /index.html
```

## 6. Outputs
After deployment is complete, CloudFormation will return important parameters for us to configure the Frontend.

```
YAML

Outputs:
  ApiEndpoint:
    Description: "API URL để dán vào Frontend .env"
    Value: !Sub "https://${HttpApi}.execute-api.${AWS::Region}.amazonaws.com"
  CognitoUserPoolId:
    Description: "User Pool ID"
    Value: !Ref UserPool
  CognitoClientId:
    Description: "App Client ID"
    Value: !Ref UserPoolClient
  WebsiteURL:
    Description: "URL trang web React của bạn (CloudFront)"
    Value: !GetAtt FrontendDistribution.DomainName
  S3BucketName:
    Description: "Tên Bucket để upload code Frontend"
    Value: !Ref FrontendBucket
```