# 리다이렉션과 부하 균형

## 라운드 로빈
- 프로세스들 사이에 우선순위를 두지 않고, 순서대로 시간단위(Time Quantum/Slice)로 CPU를 할당하는 방식의 CPU 스케줄링 알고리즘
- 보통 시간 단위는 10 ms ~ 100 ms 정도로 시간 단위동안 수행한 프로세스는 준비 큐의 끝으로 밀려나게 됨. 
- 문맥 전환의 오버헤드(어떤 처리를 하기 위해 들어가는 간접적인 처리 시간 - 메모리 등)가 큰 반면, 응답시간이 짧아지는 장점이 있어 실시간 시스템에 유리.
- 선점 스케줄링 방식
  - 먼저 들어온 순서대로 처리하되, 0.5ms씩 돌아가면서 처리함 
  - 5ms만에 작업이 처리되지 않으면, 한바퀴를 다 돈 후 다음 턴을 기다려 0.5ms를 더 사용

## 리다이렉션을 명시하는 대체 방법
### HTML 리다이렉션
- 웹 개발자는 서버에 대한 제어권을 가지고 있지 않거나 그것을 구성할 수 없는 특수한 상황들이 있을 수 있기 때문에, 웹 개발자들은 refresh를 설정하기 위해 페이지의 <head> 내에 <meta> 엘리먼트와 http-equiv 속성으로 HTML 페이지를 만들 수 있음
- 해당 페이지를 디스플레이할 때, 브라우저는 이 엘리먼트를 발견하고 표시된 페이지로 이동함
```
<head>
  <meta http-equiv="refresh" content="0;URL='http://www.example.com/'" />
</head>
```
- content 속성은 주어진 URL로 리다이렉트 하기 이전에 브라우저가 얼마만큼의 시간(초)을 기다려야 하는지를 나타내는 숫자로 시작
- 더 나은 접근성을 위해 항상 0으로 설정
- 이 메서드는 HTML 페이지(혹은 그와 유사한 무언가)에서만 동작하며 이미지나 다른 어떤 종류의 컨텐츠에 대해서 사용될 수 없음

### JavaScript 리다이렉션
```
window.location = "http://www.example.com/";
```
- JavaScript를 실행한 클라이언트 상에서만 동작

## 일반 서버 내 리다이렉트 구성
### Apache
- 서버 구성 파일 혹은 각 디렉토리의 .htaccess 내에서 설정
1. Redirect 사용 법
```
<VirtualHost *:80>
  ServerName example.com
  Redirect / http://www.example.com
</VirtualHost>
```
- URL http://example.com/ 은 http://www.example.com/ 로 리다이렉트
2. RedirectMatch 사용 법
```
RedirectMatch ^/images/(.*)$ http://images.example.com/$1
```
- images/ 폴더 내 모든 문서들은 다른 도메인으로 리다이렉트
3. 일시적인 리다이렉트를 설정하고 싶지 않다면, 다른 리다이렉트를 설정하는데 여분의 파라메터(사용하고자 하는 HTTP 상태 코드 혹은 permanent 키워드)를 사용
```
Redirect permanent / http://www.example.com
Redirect 301 / http://www.example.com
```

### Nginx
1. 리다이렉트하고자 하는 컨텐츠에 대한 특정 서버 블록을 만들 수 있음
```
server {
  listen 80;
  server_name example.com;
  return 301 $scheme://www.example.com$request_uri;
}
```

2. 폴더 혹은 페이지의 하위 집합에만 적용되는 리다이렉트
```
rewrite ^/images/(.*)$ http://images.example.com/$1 redirect;
rewrite ^/images/(.*)$ http://images.example.com/$1 permanent;
```