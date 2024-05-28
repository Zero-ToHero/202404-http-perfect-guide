# 다이제스트 인증

## MD5
- 임의의 길이의 값을 입력받아서 128비트 길이의 해시값을 출력하는 알고리즘
- 단방향 암호화이기 때문에 출력값에서 입력값을 복원하는 것은 일반적으로 불가능
- 같은 입력값이면 항상 같은 출력값이 나오고, 서로 다른 입력값에서 같은 출력값이 나올 확률은 극히 낮음

### 더 이상 사용하지 않는 이유
- 충돌 취약성: MD5는 동일한 해시 값을 생성하는 서로 다른 두 입력(충돌)을 찾기 상대적으로 쉽다는 것이 입증됨.
- 속도: MD5는 매우 빠르게 계산될 수 있기 때문에, 해커들이 무차별 대입 공격(brute force attack)이나 사전 공격(dictionary attack)을 쉽게 시도할 수 있음. 즉, 해커가 가능한 모든 입력 값을 시도해 볼 수 있는 환경이 조성됨.
- 보안 표준: 많은 보안 표준 및 권고사항에서 MD5 사용을 금지하고 있음

## Argon2
- 2015년 7월 암호 해싱 대회에서 우승작으로 선정된 키 유도 함수
- 최신 암호화 함수 중 하나로, 비밀번호 해시화에 사용되는 알고리즘
- 메모리 사용량을 조절할 수 있어 무차별 대입 공격을 방어하는 데 효과적임. 공격자는 동일한 메모리 요구사항을 충족해야 하므로 공격 비용이 증가함
- 시간 및 메모리 요구 사항을 조정하여 알고리즘의 복잡성을 설정할 수 있음.
- 현재까지 알려진 공격에 대해 강력한 보안성을 제공
- 단점 : 실행 속도가 상대적으로 느림, 구현이 상대적으로 복잡

### 자바에서 Argon2 구현하는 법
1. Gradle 추가
```
implementation 'de.mkammerer:argon2-jvm:2.11'
```
2. 해시 생성
3. 해시 검증
4. 리소스 해제 : 중요**
   - 보안 측면 : 비밀번호와 같은 민감한 데이터는 메모리에 남아 있는 기간이 길어질수록 잠재적인 보안 위험이 커짐
   - 메모리 관리 측면 : 메모리와 CPU 리소스를 많이 사용하는 알고리즘으로 높은 메모리 비용을 설정하면 많은 메모리를 사용하게 되기 때문에 메모리를 적절하게 해제하지 않으면, 메모리 누수가 발생할 수 있음
```
public class Argon2Example {
    public static void main(String[] args) {
        // Argon2 인스턴스 생성
        Argon2 argon2 = Argon2Factory.create();

        // 해시를 생성할 비밀번호
        String password = "my_secure_password";

        // 메모리 비용 (KB), 시간 비용 (iterations), 병렬성 (parallelism) 설정
        int memoryCost = 65536; // 64MB
        int timeCost = 3;       // 3 iterations
        int parallelism = 1;    // single-threaded

        // 비밀번호 해싱
        String hash = argon2.hash(timeCost, memoryCost, parallelism, password);

        // 해시 출력
        System.out.println("Hashed password: " + hash);

        // 비밀번호 검증
        boolean matches = argon2.verify(hash, password);

        if (matches) {
            System.out.println("비밀번호가 유효합니다.");
        } else {
            System.out.println("비밀번호가 유효하지 않습니다.");
        }

        // 리소스 정리
        argon2.wipeArray(password.toCharArray());
    }
}
```