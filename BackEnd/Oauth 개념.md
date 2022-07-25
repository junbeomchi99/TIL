# OAuth
= 인증을 위한 Open Standard Protocol
= 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여 할 수 있는 공통적인 수단으로 사용되는, 접근 위임을 위한 개방형 표준이다
- e.g 외부 어플리케이션(원티드)는 사용자 인증을 위해 Facebook/Google 등의 사용자 인증 방식 사용 
    - OAuth를 바탕으로 제 3자 서비스(원티드)는 외부 서비스 (Facebook,Google)의 특정 자원을 접근 및 사용할 수 있는 권한을 인가 받음

## OAuth 참가자
크게 3가지로 구분 할 수 있음
- Resource Server: Client가 제어하고자 하는 자원을 보유하고 있는 서버 
    - Facebook, Google등
- Resrouce Owner: 자원의 소유자
    - Client가 제공하는 서비스를 통해 로그인하는 실제 유저가 이에 속함
- Client: Resoruce Server에 접속해서 정보를 가져오고자 하는 클라이언트 (웹 애플리케이션)
    - e.g. 원티드

# OAuth Flow
- Github 계정을 바탕으로 웹 애플리케이션에서 로그인, Github에서 제공하는 여러 API기능 외부 웹 어플리케이션 사용하는 예시 
1. Client 등록
- Client가 Resource Server를 이용하기 위해서는 자신의 서비스를 등록하여 사전 승인을 받아야됨
    - Github Developer Settings에서 Github에 웹 어플리케이션 등록
    - Client ID, Client Secret (Client ID 비밀키), Authorized redirect URL (Authorization Code 전달받을 주소) 전달 받음
2. Resource Owner의 승인
- Resource Server에서 명시한 주소로 Get 요청과 필요 파라미터 보냄
    - GET https://github.com/login/oauth/authorize?client_id={client_id}&redirect_uri={redirect_uri}?scope={scope}
    - Resource Owner (사용자)는 Resource Server에 접속하여 로그인 실행 
        - 파라미터로 전달된 Client ID와 동일한 ID값이 존재하는지 확인
        - 해당 Client ID에 해당하는 Redirect URL이 파라미터로 전달된 Redirect URL과 같은지 확인
3. Resource Server의 승인
- Redirect URL로 클라이언트를 리다이렉트 시킴
- Resource Server는 Client가 자신의 자원을 사용할 수 있는 Acccess Token을 발급하기전, 임시 암호인 Authorization Code를 함께 발급
- Client는 ID와 비밀키 및 code를 Resource Owner를 거치지 않고 Resource Server에 직접 전달함 
- Resource Server는 정보를 검사한 다음, 유효한 요청이라면 Access Token을 발급
- Client는 해당 토큰을 서버에 저장, Resource Server의 자원을 사용하기 위한 API 호출시 해당 토큰을 헤더에 담아 보냄
4. API 호출
- AccessToken를 헤더에 담아 API를 호출되면, 해당 계정과 연동된 Resource Server의 풍부한 자원 및 기능들을 내 웹 애플리케이션에서 사요 ㅇ가능
5. Refresh Token
- AccessToken은 만료기간이 있기때문에, AccessToken이 만료되었을때 Refresh Token을 보내 새로운 Access 토큰을 발급 받음

## 카카오 로그인 연동 Flow


![](2022-07-20-17-27-45.png)