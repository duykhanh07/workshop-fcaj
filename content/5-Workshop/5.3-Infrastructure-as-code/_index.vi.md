---
title : "Hạ tầng với SAM"
#date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

Thay vì phải click chuột thủ công hàng trăm lần trên AWS Console để tạo Database, Lambda, API Gateway... chúng ta sẽ sử dụng phương pháp **Infrastructure as Code (IaC)**.

Với **AWS SAM (Serverless Application Model)**, toàn bộ kiến trúc hệ thống được định nghĩa trong duy nhất một file: `template.yaml`. 

---

## 1. Cấu hình Toàn cục (Globals)

Để tránh lặp lại code (Don't Repeat Yourself), chúng ta định nghĩa các thông số chung cho tất cả các hàm Lambda trong phần `Globals`.

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

Với Java, RAM nhiều hơn đồng nghĩa với CPU mạnh hơn và thời gian khởi động (Cold Start) ngắn hơn. Kết hợp với tính năng SnapStart, ứng dụng sẽ phản hồi tức thì.

---

## 2. Định nghĩa Backend (Lambda Functions)
Mỗi tính năng của chúng ta tương ứng với một resource `AWS::Serverless::Function`

Bạn có thể xem các định nghĩa các lamda trong `template.yaml` [tại link github](https://github.com/duykhanh07/ai-career-coach)

Ví dụ với hàm **Cover Letter:**
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
Lưu ý quan trọng nhất đối với các hàm lambda là đoạn code chạy đúng logic nhưng vẫn bị lỗi AccessDenied? Đó là do Lambda mặc định không có quyền làm gì cả. Chúng ta phải cấp quyền cụ thể:
1. `bedrock:InvokeModel`: Cho phép Lambda gửi prompt đến mô hình Claude 3.
2. `bedrock:InvokeModelWithResponseStream`: Cho phép gọi mô hình theo kiểu streaming (dữ liệu trả về theo luồng — token từng phần). Dùng cho ứng dụng cần nhận output dần dần (streaming responses).
3. `aws-marketplace:ViewSubscriptions`: Cho phép xem trạng thái đăng ký các sản phẩm trên AWS Marketplace của tài khoản (liệt kê các subscription hiện có). Cần để kiểm tra xem tài khoản đã subscribe model bên thứ ba hay chưa.
4. `aws-marketplace:Subscribe`:Vì Claude 3 là mô hình của bên thứ 3 (Anthropic), Lambda cần quyền kiểm tra xem tài khoản AWS của bạn đã đăng ký (Subscribe) model này chưa. Thiếu quyền này, Bedrock sẽ từ chối phục vụ.
5. `aws-marketplace:Unsubscribe`: Cho phép hủy đăng ký (unsubscribe) một sản phẩm trên Marketplace.

---

## 3. Định nghĩa Database & Auth
DynamoDB (Single Table)
Chúng ta khai báo bảng với chế độ thanh toán PAY_PER_REQUEST (Serverless) - dùng bao nhiêu trả bấy nhiêu, không tốn phí duy trì khi không ai dùng.

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
Nơi quản lý người dùng. Chúng ta cấu hình AutoVerifiedAttributes: [email] để tự động gửi email xác thực khi đăng ký.

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
Đây là kiến trúc hosting hiện đại và bảo mật nhất cho Static Web hiện nay.

![S3 + CloudFront OAC](/images/5-Workshop/5.3-Infrastructure-as-code/cloudfront-oac.png)

Các thành phần:
* S3 Bucket: Nơi chứa file index.html, js, css. Chế độ PublicAccessBlock được bật (Chặn truy cập trực tiếp từ internet).
* CloudFront Distribution: CDN giúp cache nội dung và cung cấp HTTPS.
* Origin Access Control (OAC): Dùng OAC. Đây là vệ sĩ đứng giữa. Nó ký request từ CloudFront gửi sang S3.

Kết quả: Người dùng bắt buộc phải đi qua CloudFront (có HTTPS, có Cache). Nếu cố tình truy cập link S3 trực tiếp sẽ bị chặn (403 Forbidden).

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

## 6. Outputs (Kết quả đầu ra)
Sau khi deploy xong, CloudFormation sẽ trả về các thông số quan trọng để chúng ta cấu hình cho Frontend.

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