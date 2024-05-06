# 프록시
# Proxy Server Vs FireWall

## Proxy Server

프록시는 클라이언트 또는 서버를 보호하기 위해 배포할 수 있으며, 그 뒤에 있는 디바이스의 개인정보와 보안을 보호할 수 있다. 조직에서 프록시를 설정하면 프록시 뒤에 있는 모든 시스템이 모든 트래픽을 프록시로 보내도록 구성된다.

보통의 프록시 서버는 대리요청자로 가장 간단하게 Foward Proxy 와 Reverse Proxy로 나뉜다.

![image](https://github.com/Zero-ToHero/202404-http-perfect-guide/assets/71249347/ee608391-59c8-4992-8bf7-21498ff1655d)

- CloudFlare
- Aws WAF(Web Application Firewall)
- Nginx Proxy

## FireWall

Firewall(방화벽) 은 네트워크 경계를 정의하고 사이버 위협으로부터 조직을 보호하는 디바이스다. 방화벽은 네트워크 경계에 배포되며 방화벽을 통과하는 모든 트래픽을 검사한다.

방화벽은 보호된 네트워크에 들어오고 나갈 수 있는 트래픽 유형과 경계에서 중지해야 하는 트래픽을 지정하는 미리 정의된 규칙을 적용하여 작동한다.

예를 들어 대부분의 방화벽은 기본적으로 모든 `인바운드` 연결을 거부하고 대부분의 `아웃바운드` 연결은 허용하도록 구성된다. 그런 다음 이러한 일반 정책은 특정 IP 범위에서 들어오고 나가는 트래픽을 차단한다.

![image](https://github.com/Zero-ToHero/202404-http-perfect-guide/assets/71249347/072a9495-0d46-419a-aa89-b2bfb2d47489)
- Fortigate VPN

[VPN 쉽게 이해하기](https://aws-hyoh.tistory.com/161)

[[AWS] 📚 VPC 개념 & 사용 - 사설 IP 통신망 [NAT Gateway / Bastion Host]](https://inpa.tistory.com/entry/AWS-📚-VPC-개념-사용-사설-IP-통신망-NAT-Gateway-Bastion-Host)
