## 보안관련 쿠키속성

### **Secure**

- 쿠키가 HTTPS 연결에서만 전송되도록 설정하는 속성
- HTTP 인 경우는 전송되지 않음
- `Set-Cookie: sessionId=abc123; Secure`

### **HttpOnly**

- JavaScript를 통해 접근할 수 없도록 설정하여 XSS 공격 방지
- 서버와의 HTTP/HTTPS 요청에서만 전송 됨
- `Set-Cookie: sessionId=abc123; HttpOnly`

### **SameSite 속성**

- Cross-site 요청에서의 쿠키가 전송되지 않도록 하는 속성
- 외부에서 기능 사용이 제한됨으로써 CSRF 공격을 방지할 수 있다
- SameSite 옵션
    - None
        - 쿠키는 모든 요청과 함께 전송됨
        - secure 속성도 함께 설정해야 함
        - `Set-Cookie: sessionId=abc123; SameSite=None; Secure`
          
    - Lax
        - 대부분의 동일 사이트 요청에만 쿠키 전송
        - POST 요청일때는 쿠키 전송되지 않음
        
    - Strict
        - 동일한 사이트에서 생성된 요청에만 쿠키가 전송됨
        - 다른 사이트에서 발생한 모든 요청에는 쿠키가 전송되지 않음
        - 보안이 가장 엄격한 설정으로, CSRF 공격을 방지하는 데 매우 효과적
