12. 기본 인증

##LDAP

### 개요
- LDAP(Lightweight Directory Access Protocol)는 디렉토리 서비스를 제공하기 위한 프로토콜로,
네트워크상에서 조직이나 개인, 파일, 디바이스 등을 찾아볼 수 있게 해주는 소프트웨어 프로토콜
- 등장한 지 30년이 지났음에도 불구하고 그동안 IT 환경에 맞춰 변화를 거듭하면서 현재도 기업 시스템과 보안 서비스에서 사용자 관리 및 인증에 사용되는 등 여전히 중요한 기술로 자리 잡고 있음
- LDAP가 등장하기 전 디렉토리 서비스 표준인 X.500의 DAP(Directory Access Protocol)가 존재했지만 OSI 계층 전체의 프로토콜을 지원하고 통신 간에 네트워크 자원을 많이 소비하는 등 운영 환경에 제약이 많았음
- LDAP은 OSI 계층 전체가 아닌 TCP/IP 위에서 운용되고 DAP의 스펙을 최대한 유지하면서도 경량화해 네트워크 부담을 줄이도록 설계됨

### 활용 사례
- 가장 일반적인 LDAP 활용 사례는 디렉터리 서비스에 액세스하여 해당 서비스를 관리할 수 있는 중앙 위치를 제공하는 것
- LDAP를 사용하는 조직은 조직, 조직의 사용자, 자산(예: 사용자 이름, 암호)에 대한 정보를 저장, 관리, 보호할 수 있음
- LDAP는 정보 계층 구조를 제공하여 스토리지 액세스를 간소화하는 데 도움이 되고, 기업이 성장하면서 더 많은 사용자 데이터와 자산을 확보함에 따라 중요할 수 있음
- LDAP는 Kerberos 및 SSO(Single Sign-On), SASL(Simple Authentication Security Layer), SSL(Secure Sockets Layer) 지원을 포함하여 사용자 인증을 목표로 Identity 및 액세스 관리(IAM) 솔루션 역할을 하기도 함

### LDAP와 Active Directory 비교
- LDAP는 네트워크의 모든 사용자 계정을 포괄하는 정보가 포함된 대규모 디렉터리 서비스 데이터베이스인 Microsoft Active Directory(AD) 디렉터리 서비스 등에서 사용되는 핵심 프로토콜
- 더 구체적으로 말해 LDAP는 DAP(Directory Access Protocol)의 경량 버전으로, TCP/IP(Transmission Control Protocol/Internet Protocol)에서 실행되는 디렉터리 서비스에 액세스하고 이를 관리하는 중앙 위치를 제공함
- AD는 사용자와 그룹에 대한 인증 및 관리를 제공하므로 결국 사용자 또는 컴퓨터를 인증하게 됨
- 데이터베이스에는 LDAP로 가져온 것보다 더 많은 볼륨의 속성이 포함되어 있음 
- 그러나 LDAP는 정보가 거의 없는 디렉터리 오브젝트를 찾는 데 특화되어 있으므로 AD 또는 가져오는 디렉터리 서비스에서 모든 속성을 추출할 필요가 없음
- LDAP의 주요 목표는 AD에서 오브젝트(즉 도메인, 사용자, 그룹 등)와 통신하고 오브젝트를 저장하여 이를 LDAP 서버에 있는 자체 디렉터리에 사용할 수 있는 형식으로 추출하는 것
- AD는 전 세계 최대 도서관이고 사용자는 좀비가 언급된 제목의 책을 찾고 있다고 예를 들면,
LDAP의 세계에서는 이 책이 미국에서 출판되었는지, 분량이 1,000페이지 이상인지 또는 좀비 대재앙에서 살아남는 방법에 대한 안내는 (선택 가능한 옵션을 줄이는 데는 도움이 되지만) 중요하지 않음.
LDAP는 요청에 맞는 옵션을 모두 찾을 수 있는 위치를 정확히 알고 원하는 것을 찾았는지 확인해주는 숙련된 사서에 해당

### LDAP 인증 프로세스
- LDAP 인증 프로세스는 클라이언트-서버 인증 모델로, 다음과 같은 주요 요소로 구성됩니다. 
    - DSA(Directory System Agent): 네트워크에서 LDAP를 실행하는 서버
    - DUA(Directory User Agent): DSA에 클라이언트(예: 사용자의 PC)로 액세스
    - DN: LDAP에서 탐색할 디렉터리 정보 트리(DIT)를 통한 경로가 포함된 고유 이름(예: cn=Susan, ou=users, o=Company)
    - RDN(Relative Distinguished Name): DN 내 경로의 각 구성 요소(예: cn=Susan)
    - 애플리케이션 프로그래밍 인터페이스(API): API를 사용하면 다른 제품/서비스의 구현 방식을 몰라도 보유한 제품/서비스와 다른 제품/서비스가 서로 통신할 수 있음
- 사용자가 PC에서 비즈니스 이메일 애플리케이션과 같은 LDAP 지원 클라이언트 프로그램에 액세스를 시도할 때 이 프로세스가 시작됨
- LDAPv3에서는 두 가지 사용 가능한 사용자 인증 방법, 즉 로그인 자격 증명이 있는 SSO와 같은 단순 인증 또는 Kerberos와 같은 프로그램에 LDAP 서버를 바인딩하는 SASL 인증 중 하나를 거침 
- 로그인을 시도하면 사용자에게 할당된 DN을 인증하는 요청이 전송됨
- DN은 DSA를 시작하는 클라이언트 API 또는 서비스를 통해 전송됨
- 클라이언트는 자동으로 DSA에 바인딩되고 LDAP는 DN을 사용하여 LDAP 데이터베이스의 레코드에 대해 일치하는 오브젝트 또는 오브젝트 집합을 검색함 
- 이 단계에서 DN의 RDN이 매우 중요한데, 개인을 찾을 수 있도록 DIT를 통한 LDAP 검색의 각 단계를 제공하기 때문
- 경로에서 백엔드에 연결 RDN이 없으면 결과가 유효하지 않은 것으로 나타날 수 있음
- 이 경우 LDAP가 검색하는 오브젝트는 개별 사용자 계정(cn=Susan)이며 디렉터리의 계정에 일치하는 uid 및 userPassword가 있는 경우에만 사용자를 검증할 수 있음
- 사용자 그룹은 LDAP 디렉터리 내의 오브젝트로도 식별됩니다.
- 사용자가 응답(유효성 여부)을 받으면 LDAP 서버에서 클라이언트의 바인딩이 해제됨
- 그러면 인증된 사용자가 시스템 관리자가 부여한 권한에 따라 필요한 파일, 사용자 정보 및 기타 애플리케이션 데이터를 포함하여 API 및 해당 서비스에 액세스할 수 있음

### LDAP 사용 이유
- 엔터프라이즈 네트워크 관리자는 일반적으로 한 번에 수천 명의 사용자를 관리함
- 따라서 회사 인트라넷과 같이 일상적인 태스크를 위해 사용자 역할과 파일에 대한 액세스를 기반으로 액세스 제어 및 정책을 할당할 책임이 있음
- LDAP는 사용자 관리 프로세스를 간소화하고 네트워크 관리자의 귀중한 시간을 절약하며 인증 프로세스를 중앙화함
- LDAP는 일반적으로 AD에서 사용되지만, UNIX의 Red Hat Directory Server 및 Windows의 오픈소스 애플리케이션인 OpenLDAP를 비롯하여 다른 툴 및 클라이언트 환경에 대한 사용자 인증에도 사용할 수 있음
- API 관리, 역할 기반 액세스 제어(RBAC) 또는 Docker 및 쿠버네티스와 같은 기타 애플리케이션과 서비스를 위한 LDAP 인증 및 사용자 관리 기능을 활용할 수도 있음

### 참고 링크
- https://www.samsungsds.com/kr/insights/ldap.html
- https://www.redhat.com/ko/topics/security/what-is-ldap-authentication
- https://support.apple.com/ko-kr/guide/directory-utility/diru0383607a/mac
- https://www.strongdm.com/blog/ldap-vs-active-directory
