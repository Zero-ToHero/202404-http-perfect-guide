# [HTTP] 8회차 (ch16)

## 국제화

- HTTP를 통해서 여러 언어의 문자로 텍스트를 보여주고 요청하기 위해 문자 집합 인코딩을 사용한다.
- 주요 이슈
  - 문자집합 인코딩
  - 언어 태그
- 서버는 클라에게 각 문서의 문자와 언어를 알려준다.
- 클라가 올바르게 문서를 이루고 있는 비트들을 문자로 변환하고, 올바르게 처리해서 사용자에게 콘텐츠를 제공해줄 수 있도록 해야한다.
- 언어는 Content-Language 헤더로, 문자 집합은 Content-Type으로 알려준다.

```
Content-Language: ko
Content-Type: text/html; charset=utf-8
```

```
클라 -> 서버
Accept-Charset: 인코딩
Accept-Language: 언어

서버 -> 클라
Content-Language
Content-Type
```
