## 1.2 웹 클라이언트와 서버
- 웹서버는 인터넷의 데이터를 저장하고 HTTP 클라이언트가 요청한 데이터를 제공한다.

## 1.3 리소스
- 웹 리소스로는 웹 서버 파일 시스템의 정적파일이나 요청에 따라 다르게 생성되는 동적 콘텐츠가 있다.

### 미디어 타입

- MIME 타입 : 데이터 포맷 라벨
    - 원래 각기 다른 전자메일 시스템 사이에서 메세지가 오갈때 겪는 문제점을 해결하기 위해 설계되었다.
    - `/` 으로 구분된 primary object type과 specific subtype 으로 이루어진 문자열 라벨이다.
    - MIME 타입 종류
        - HTML → `text/html`
        - Plain ASCII → `text/plain`
        - JPEG → `image/jpeg`
        - GIF → `image/gif`
        - JSON 형식 → `application/json`
    - 웹서버는 모든 HTTP 객체 데이터에 MIME 타입을 붙인다.
- 웹클라이언트는 서버로부터 객체를 돌려 받을 때, 다룰 수 있는 객체인지 MIME 타입을 통해 확인한다.
  
### URI

- URI (Uniform resource identifier) : 통합 자원 식별자로 서버 리소스의 이름
    - URI는 정보 리소스를 고유하게 식별하고 위치를 지정할 수 있다.
    - URL : 특정 서버의 리소스에 대한 구체적인 위치
        - 대부분의 URI는 URL이다
        - 세 부분으로 이루어진 URL의 표준 포맷
            - URL의 첫 번째 부분은 scheme이라고 하고 보통 HTTP 프로토콜 `http://`
            - 두 번째 부분은 서버의 인터넷 주소 `www.joes-hardware.com`
            - 마지막은 웹서버의 리소스 `/specials/saw-blad.gif`
    - URN : 콘텐츠를 이루는 한 리소스에 대해, 그 리소스의 위치에 영향을 받지 않는 유일 무이한 이름역할을 한다.
        - 위치 독립적인 URN은 리소스를 여기저기로 옮기더라도 문제 없이 동작한다.
    

## 1.4 트랜젝션

- HTTP 트랜젝션은 요청 명령(클라이언트 → 서버)과 응답결과(서버 → 클라이언트)로 구성되어 있다.

### 메서드

- HTTP 메서드는 서버에게 어떤 동작이 취해져야 하는지 말해준다

### 상태코드

- 상태코드 : 클라이언트에게 요청이 성공했는지 아니면 추가 조치가 필요한지 알려주는 세 자리 숫자
- HTTP는 각 숫자 상태코드에 텍스트로 된 reaons phrase도 함께 보내는데 이 구문은 설명을 위해 포함된 것이므로 실제 응답 처리는 숫자로 된 상태코드로 해야한다.


## 1.5 메시지

- HTTP 요청과 응답 메세지의 구조

- 시작줄
    - 요청이라면 무슨일을 해야하는지(명령), 응답이라면 무슨일이 일어났는지(상태) 나타낸다
- 헤더
    - 헤더 필드는 쉬운 구문분석을 위해 : 으로 구분되어 있는 이름과 값으로 구성된다.
    - 헤더는 빈 줄로 끝난다
- 본문
    - 어떤 종류의 데이터든 들어갈 수 있는 메세지 본문이 필요에 따라 올 수 있다.
    

## 1.6 TCP 커넥션

### TCP/IP
- TCP/IP는 패킷 통신 방식의 인터넷 프로토콜인 IP와 전송 조절 프로토콜인 TCP로 이루어져있다. TCP는 IP 위에서 동작하고 TCP를 기반으로 한 HTTP, FTP, SMTP 같은 애플리케이션 프로토콜들이 IP 위에서 동작하므로 묶어서 TCP/IP라고 부른다.

### 접속, IP 주소 그리고 포트 번호

- HTTP 클라이언트가 서버에 메세지를 전송하려면 IP 주소와 포트 번호를 사용해서 클라이언트와 서버 사이에 TCP/IP 커넥션을 맺어야 한다.
- HTTP URL에 포트번호가 빠진 경우는 기본값 80이라고 가정하면 된다.

## 1.7 프로토콜 버전
- HTTP/1.0
 - 헤더, 버전 관리, 응답에 상태코드, content-type, HTTP 메소드 (POST, HEAD)가 추가됨

- HTTP/1.1
 - 호스트 헤더
  - 클라이언트에서 호스트 서버를 식별하는 데 사용된다.
- 영구 연결 : 1.0 에서는 요청할 때마다 새로운 커넥션을 열어야했음. 1.1에서는 TCP 연결을 한번 열면 계속 사용할 수 있음
  <img width="451" alt="image" src="https://github.com/heeom/202404-http-perfect-guide/assets/64389364/fbf96d32-0aab-46f9-8748-8665029626b7">
    - HTTP1.0 은 TCP 세션을 유지하지 않고, HTTP 1.1은 TCP 세션을 유지한다.
-  continue status : 서버가 요청을 거부하는것을 방지하기 위해 클라이언트에서는 요청헤더만 먼저 보내고 상태코드 100을 수신하는지 확인 할 수 있다.
  - 클라이언트가 POST 요청을 보냈을 때 서버가 요청을 수락했음을 알리기위해 100 Continue 상태코드를 보낸다 → 클라이언트에서는 100을 받으면 요청 본문을 보낸다.
- PUT, PATCH, DELETE, CONNECT, TRACE 및 OPTIONS HTTP 메서드가 추가됐다. 

## 1.8 웹의 구성요소

- 프록시
    - 클라이언트와 서버 사이에 위치한 HTTP 중개자
    - 클라이언트와 서버사이에 위치하여, 클라이언트의 모든 HTTP 요청을 받아서 서버에 전달한다.
- 캐시
    - 자주 찾는 문서의 사본을 저장해두는 HTTP 프락시 서버
- 게이트웨이
    - 다른 서버들의 중개자로 동작하는 서버. 게이트웨이는 HTTP 트래픽을 다른 프로토콜로 변환하기 위해 사용된다.
- 터널
    - 두 커넥션 사이에서 raw데이터를 열어보지 않고 그대로 전달해주는 HTTP 애플리케이션이다.
    - 암호화된 SSL 트래픽을 HTTP 커넥션으로 전송함으로써 웹트래픽만 허용하는 사내 방화벽을 통과시키는 것이 있다.
- 에이전트
    - HTTP 요청을 만들어주는 클라이언트 프로그램