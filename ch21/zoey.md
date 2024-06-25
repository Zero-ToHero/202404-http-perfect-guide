### 로깅 항목

- HTTP 메서드
- 클라이언트와 서버의 HTTP 버전
- 요청받은 리소스의 URL
- 응답의 HTTP 상태 코드
- 요청과 응답 메시지의 크기
- 트랜잭션이 일어난 시간
- Referer와 User-Agent 헤더 값

### 로그 포맷

| remotehost | 요청한 컴퓨터의 호스트 명 혹은 IP주소 |
| --- | --- |
| username | ident 검색을 수행했다면, 인증된 요청자의 사용자 이름 |
| auth-username | 인증을 수행했다면, 인증된 요청자의 이름 |
| teimstamp | 요청 날짜와 시간 |
| request-line | HTTP 요청의 행을 그대로 기술 |
| response-code | 응답으로 보내는 HTTP 상태 코드 |
| response-size | 응답 엔터티의 Content-Length, 아무런 엔터티를 반환하지 않으면 값이 0이 된다. |
