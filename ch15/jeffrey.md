# 15. 엔터티와 인코딩

<br>

# Content-Encoding / Accept-Encoding

### Brotli (br)

![Screenshot 2024-06-09 at 11.46.22 PM.png](./Screenshot_2024-06-09_at_11.46.22_PM.png)
- 구글에서 만든 `Brotli` 압축 알고리즘.
- 주로 웹 서버 및 CDN(Content Deliver Network)에서 **HTTP 콘텐츠를 압축**하여 인터넷 웹 사이트를 더 빠르게 로드하는 데 사용된다.
- `브라우저 캐싱` 및 `정적 리소스 전송`에 최적화
- 압축 속도가 비교적 느리지만, 높은 압축률을 보여준다.
- 브라우저별 지원 버전
    - `Chrome` : 경우 **50(2016/04/20 배포)** 이후 버전
    - `Firefox` **: 44(2016/01/26)** 이후 버전
    - `Safari` : **11(2017/10/05 배포)** 이후 버전
- `css`, `js`, `html` 파일에 대해 `gzip`보다 대략 15~20% 크기 측면에서 효율적인 것으로 알려져 있다.
    - JavaScript 파일 : 약 15%
    - CSS 파일 : 약 16%
    - HTML 파일 : 약 20%
- 참고
    - [https://en.wikipedia.org/wiki/Brotli](https://en.wikipedia.org/wiki/Brotli)

<br>

### Zstandard (zstd)

![Screenshot 2024-06-09 at 11.46.44 PM.png](./Screenshot_2024-06-09_at_11.46.44_PM.png)

- Facebook에서 만든 압축 알고리즘 (오픈소스 제공)
    - [https://facebook.github.io/zstd/](https://facebook.github.io/zstd/)
    - [https://github.com/facebook/zstd](https://github.com/facebook/zstd)
- 주로 속도와 압축 효율성의 균형을 맞추는 데 초점을 맞추고 있다.
- 다양한 사용 사례에 적합
    - 파일 시스템, 데이터베이스, 로그 압축, 파일 압축과 파이프 및 스트리밍 압축
    - 더 일반적인 데이터 압축 솔루션으로 사용
- 브라우저별 지원 버전
    - `Chrome` : 경우 **123(2024/03/19 배포)** 이후 버전
        - iOS Chome 앱은 아직 미제공
    - `Firefox` **: 126(2024/05/14)** 이후 버전
    - `Safari` : **16.4(2023/03/27 배포)** 이후 버전
- 참고
    - [https://chromestatus.com/feature/6186023867908096](https://chromestatus.com/feature/6186023867908096)
    - [https://datatracker.ietf.org/doc/html/rfc8878](https://datatracker.ietf.org/doc/html/rfc8878)


<br>

## 델타 인코딩

![IMG_9557.png](./6f78daa6-8d46-46d4-ba20-d1eca2654e7a.png)

- 완전한 파일이 아닌 **연속 데이터간 데이터 차이의 형태**로 데이터를 저장하고 전송하는 한 방법.
- **두 파일 간의 차이**를 나타내는 델타를 계산하고, 이 델타 정보만을 전송하여 파일을 전송한다.
- **데이터의 중복을 줄이고** **저장 공간을 절약**하거나, **데이터 전송량을 줄이는 데 사용**한다.
- `소프트웨어 배포`, `파일 동기화`, `버전 관리 시스템` 등 다양한 응용 분야에서 사용된다.
    - Git과 같은 버전 관리 시스템
    - 온라인 백업
    - 문서 편집기 (Google Docs)
    - 비디오 / 오디오 코덱

<br>

### 시그니처 (Signature)

![IMG_9558.png](./3dd09d1a-018a-4b4e-a035-630448af8d7e.png)

- 원본 파일을 일정한 크기의 블록으로 자르고 각 블록의 체크섬을 계산 후, 계산된 체크섬 블록을 이용하여 비교를 하는데, 이 체크섬 블록을 `Signature`라고 한다.
- 바이트 단위가 아닌 블럭 단위로 비교 수행
- 원본 자체를 비교할 필요 없이, 훨씬 작은 크기의 파일을 가지고 블럭 단위로 비교 가능.
- 구현체마다 스타일이 다르다.
    
    ![Screenshot 2024-06-10 at 10.57.21 PM.png](./Screenshot_2024-06-10_at_10.57.21_PM.png)
    
    - Weak hash가 Strong hash에 비해 계산 비용이 매우 적고 빠르며, 차지하는 공간도 적다.
    - Weak hash로 1차 비교 후, 같으면 Strong hash로 한번 더 비교.

<br>

### 시그니처를 이용한 비교 과정

- 파일 A와 B가 동일한 지 확인
    1. 파일 B에서 시그니처 생성 (블럭 크기는 2로 가정)

![Screenshot 2024-06-10 at 11.03.03 PM.png](./Screenshot_2024-06-10_at_11.03.03_PM.png)

1. 파일 A에서 동일한 블럭 크기로 자르고 해시값을 구한다.
2. 구해진 해시값을 이용해 비교. 해당 부분에서 3회에 걸쳐 동일한 파일인 것을 알 수 있음.

![Screenshot 2024-06-10 at 11.03.27 PM.png](./Screenshot_2024-06-10_at_11.03.27_PM.png)

- 파일 A 뒷부분에 데이터(08)가 추가된 경우
    
    ![IMG_9559.png](./095188d7-61e0-41de-9e5b-484604aaa261.png)
    
    1. 위와 똑같이 블럭을 나누고 해시값을 비교한다.
    2. 1, 2번 블럭은 동일한 것을 확인하였으나, 3번 블럭은 다름을 알 수 있다.

- 파일 A의 처음에 데이터(00)가 추가된 경우
    
    ![Screenshot 2024-06-10 at 11.09.52 PM.png](./Screenshot_2024-06-10_at_11.09.52_PM.png)
    
    - 블럭으로 비교하면 한 칸씩 뒤로 밀리게 되어 00만 추가되었지만, 모든 구간에서 일치하는 구간을 찾을 수 없다. → 전체가 다르다고 판단.

<br>

### 롤링 해시 (Rolling Hash)

![IMG_9560.png](./0733f110-e955-43ed-b725-b5b57e9e192b.png)

- **롤링 해시**는 입력 데이터를 `슬라이딩 윈도우 방식`으로 처리하여 해시 값을 효율적으로 계산하는 해시 함수다.
- 특정 블록을 한 바이트씩 이동하면서 해시를 계산한다.
- 이전 블록의 해시 값을 바탕으로 새로운 블록의 해시 값을 빠르게 계산할 수 있어, 연속적인 데이터 블록 간의 해시 값을 신속하게 갱신할 수 있다.
- 중복되는 부분의 해시값은 그대로 두고 업데이트되는 부분만 해시값을 계산해주어서 중복되는 연산을 줄일 수 있다

![IMG_9561.png](./840abb57-0c0a-4985-894f-0c466ce6608d.png)

- 이전 예제에서 00이 추가되어 첫 번째 블록이 맞지 않지만, 한번 슬라이드를 한 다음 재계산을 하여 맞는 블럭을 찾을 수 있게된다.
- 슬라이딩을 할 때마다 모든 블록을 다시 재계산 해야하는 오버헤드가 발생 할 수 있다.
    - RSYNC 방식에서는 Weak hash를 이전에 해시값을 `재사용`해서 빠르게 계산할 수 있는 특징이 있다.
    - 첫 번째 블록 계산 후 두 번째 블록으로 이동 시, `K ~ /` 까지 재계산 해야되지만, 슬라이딩 했을 때 빠져나가는 `A`를 제외하고, 추가되는 `/`만 더하는 방식으로 동작한다.
    - 만약 **블럭이 일치하면 한 바이트가 아닌 블럭 단위로 이동**되기에, 더 오버헤드가 줄게된다.
- 조금 더 알아보기
    - [https://blog.naver.com/babobigi/220911965116](https://blog.naver.com/babobigi/220911965116)

<br>

### Instruction

![IMG_9563.png](./701efad7-f559-4e7d-857f-134102f9c307.png)

- 위 과정을 모두 소화하면 Instruction이라는 비교 결과를 얻게된다.
- reference와 data로 구성
    - `reference`
        - 실제 전달이 될 필요가 없는 부분.
        - 이전 버전에 포함되어있는 실제 위치를 나타냄. (시작되는 offset과 length값으로 표시)
    - `data`
        - 현재 버전의 실제 변경된 부분 (변경된 데이터 Array가 들어간다.)
- Instruction을 이용한 파일 업데이트 과정
    
    ![IMG_9564.png](./ae02cd31-806e-4a09-b1b1-9dfa16f96063.png)
    
    1. 새로운 빈 파일 생성
    2. reference 부분을 만나면, 자신의 파일에서 해당 위치의 데이터를 추가한다.
    3. data 부분을 만나면, 실제 데이터 부분에 포함되어 있는 byte array를 추가한다.
    4. 처음부터 끝까지 Instruction을 실행하게 되면, 이전 버전은 새로운 버전으로 업데이트 된다.

<br>

### Instruction 생성 과정

1. 시그니처 요청
    
    ![IMG_9565.png](./1ecaa848-c8bd-4a21-b6ea-bed5e5f7a6fa.png)
    

1. 시그니처 생성
    - **블록 사이즈**를 `2`로, `weak hash`와 `strong hash`를 계산하여 시그니처 전달.
    
    ![IMG_9566.png](./f39ee577-b9fd-482a-bd47-29d06f63102c.png)
    

1. 전달받은 시그니처의 블록 사이즈와 동일하게 잘라서, 첫 번째 블록부터 비교 진행.
2. 처음 01, 02의 weak hash값을 구한 뒤, 시그니처에서 값이 있는지 확인 → **hit**
    
    ![IMG_9567.png](./519ee484-14b3-4a6e-b20a-d767174563f7.png)
    

1. 실제로 동일한 값인지, strong hash를 구해서 시그니처에서 값이 있는지 확인 → hit
    
    ![IMG_9568.png](./876fb11f-eb72-40c2-967a-47e02fad91f4.png)
    

1. 첫 번째 블록은 일치하였기에, 한 바이트 이동하지 않고 다음 블록으로 이동하며,
    
    Instruction에는 변경된 부분이 아니기에 reference 타입으로 추가한다.
    
    - 시그니처의 첫 번째에서 찾았기에, offset은 0
    
    ![IMG_9569.png](./602c28ed-0cc0-44ac-abb5-7ccc429e4efb.png)
    

1. 다음 블럭 02, 04에 대해서 weak hash를 계산하고 시그니처에서 확인을 해본다. → miss
    
    Instruction에는 롤링을 하는 과정에서 사라지는 02를 데이터에 추가한다.
    
    - 02, 04 에서 롤링이 되어 02가 사라짐.
    
    ![IMG_9570.png](./abe456e7-0bce-4057-94c4-7839d76745b5.png)
    

1. 다음 바이트로 이동하여 04, 05의 weak hash 계산 후 시그니처에서 확인 및 strong hash 계산 후 시그니처에서 확인 → hit
    
    Instrcution에는 일치하므로 reference 타입으로 추가.
    
    - 시그니처의 2번째에서 찾았기에, offset은 2
    
    ![IMG_9571.png](./abf01924-897e-48b8-9742-103d28df25c9.png)
    

1. 다음 블록 06, 07에 대해서 weak hash를 계산하고 시그니처에서 확인을 해본다. → miss
    
    Instruction에는 롤링을 하는 과정에서 사라지는 06를 데이터에 추가한다.
    
    ![IMG_9572.png](./c1eba785-3aa4-4e18-8668-d50efe306a95.png)
    

1. 마지막 블록으로 이동하고 더 이상 비교할 데이터가 없을 때, 구현체에 따라 동작이 다르다.
    1. 파일 마지막에 도착 시, 다시 한번 해시해서 시그니처에서 확인하는 방법
    2. 나머지 데이터는 data로 추가하는 방법
    
    (아래는 b방식이며, 마지막이 data였어서 그 뒤에 붙여서 추가.)
    
2. 만들어진 Instruction을 전달.

![IMG_9574.png](./f3c04fa8-2d3a-4f83-9e7f-1777b78cdc6b.png)

<br>

### Instruction 적용 과정 (파일 재구성)

1. 빈 파일 생성
2. 첫 번째 Instruction을 해석. reference 블록이기 때문에, 본인이 가지고 있는 파일에서 offset 0번에서 2만큼의 데이터를 새로운 파일에 기록 
    
    ![IMG_9575.png](./d3bb8bad-a302-48bf-8c40-2a5db28d9101.png)
    

1. 두 번째 Instruction은 data 블록이기 때문에, 데이터값 그대로를 파일에 추가한다.
    
    ![IMG_9576.png](./8410fee0-81a9-4790-a39c-00950ad52401.png)
    

1. 세 번째 Instruction인 reference 타입을 확인하고, 자신의 파일에서 offset 2에서 2만큼을 파일에 추가한다.
    
    ![IMG_9578.png](./4afbda6a-5fed-463b-abc5-54c08290bff2.png)
    

1. 네 번째 Instruction. 데이터 블록에서 데이터값 그대로를 파일에 추가한다.
    
    ![IMG_9577.png](./1564f836-d6f0-41d8-bc57-f60c6f2b00fb.png)
    

1. 업데이트 완료
    
    ![IMG_9580.png](./1a70a5e6-215c-4250-9824-8940b12c0a5a.png)
    

<br>

### 클라이언트 ↔ 서버의 업/다운로드에서의 동작 및 델타 인코딩 적용 시 성능 이슈, Trade off에 대한 영상

- [https://www.youtube.com/watch?v=iDsIccISD8o](https://www.youtube.com/watch?v=iDsIccISD8o)