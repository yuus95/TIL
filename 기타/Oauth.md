# Oauth 

## Oauth란?

```bash
서버를 구축할 때, 로그인 개인정보 관리 책임을 서드파티 애플리케이션(Google,Facebook,Kakao)등에게 위임할 수 있다.(단 
사용자가 기존에 서드파티 서비스에 회원가입이 되어 있어야함)

>> 구글만 가입하면 다양한 애플리케이션에서 각각 사용자가 권한을 부여 받을 필요 없이 구글로부터 부여 받은 권한을 다양한 애플리케이션에서 행사할 수 있다. 
```

## Oauth 요소

```bash
Client
>> Oauth2.0을 사용해 서드파티 로그인 기능을 구현할 자사 또는 개인 애플리케이션 서버다.

Resource Owner
>>서드파티 애플리케이션에 이미 개인정보를 저장하고 있으며 Client가 제공하는 서비스를 이용하려는 사용자. (Resource는 개인정보라고 생각하면 된다.)

Resource Server
>> 사용자의 개인정보를 가지고있는 애플리케이션 서버다. Client는 Token을 이 서버로 넘겨 개인정보를 응답받을 수 있다.

Authorization Server
>> 권한을 부여(인증에 사용할 아이템을 제공)해주는 서버다.
사용자는 이 서버로 ID,PW를 넘겨 Authorization Code를 발급 받을 수 있다.
Client는 이 서버로 Authorization Code를 넘겨 Token을 발급 받을 수 있다. 
```


## OAuth Flow

```bash
1. Client등록 
>> Client(웹 어플리케이션)가 Resources Server를 이용하기 위해서는 자신의 서비스를 등록함으로써 사전 승인을 받아야 합니다.

2. Resource Owner의 승인
>> Resource Owner는 Client의 어플리케이션에 로그인하기 위해
Resource Server에 로그인을 합니다.  이 때 Resource Server는
Client에게 정보를 제공해도 되냐고 물어봅니다. 

3. Resource Server의 승인
>> Resource Owner의 승인이 마무리 되면 명시된 Redirect URI로 클라이언트를 리다이렉트를 시킵니다. 이 때 Resource Server는 Client가 자신의 자원을 사용할 수 있는 Access Token을 발급 하기 전에, 임시 암호인 Authorization Code를 함께 쿼리스트링으로 제공합니다. 

4.Client는  API요청으로 쿼리스트링을 Resource Server에 전달합니다.
Resource Server는 정볼르 검사한 다음, 유효한 요청이라면 Access Token을 발급해줍니다.

```