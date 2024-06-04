# HTTP 메시지
HTTP 애플리케이션 간에 주고받은 데이터의 블록

- 인바운드 : 메세지의 방향이 클라이언트에서 서버로 이동하는 것

- 아웃바운드 : 서버에서 모든 처리가 끝난 후 메시지가 클라이언트로 돌아오는 것

- 다운스트림 : 메세지의 흐름 
  - 모든 메세지는 다운스트림으로 흐름
- 업스트림 : 메세지의 발송자는 수신자의 업스트림

## 메시지 문법
### 요청 메시지
서버에게 리소스에 대해 무언가를 해달라고 부탁
- 메시지 형식
```
<메서드><요청 URL><버전>     // 시작줄

<헤더>

<엔티티 본문>
```
- 메서드 : 클라이언트 측에서 서버가 리소스에 대해 수행해주길 바라는 동작 
  - GET : 서버에서 어떤 문설르 가져옴 (메시지 본문 X)
  - HEAD : 서버에서 어떤 문서에 대해 헤더만 가져옴 (메시지 본문 X)
  - POST : 서버가 처리 해야할 데이터를 보냄 (메시지 본문 O)
  - PUT : 서버에 어떤 요청 메시지의 본문을 저장 (메시지 본문 O)
  - TRACE : 메시지가 프락시를 거쳐 서버에 도달하는 과정을 추적 (메시지 본문 X)
  - OPTIONS : 서버가 어떤 메서드를 수행할 수 있는지 확인 (메시지 본문 X)
  - DELETE : 서버에서 문서를 제거 (메시지 본문 X)
- 요청 URL : 요청 대상이 되는 리소스를 지칭하는 완전한 URL 혹은 URL 경로의 구성 요소
### 응답 메시지
클라이언트 요청에 대한 수행결과를 상태 정보, 결과 데이터를 포함하여 클라이언트에게 돌려줌
- 메시지 형식
```
<버전><상태코드><사유구절>        // 시작줄

<헤더>

<엔티티 본문>
```
- 상태 코드 : 요청 중에 무엇이 일어났는지 설명하는 세자리 숫자
- 사유 구절 : 상태 코드의 의미를 사람이 이해할 수 있게 설명하는 구절

### 공통 구성요소
- 버전 : HTTP 버전, 프로토콜의 버전을 알려주기 위해 사용
- 헤더 : 이름, 콜론, 선택적인 공백, 값, CRLF 가 순서대로 나타나는 0개 이상의 헤더들
  - 일반 헤더 : 요청, 응답 모두에 나타날 수 있음
  - 요청 헤더: 요청에 대한 부가 정보를 제공
  - 응답 헤더 : 응답에 대한 부가 정보를 제공
  - Entity 헤더 : 본문 크기와 콘텐츠 혹은 리로스 그 자체를 서술
  - 확장 헤더 : 명세에 정의되지 않은 새로운 헤더
- 본문 : 임의의 데이터 블록을 포함
  - 이미지, 비디오, html 문서, 소프트웨어 애플리케이션, 신용카드 트랜잭션 등 여러종류의 디지털 데이터를 실어 나를 수 있음

## 메서드
### 안전한 메서드
- HTTP는 안전한 메서드라 불리는 메서드의 집합을 정의함
- GET, HEAD 메서드의 사용이 서버에 어떤 작용도 없기 때문에 안전한 메서드
- POST 의 경우 서버에서 어떤 결과에 대한 작용이 일어남
- 안전한 메서드의 목적은 서버에 어떤 영향을 줄 수 있는 않전하지 않은 메서드가 사용될 때 사용자들에게 그 사실을알려주는 HTTP 애플리케이션을 만들 수 있도록 하는 것

### GET
- 가장 흔히 쓰이는 메서드
- 서버에 리소스를 요청하기 위해 사용

### HEAD
- GET 과 유사하지만 응답 메시지에 헤더만 있음
- 클라이언트가 리소스를 실제로 가져올 필요 없이 헤더만을 조사하기 위해 사용
  - 리소스를 가져오지 않고도 타입을 알아낼 수 있음
  - 응답의 상태 코드를 통해 개체가 존재하는지 확인
  - 헤더를 확인하여 리소스가 변경 됐는니 확인
- 반환되는 헤더가 GET 으로 얻는 것과 정확히 일치함을 보장해야 함

### PUT
- 서버에 문서를 씀
- 서버가 요청의 본문을 가지고 요청 URL의 이름대로 새 문서를 만들거나 이미 URL이 존재한다면 본문을 사용해서 교체
- 콘텐츠를 변경할 수 있게 해줌
### POST
- 서버에 입력 데이터를 전송하기 위해 설계
- HTML 폼을 지원하기 위해 흔히 사용
- 채워진 폼에 담긴 데이터는 서버로 전송되며 서버는 이를 모아서 필요로 하는 곳에 전달

### TRACE
- 클라이언트에게 자신의 요청이 서버에 도달했을 때 어떻게 보이게 되는지 알려 줌
- 요청 전송의 마지막 단계에 있는 서버는 자신이 받은 요청 메시지를 본문에 넣어 응답 메시지로 전달
- 진단을 위해 사용
  - 의도한 됴청, 응답 연쇄를 거쳐가는지 검사

### OPTIONS
- 웹 서버에게 여러 가지 종류의 지원 범위에 대해 물어봄
- 실제로 접근하지 않고도 리소스에 어떻게 접근하는 것이 최선인지 확인할 수 있는 수단을 클라이언트에게 제공

### DELETE
- 요청 URL로 지정한 리소스를 삭제할 것을 요청

### 확장 메서드
- HTTP/1.1 명세에 정의되지 않은 메서드

## 상태코드
클라이언트에게 그들의 트랜잭션을 이해할 수 있는 쉬운 방법을 제공

### 100-199: 정보성 상태코드
- 100(continue) : 요청 시작 부분 일부가 받아들여졌으며 클라이언트는 나머지를 계속 이어서 보내야 함을 의미
- 101(switching protocols) : 클라이언트가 upgrade 헤더에 나열한 것 중 하나로 서버가 프로토콜을 바꾸었음을 의미

1. 클라이언트와 100 Continue
- HTTP 클라이언트 애플리케이션이 서버에 엔티티 본문을 전송하기 전에 그 엔티티 본문을 받아들일 것인지 확인하려고 할 때 그 확인 작업을 최적화를 위한 것
- 서버가 다루거나 사용할 수 없는 큰 엔티티를 서버에게 보내지 않으려는 목적으로만 사용
- 클라이언트에서는 약간의 타임아웃 후에 엔티티를 보내야 함
2. 서버와 100 Continue
- 서버에서 100-continue 값이 담긴 expect 헤더가 포함된 요청을 받는다면, 100 continue 응답 혹은 에러 코드로 답해야 함
- 100 continue 응답을 받을 것을 의도하지 않은 클라이언트에게 100 continue 응답을 보내서는 안됨
- 서버가 요청을 끝까지 다 읽은 후에는 요청에 대한 최종 응답을 보내야 함
3. 프락시와 100 Continue
- 서버가 HTTP/1.1 을 따르거나 어떤 버전을 따르는지 모른다면 expect 헤더를 포함시켜서 요청을 다음으로 전달해야 함
- 서버가 HTTP/1.1 이전 버전을 따른다면 417 expectation failed 에러로 응답해야 함

### 200-299: 성공 상태 코드
- 200(ok) : 요청은 정상, 엔티티 본문은 요청된 리소스를 포함
- 201(created) : 서버 개체를 생성하라는 요청을 위한 것
- 202(accepted) : 요청은 받아들여졌으나 서버에서 요청에 대한 어떤 동작도 수행하지 않았다
- 203(non-authoritative-information) : 엔티티 헤더에 들어있는 정보가 원래 서버가 아닌 리소스 사본에서 왔다
- 204(no content) : 응답 메시지에 엔티티 본문은 포함하지 않음
- 204(reset content) : 브라우저에게 현재 페이지에 있는 html 폼에 채워진 모든 값을 비우라고 말함
- 205(partial content) : 부분 혹은 범위 요청이 성공

### 300-399: 리다이렉션 상태 코드
- 클라이언트가 관심있어 하는 리소스에 대해 다른 위치를 사용하라고 말해주거나 리소스의 내용 대신 다른 응답을 제공

### 400-499: 클라이언트 에러 상태 코드
- 400(bad request) : 클라이언트가 잘못된 요청을 보냄
- 403(forbidden) : 요청이 서버에 의해 거부되었음 알려줌 거절 이유를 엔티티 본문에 포함시킬 수 있지만 보통 서버가 거절의 이유를 숨기고 싶을 때 사용
- 404(not found) : 요청한 url을 찾을 수 없음
- 405(method not allowed) : 요청한 url에 대해 지원하지 않은 메서드로 요청 받은 경우
- 406(not acceptable) : 주어진 url에 대한 리소스 중 클라이언트가 받아들일 수 있는 것이 없는 경우 사용
- 408(request timeout) : 클라이언트 요청을 완수하기에 시간이 너무 많이 걸리는 경우
- 415(unsupported media type) : 서버가 이해하거나 지원하지 못하는 내용 유형의 엔티티를 클라이언트가 보냈을 때 사용
### 500-599: 서버 에러 상태 코드
- 서버 자체에서 에러가 발생하는 경우
- 500(internal server error) : 서버가 요청을 처리할 수 없게 만드는 에러를 만났을 때 사용
- 501(not implemented) : 클라이언트가 서버의 능력을 넘은 요청을 했을 때 사용
- 502(bad gateway) : 프락시나 게이트웨이처럼 행동하는 서버가 그 요청 응답 연쇄에 있는 다음 링크로부터 가짜 응답에 맞닥뜨렸을 때 사용
- 503(service unavailable) : 현재는 서버가 요청을 처리해 줄 수 없지만 나중에는 가능함을 의미하고자 할 때 사용
- 504(gateway timeout) : 다른 서버에게 요청을 보내고 응답을 기다리다 타임아웃이 발생한 게이트웨이나 프락시에서 온 응답
- 505(http version not supported) : 서버가 지원할 수 없거나 지원하지 않으려고 하는 버전의 프로토콜로 된 요청을 받았을 때 사용

## 헤더
메서드와 함께 클라이언트와 서버가 무엇을 하는지 결정하기 위해 함께 사용
### 일반 헤더
- 클라이언트와 서버 양쪽 모두가 사용
- 메시지에 대한 아주 기본적인 정보를 제공
  - connection : 클라이언트와 서버가 요청/응답 연결에 대한 옵션으 정할 수 있게 해줌
  - date : 메시지가 언제 만들어졌는지에 대한 날짜와 시간을 제공
- 일반 캐시 헤더
  - cache-contral : 메시지와 함께 캐시 지시자를 전달하기 위해 사용
  - pragma : 메시지와 함께 지시자를 전달하는 또 다른 방법
### 요청 헤더
- 요청 메시지에서만 의미를 갖는 헤더
- 요청이 최초 발생한 곳에서 누가 혹은 무엇이 요청을 보냈는지에 대한 정보나 클라이언트의 선호, 능력에 대한 정보를 서버에 전달
  - client-ip : 클라이언트가 실행된 컴퓨터의 ip를 제공
  - from : 사용자의 메일 주소를 제공
  - host : 요청의 대상이 되는 서버의 호스트 명과 포트 제공
  - referer : 현재 요청 URI 가 들어있었던 문서의 URL 제공
1. Accept 관련 헤더
- 서버에게 자신의 선호와 능력을 알려 줄 수 있음
  - accept : 서버가 보내도 되는 미디어의 종류
  - accept-chaarset : 서버가 보내도 되는 문자 집합
  - accept-encoding : 서버가 보내도 되는 인코딩
  - accept-language : 서버가 보내도 되는 언어
  - TE : 서버가 보내도 되는 확장 전송 코딩
2. 조건부 요청 헤더
- 서버에게 요청에 응답하기 전에 먼저 조건이 참인지 확인하게 하는 제약을 포함
3. 요청 보안 헤더
- 클라이언트가 어느 정도의 리소스에 접근하기 전에 자신을 인증하게 함으로써 트랜잭션을 약간 더 안전하게 만듦
  - authorization : 클라이언트가 서버에게 제공하는 인증 그 자체에 대한 정보를 담고 있음
  - cookie : 클라이언트가 서버에게 토큰을 전달할 때 사용

### 응답 헤더
- 클라이언트에게 부가 정보를 제공
- 누가 응답을 보내고 있는지 혹은 응답자의 능력은 어떻게 되는지 알려 줌
1. 협상 헤더
- 서버와 클라이언트가 어떤 표현을 택할 것인가에 대한 협상을 할 수 있도록 지원
2. 응답 보안 헤더

### 엔티티 헤더
- 엔티티와 내용물에 대한 개체의 타입, 주어진 리소스에 대해 요청할 수 있는 유효한 메서드 등에 대한 정보를 제공
1. 콘텐츠 헤더
- 엔티티의 콘텐츠에 대한 구체적인 정보를 제공
- 콘텐츠의 종류, 크기 등
  - content-encoding : 본문에 적용된 인코딩
  - content-language : 본문을 이해하는데 가장 적절한 자연어
  - content-length : 본문의 길이나 크기
  - content-type : 본문이 어떤 종류의 객체인지
2. 엔티티 캐싱 헤더
- 엔티티 캐싱에 대한 정보를 제공