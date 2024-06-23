## Quality Value

- 클라이언트가 서버에 특정 콘텐츠 타입, 언어, 문자 인코딩 또는 압축 형식을 얼마나 선호하는지를 나타내기 위해 사용
- q값은 클라이언트가 여러 가지 대안을 제시할 때, 그 선호도의 우선순위를 서버에 전달하는 역할
- q값은 0.0에서 1.0까지의 값을 가지고, 기본값은 1.0 (명시하지 않았을때도 1.0)
- 클라이언트는 각 항목에 대해 q값을 지정할 수 있으며, 이 값이 높을수록 그 항목의 선호도가 높음

### q값 적용 예시

```jsx
GET /resource HTTP/1.1
Host: example.com
Accept: text/html, application/json;q=0.9
```

- 서버는 클라이언트의 Accept 헤더를 검사하여 HTML을 우선적으로 제공하고, HTML이 제공되지 않는 경우에만 JSON을 제공한다.

```jsx
GET /index HTTP/1.1
Host: example.com
Accept-Language: en-US, fr;q=0.8, es;q=0.6
```

- 서버는 클라이언트의 Accept-Language 헤더를 검사하여 미국 영어 페이지를 우선적으로 제공하고, 그 다음 프랑스어, 마지막으로 스페인어 페이지를 제공한다.

### Spring MVC에서 컨텐츠 협상

- 요청의 컨텐츠 타입 지정하는 방법 (순서대로 우선순위)
    - 요청에 URL 접미사 (확장자) 사용 (deprecated)
        - `.xml` `.json`
    - URL 매개 변수 사용
        - `?mediaType=json`
    - Accept 헤더 사용

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
        configurer.favorParameter(true) // url 매개변수 사용
                .parameterName("mediaType") 
                .ignoreAcceptHeader(false) // Accept 헤더
                .defaultContentType(MediaType.APPLICATION_JSON)
                .mediaType("json", MediaType.APPLICATION_JSON)
                .mediaType("xml", MediaType.APPLICATION_XML);
    }
}
```

```java
@RestController
public class TestController {

    @Autowired
    private UserService userService;

    @GetMapping(value = "/user/{id}", produces = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})
    public UserData getUser(@PathVariable Long id) {
        User user = userService.getUser(id);
        return new UserData(user.getId(), user.getName(), user.getEmail());
    }
}
```

### URL 매개변수 사용
![image](https://github.com/heeom/202404-http-perfect-guide/assets/64389364/09ad7b53-12ec-4933-94c4-6b8f3f14ad7e)
![image](https://github.com/heeom/202404-http-perfect-guide/assets/64389364/44ecaf95-0948-44b3-b359-dd8d9e90290b)



### Accept 헤더 사용

![image](https://github.com/heeom/202404-http-perfect-guide/assets/64389364/1823b580-79f8-4484-8a8b-951e6e984d8c)
![image](https://github.com/heeom/202404-http-perfect-guide/assets/64389364/bf756aa8-77b1-49d4-bc13-be0e010b33d6)


### produces 옵션

- `produces` 옵션을 사용하여 응답의 미디어 타입을 JSON과 XML로 지정

 `@RequestMapping`, `@GetMapping`, `@PostMapping` 등의 어노테이션에서 사용될 수 있다.

- 이 옵션은 해당 핸들러 메서드가 생성할 수 있는 미디어 타입을 나타낸다.
- 클라이언트의 `Accept` 헤더와 비교하여 가장 적합한 미디어 타입을 선택하는 내용 협상(Content Negotiation)의 일부로 사용됨
- 예를 들어, 클라이언트의 `Accept` 헤더가 `application/json;q=0.7, application/xml;q=0.3`인 경우, Spring은 이 정보를 기반으로 JSON과 XML 중에서 적합한 타입을 선택하여 응답.
- 기본적으로는 q값이 높은 순서대로 우선순위가 결정된다.

### Accept 헤더 사용할 때 서버의 q값 처리로직

1. 클라이언트가 보낸 헤더들을 확인
2. 각 헤더의 q값을 추출하고, q값이 높은 순서대로 항목들을 정렬
3. 서버가 지원하는 항목과 클라이언트의 선호도를 비교하여 가장 적절한 응답을 선택
4. 선택된 항목에 따라 클라이언트에게 응답을 반환한다.

```java
/**
 * A {@code ContentNegotiationStrategy} **that checks the 'Accept' request header**.
 *
 * @author Rossen Stoyanchev
 * @author Juergen Hoeller
 * @since 3.2
 */
public class HeaderContentNegotiationStrategy implements ContentNegotiationStrategy {

	/**
	 * {@inheritDoc}
	 * @throws HttpMediaTypeNotAcceptableException if the 'Accept' header cannot be parsed
	 */
	@Override
	public List<MediaType> resolveMediaTypes(NativeWebRequest request)
			throws HttpMediaTypeNotAcceptableException {

		String[] headerValueArray = request.getHeaderValues(HttpHeaders.ACCEPT);
		if (headerValueArray == null) {
			return MEDIA_TYPE_ALL_LIST;
		}

		List<String> headerValues = Arrays.asList(headerValueArray);
		try {
			List<MediaType> mediaTypes = **MediaType**.**parseMediaTypes**(headerValues);
			**MimeTypeUtils**.**sortBySpecificity**(mediaTypes);
			return !CollectionUtils.isEmpty(mediaTypes) ? mediaTypes : MEDIA_TYPE_ALL_LIST;
		}
		catch (InvalidMediaTypeException | InvalidMimeTypeException ex) {
			throw new HttpMediaTypeNotAcceptableException(
					"Could not parse 'Accept' header " + headerValues + ": " + ex.getMessage());
		}
	}

}
```

- `MediaType**.**parseMediaTypes**(String acceptHeader)` : Accept 헤더를 MediaType 리스트로 파싱
- `MimeTypeUtils**.**sortBySpecificity**(List<MediaType> mediaTypes)` : q값 기준으로 MediaType 정렬
![image](https://github.com/heeom/202404-http-perfect-guide/assets/64389364/a8f39c13-a5b4-4fa9-8793-f8b2e1f67b74)
![image](https://github.com/heeom/202404-http-perfect-guide/assets/64389364/9cbd8606-4814-4aa3-aa23-bae19bab3fbb)

