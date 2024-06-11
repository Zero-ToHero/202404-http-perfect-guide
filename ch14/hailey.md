# 보안 HTTP

## 디지털 서명
- 일반적으로 해싱, 서명, 검증 이 세 가지 알고리즘으로 이루어진 전자 서명의 일종
1. 개인키와 공개키 생성
- 보내야 할 데이터, 문서, 메시지 등을 해시 알고리즘으로 암호화하여 개인키와 공개키를 생성 
- 데이터는 해시 함수를 사용하여 암호화되기 때문에 데이터의 길이에 상관없이 고정된 크기의 해시를 생성
- 개인키와 이에 상응하는 공개키를 생성
- 단 1비트라도 변하게 된다면 완전 다른 해시값을 내놓기 때문에 역산해서 암호를 푸는 것은 사실상 불가능함.

2. 서명
- 서명은 데이터나 문서에 발신자의 개인키가 포함됨으로써 서명을 하게 됨
- 발신자는 수신자에게 개인키에 부합하는 공개키를 함께 보냄
- 데이터가 해시화되었기 때문에 메시지 별로 모두 다른 디지털 서명을 갖게 됨

3. 검증
- 수신자는 발신자가 보낸 공개키를 통해 디지털 서명의 유효성을 확인

![image](https://github.com/hwyi21/202404-http-perfect-guide/assets/58624211/0a47adc7-52d8-451b-b70f-37a41d46b714)

### 서버 인증서
- HTTPS에서 무조건 요구
![image](https://github.com/hwyi21/202404-http-perfect-guide/assets/58624211/f8ff3000-563f-4a03-9498-0d8b34c65108)
1. 구성 요소
  - 공개 키(Public Key): 클라이언트가 서버와 안전하게 통신하기 위해 사용하는 키
  - 서명(Signer): 인증 기관(CA)의 디지털 서명으로, 인증서의 진위 여부를 보증.
  - 소유자의 정보(Subject Information): 서버 인증서가 발급된 웹 사이트의 도메인, 조직명 등의 정보가 포함.
  - 유효 기간(Validity Period): 인증서가 유효한 기간으로, 시작일과 만료일을 포함.
  - 인증 기관 정보(Issuer Information): 인증서를 발급한 인증 기관(CA)의 정보
  - 확장 정보(Extensions): 추가적인 사용 조건이나 정책 등의 정보가 포함

2. 발급 과정
- 인증 기관(CA)에 의해 발급됨.
- CSR 생성(Certificate Signing Request): 웹 서버의 관리자 또는 소유자는 공개 키와 소유자의 정보를 포함한 CSR을 생성.
- CSR 제출: 생성된 CSR을 인증 기관에 제출.
- 인증 기관의 검증: 인증 기관은 제출된 정보를 검토하고, 소유자의 신원을 검증.
- 인증서 발급: 검증이 완료되면 인증 기관은 서명된 서버 인증서를 발급.
- 서버에 설치: 발급된 서버 인증서를 웹 서버에 설치하고, SSL/TLS 설정을 완료.

3. NGINX 서버 설치 방법
```
sudo nano /etc/nginx/sites-available/default    // 설정 파일 열기

server {
    listen 443 ssl;
    server_name your_domain.com;

    ssl_certificate /path/to/your_domain_name.crt;
    ssl_certificate_key /path/to/your_private.key;
    ssl_trusted_certificate /path/to/CA_bundle.crt;
}

sudo systemctl restart nginx     // 재시작
```

![image](https://github.com/hwyi21/202404-http-perfect-guide/assets/58624211/39a30893-f94b-4d76-8bb1-afd90862020f)