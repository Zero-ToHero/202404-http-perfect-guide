# 국제화

## Accept-Charset 을 더 이상 사용하지 않음
1. 주요 브라우저에서 사양을 준수하고 있기 때문에 utf-8 설정을 주지 않아도 자동적으로 utf-8 로 인코딩 해줌
2. Spring Framework 5.2부터 depreacated
<img width="1384" alt="image" src="https://github.com/hwyi21/202404-http-perfect-guide/assets/58624211/ed8eda44-1327-4044-bca9-44e342aaf86e">
<img width="753" alt="image" src="https://github.com/hwyi21/202404-http-perfect-guide/assets/58624211/d4c039a6-51bc-4d36-aa58-88de9744c8d0">

## MIME (Multipurpose Internet Mail Extensions)
- 인터넷에서 멀티미디어 메시지를 전송하는 데 사용되는 표준
- 네트워크를 통해 ASCII 파일이 아닌 바이너리 파일인 음악, 영상, 워드 등의 파일을 보내야할 일이 많아 졌고 해당 파일들을 문제 없이 전송하기 위해서는 텍스트 파일로 변환이 필요해짐
- 구조 : type/subtype 으로 이루어짐 text/html, image/jpeg, application/json
- HTTP 응답에서 Content-Type 헤더를 사용하여 MIME 타입을 클라이언트에게 전달함 Content-type : application/json
