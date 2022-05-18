

```
//사용방법
- Deployment:
    - serverless deploy 

- Invocation
    - serverless invoke local --function hello
    - serverless invoke --function hello
```	



## What is Amazon API Gateway?

```
Amazon API Gateway is a managed service that allows developers 
to define the HTTP endpoints of a REST API or a WebSocket API 
and connect those endpoints
 with the corresponding backend business logic.
WebSocker aPI또는 Http endPoint를 개발자들이 정의할 수 있도로 
관리해주는 서비스

API Gateway integrates with many other AWS services 
like AWS Lambda, AWS SNS, AWS IAM, 
and Cognito Identity Pools. These integrations allow for fully managed authentication and authorization layers, as well as detailed metrics and tracing for API requests.

api gateway 다른 aws 서비스와 함께 사용될 때고객 대면 웹서비스를 기능적으로
완벽하게 대응할 수 있다
```