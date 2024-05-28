## SSO (Single sign-on)

- Single Sign-On(SSO)은 1회 사용자 인증으로 다수의 애플리케이션 및 웹사이트에 대한 사용자 로그인을 허용하는 인증 솔루션
- SSO 프로토콜
    - SAML(Security Assertion Markup Language)
        - XML 포맷을 사용하여 인증 및 권한 정보를 교환
    - OIDC (OpenID Connect)
        - JWT를 사용하여 인증 정보 교환

### SSO 프로세스 (SAML)

![image](https://github.com/heeom/202404-http-perfect-guide/assets/64389364/8af8f5dd-14c7-4436-bf03-e65e33177183)


1. 사용자 요청 
    1. 사용자가 서비스 제공자에 접근 → 이메일 주소를 입력한다
2. 서비스 제공자가 SAR 전송
    1. 인증되지 않은 사용자이므로 SAR (SAML Authentication Request)을 생성해서 브라우저로 전송한다.
3. 브라우저는 Identity Provider로 SAR 전달한다.
4. Identity Provider는 SAR을 받고, 인증되지 않은 사용자이면 로그인 페이지를 보여줌
5. 사용자는 로그인 페이지에서 자신의 자격 증명(사용자 이름과 비밀번호)을 입력하여 Identity Provider로 전송
6. 브라우저로 SAML Assertion 반환
    1. IdP는 사용자의 자격 증명을 검증한 후, 사용자가 인증되었음을 나타내는 서명된 SAML Assertion을 생성해서 브라우저로 반환
7. 서비스 제공자에게 서명된 SAML Assertion 전달
8. SAML Assertion 검증
    1. 서비스 제공자는 받은 SAML Assertion을 검증한다. 검증 과정에서 Assertion의 서명이 유효한지 확인하고, Assertion에 포함된 인증 정보를 확인합니다.
    2. 검증이 성공하면, 사용자를 인증된 상태로 처리하고, 서비스에 접근을 허용한다.
