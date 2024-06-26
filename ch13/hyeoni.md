# 13. 다이제스트 인증

## 해시 함수와 해시 테이블
### 해시 함수(Hash function)
#### 정의
- 임의의 길이를 갖는 데이터를 고정된 길이의 데이터로 변환시켜주는 함수
- 이 함수에 의해 얻어지는 값을 해시 값, 해싴코드, 해시 체크섬, 해시라고 부름

### 해시 테이블(Hash Table)
#### 정의
- 키-값 쌍을 저장하는 자료 구조
- 해시 함수를 사용하여 키를 해시 값으로 변환하고 이 해시 값을 인덱스로 사용하여 값을 저장하거나 검색함

### 참고
- [해시 테이블](https://velog.io/@cyranocoding/Hash-Hashing-Hash-Table%ED%95%B4%EC%8B%9C-%ED%95%B4%EC%8B%B1-%ED%95%B4%EC%8B%9C%ED%85%8C%EC%9D%B4%EB%B8%94-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%9D%98-%EC%9D%B4%ED%95%B4-6ijyonph6o)

<br>

## 보안에 대한 고려 사항
### 서버 보안 강화 방안
#### 1. 헤더 부당 변경
- 헤더 부당 변경에 대해 안전한 시스템을 제공하기 위해, 양종단 암호화나 헤더에 대한 디지털 서명하기
#### 2. 재전송 공격
- 재전송 공격을 막기 위해 매 트랜잭션 마다 유일한 난스 값 사용과 함께 타임아웃 값 발급하기
#### 3. 다중 인증 매커니즘
- 클라이언트에게 항상 가장 강력한 인증제도를 선택하게 하며, 가장 강력한 인증 제도만을 유지하는 프락시 서버를 사용하기
#### 4. 사전 공격
- 크래킹하기 어렵도록 복잡한 비밀번호를 사용하고 괜찮은 비밀번호 만료 정책 사용하기
#### 5. 악의적인 프락시와 중간자 공격
- 클라이언트가 사용자에게 인증 강도 시각적으로 보여주기
- 사용자가 가장 강력한 인증을 선택하도록 설정하게 하기
- 클라이언트는 SSL 사용하기
#### 6. 선택 평문 공격
- 클라이언트가 서버에서 제공된 난스 대신 선택적인 c난스 지시자를 사용하여 응답을 생성할 수 있도록 설정하기
#### 7. 비밀번호 저장
- 비밀번호 파일 안전하게 보호하기

### 앱 보안 강화 방안
#### 1. 코드 난독화
- 앱을 해킹으로부터 보호하기 위해 가장 많이 사용되는 앱보안 기술 중에 하나로, 코드에 대해 읽기 어렵게 만드는 작업
#### 2. 런타임 무결성 검사
- 앱이 실행 중일 때 변경되지 않았는지 확인하는 것
- iOS는 [샌드박스](https://jeonyeohun.tistory.com/370), 선언된 권한 및 [ASLR(Address Space Layout Randomization)](https://www.techtarget.com/searchsecurity/definition/address-space-layout-randomization-ASLR)을 사용하여 런타임 보안을 보장 (출처: [apple support](https://support.apple.com/ko-kr/guide/security/sec15bfe098e/web))
#### 3. 네트워크 보안
- iOS에서 https만 연결되도록 하는 정책인 [ATS(App Transport Security)](https://green1229.tistory.com/421) 준수
