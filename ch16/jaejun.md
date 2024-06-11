```jsx
original_string = "안녕하세요, Hello, World!"

charsets = ['utf-8', 'utf-16', 'utf-32', 'iso-8859-1', 'ascii']

def encode_decode_string(input_string, charset):
    try:
        encoded_string = input_string.encode(charset)
        print(f"Encoded with {charset}: {encoded_string}")

        decoded_string = encoded_string.decode(charset)
        print(f"Decoded back with {charset}: {decoded_string}")

    except UnicodeEncodeError as e:
        print(f"Encoding error with charset {charset}: {e}")
    except UnicodeDecodeError as e:
        print(f"Decoding error with charset {charset}: {e}")

# Test encoding and decoding with each charset
for charset in charsets:
    print(f"\nTesting charset: {charset}")
    encode_decode_string(original_string, charset)
 
```

- UTF-8은 가변 길이 문자 인코딩 방식으로, 유니코드 문자들을 1바이트에서 4바이트 사이의 가변 길이 바이트 시퀀스로 인코딩
- UTF-16은 고정 길이 또는 가변 길이 문자 인코딩 방식으로, 유니코드 문자들을 2바이트 또는 4바이트로 인코딩
- UTF-32는 고정 길이 문자 인코딩 방식으로, 모든 유니코드 문자를 4바이트로 인코딩
- ISO-8859-1은 1바이트 고정 길이 문자 인코딩 방식으로, 유럽 언어의 라틴 알파벳을 지원합니다.
- ASCII는 7비트 고정 길이 문자 인코딩 방식으로, 기본 라틴 문자와 몇 가지 제어 문자를 포함합니다.
- UTF-8은 그 범용성과 효율성으로 인해 현대 컴퓨팅 환경에서 가장 널리 사용

---

퓨니코드(Punycode)는 도메인 이름 시스템(DNS)에서 비 ASCII 문자를 포함한 국제화 도메인 이름(Internationalized Domain Name, IDN)을 표현하기 위해 사용되는 인코딩 방식

ASCII 문자 집합만을 사용하는 DNS의 제한을 극복하고, 전 세계 다양한 언어와 문자 시스템을 지원하기 위해 사용

도메인명은 ascii코드로 구성되어있는데 한글은 유니코드라서 원래는 한글을 못쓴다.

근데 한글을 쓰기 위해서 이걸 ascii로 변환해서 사용하는것이다.

- [https://한국인터넷진흥원.한국/](https://xn--3e0bx5e6xzftae3gxzpskhile.xn--3e0b707e/)