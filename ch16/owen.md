# 서버의 다국어 처리
# **Locale 이란?**

자바에서 Locale 이란 나라, 언어 등 지리적/문화적으로 분리된 지역들에 대한 정보를 담고있는 객체다.  Locale-sensitive 한 서비스는 여러 방법으로 클라이언트의 locale 을 얻고/저장하고/불러와야 한다. accept-language 헤더, request params, cookie / session 등의 방법이 쓰일 수 있다

# **LocaleResolver**

request 로부터의 locale resolution 과 request-response 간 locale 수정 전략을 선언한 인터페이스.

`AcceptHeaderLocaleResolver`는 `LocaleResolver` 를 구현하며, 다음과 같이 Http 헤더의 'accept-language'에 담겨오는 locale 정보를 얻어온다.

![image](https://github.com/Zero-ToHero/202404-http-perfect-guide/assets/71249347/b21611e3-35a2-426b-ac06-6ddbcde86df8)


# **LocaleContextHolder**

`localeResolver` 메서드엔 locale을 어디 저장하는 로직이 없지만, 스프링에선 리졸빙한Locale을 `LocaleContextHolder`에 저장한다

```java
...
    @Override
    protected LocaleContext buildLocaleContext(final HttpServletRequest request) {
        LocaleResolver lr = this.localeResolver;
        if (lr instanceof LocaleContextResolver) {
            return ((LocaleContextResolver) lr).resolveLocaleContext(request);
        }
        else {
            return () -> (lr != null ? lr.resolveLocale(request) : request.getLocale());
        }
    }
...
 
// FrameworkServlet.java
// initContextHolders: localeConetext를 받아서 LocaleContextHolder에 저장
...
    private void initContextHolders(HttpServletRequest request,
            @Nullable LocaleContext localeContext, @Nullable RequestAttributes requestAttributes) {
 
        if (localeContext != null) {
            LocaleContextHolder.setLocaleContext(localeContext, this.threadContextInheritable);
        }
        if (requestAttributes != null) {
            RequestContextHolder.setRequestAttributes(requestAttributes, this.threadContextInheritable);
        }
    }
...
```

# MessageSource

SpringBoot 사용시 MessageSource에 대해 자동으로 스프링 빈으로 등록한다. 별도의 설정을 하지 않으면 message 라는 이름으로 기본적으로 등록됨. 따라서 message_en.properties 등을 통해 다국어 처리가 가능함.

```java
# message_en.properties 파일
item=Item
item.id=Item ID
item.itemName=Item Name
item.price=price
item.quantity=quantity

# message_ko.properties 파일
item=상품
item.id=상품 ID
item.itemName=상품명
item.price=가격
item.quantity=수량

messageSource.getMessage(code, null, LocaleContextHolder.getLocale());
```
