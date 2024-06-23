# 내용 협상과 트랜스코딩

## Vary 헤더
 - Vary 헤더는 HTTP 응답 헤더 중 하나로, 캐시 관리를 위해 사용되는 중요한 요소
 - 캐시 결정을 내릴 때 고려해야 할 요청 헤더를 지정
 - 같은 URL 요청이라도 다른 Vary 헤더를 포함하는 요청에 대해서는 별도의 캐시를 유지해야 함

### 기능
1. 캐시 제어 : 캐시 서버는 Vary 헤더를 참고하여 동일한 URL에 대해 다른 요청 헤더 값을 가진 요청이 올 때 다른 응답을 저장
2. 콘텐츠 협상 : 서버는 클라이언트의 요청 헤더(예: Accept, Accept-Encoding, User-Agent 등)에 따라 다른 콘텐츠를 제공

### 예시
1. Vary: User-Agent
- 클라이언트의 브라우저 종류에 따라 다른 HTML을 제공할 때 사용
2. Vary: Accept-Encoding
- 클라이언트의 Accept-Encoding 헤더 값에 따라 압축된 응답(gzip, deflate 등)을 제공할 때 사용
3. Vary: Accept-Language
- 클라이언트의 언어 설정에 따라 다른 언어의 콘텐츠를 제공할 때 사용
4. 결합 사용 
- Vary: Accept-Encoding, User-Agent

### 효과
- 캐시 효율성을 높이고, 클라이언트 요청에 맞는 정확한 응답을 제공함으로써 다양한 사용자 환경에 적합한 콘텐츠를 동적으로 제공