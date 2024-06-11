# 15장. 엔터티와 인코딩

## ETag
### ETag 정의
- ETag HTTP 응답 헤더는 특정 버전의 리소스를 식별하는 식별자
- 서버는 리소스가 변경될 때마다 새로운 ETag를 생성하여 클라이언트에게 제공함 
- 웹 서버가 내용을 확인하고 변하지 않았으면, 웹 서버로 full 요청을 보내지 않기 때문에, 캐쉬가 더 효율적이게 되고, 대역폭도 아낄 수 있음


### ETag 관련 링크
- [ETag 동작 방식](https://tecoble.techcourse.co.kr/post/2020-09-30-ETag-with-Spring/)
- [ETag를 이용하여 더 나은 Restful API 만들기]( https://yozm.wishket.com/magazine/detail/1772/)

### Cache-Control(캐시 제어) 정의
- **브라우저 캐싱 동작을 지시하는 HTTP 헤더**
- 간단히 말해서 누군가가 웹 사이트를 방문하면 브라우저는 이미지 및 웹 사이트 데이터와 같은 특정 리소스를 캐시라는 저장소에 저장함
- 해당 사용자가 동일한 웹 사이트를 다시 방문할 때 Cache-Control은 해당 사용자가 해당 리소스를 로컬 캐시에서 로드할 것인지 아니면 브라우저가 새로운 리소스에 대한 요청을 서버에 보내야 하는지 여부를 결정하는 규칙을 설정함
- 즉, **Cache-Control 헤더는 클라이언트와 서버가 리소스를 어떻게 캐싱할지를 결정하는 데 사용**되며 다양한 지시어(directive)를 통해 캐시 동작을 세밀하게 제어할 수 있음


### Cache-Control과 ETag 헤더

- Cache-Control과 ETag 헤더는 모두 HTTP 응답 헤더의 일부로, 브라우저에서 캐싱을 처리하는 데에 사용됨
- 이 두 헤더는 서로 독립적으로 사용되지만, 함께 사용될 수도 있음

## 참고 링크
- [Etag란](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/ETag)
- [Cache-Control과 ETag 헤더](https://yozm.wishket.com/magazine/questions/share/i6MB9h5hu0AXnNBI/)
- [Cache-Control이란](https://www.cloudflare.com/ko-kr/learning/cdn/glossary/what-is-cache-control/)
