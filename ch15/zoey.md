## HTTP/1.1, HTTP/2, QUIC 비교
| 특성 | HTTP/1.1 | HTTP/2 | HTTP/3 |
| --- | --- | --- | --- |
| 데이터 전송 | 텍스트 기반, 청크 인코딩 필요 | 바이너리 프레이밍 | 바이너리 프레이밍 (QUIC 기반) |
| 다중화 | 없음 | 단일 연결에서 여러 스트림 다중화 가능 | 단일 연결에서 여러 스트림 다중화 가능 |
| 헤더 압축 | 없음 | HPACK 압축 | QPACK 압축 |
| 전송 인코딩 | Transfer-Encoding 필요 | 필요 없음 (프레임 기반 전송) | 필요 없음 (프레임 기반 전송) |

## 바이너리 프레임 기반 프로토콜
- HTTP/2와 HTTP/3는 텍스트 기반이 아닌 바이너리 프레임 기반 프로토콜이다. 즉, 데이터를 바이너리 형식으로 직접 전송하기 때문에 ASCII 문자열로 변환하는 과정이 필요 없고, 텍스트 기반 프로토콜보다 더 효율적인 데이터 표현이 가능하다.

## 헤더 압축
- HTTP/1.x에서 헤더는 항상 일반 텍스트로 전송되고 전송당 500~800바이트의 오버헤드가 추가된다.
- 이러한 오버헤드를 줄이고 성능을 개선하기 위해 HTTP/2에서는 HPACK 압축 형식(Huffman Encoding)을 사용하여 요청 및 응답 헤더 메타데이터를 압축한다.

![image](https://github.com/heeom/202404-http-perfect-guide/assets/64389364/82a435fe-d96c-447c-8ade-4c3506ccd1c3)

- HTTP/1.x의 경우 클라이언트가 두번의 요청을 보낸다고 하면, 두개의 요청 Header에 중복값이 존재해도 중복 전송한다.

![image](https://github.com/heeom/202404-http-perfect-guide/assets/64389364/f0c8b107-df67-4a7a-970a-7af4ede359e8)
- HTTP/2에선 Header에 중복값이 존재하는 경우 Static/Dynamic Header Table 개념을 사용하여 중복 Header를 검출하고 중복된 Header는 index값만 전송하고 중복되지 않은 Header정보의 값은  Huffman Encoding 기법으로 압축해서 전송한다.

https://http2.tistory.com/1
