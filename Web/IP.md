Internet Protocol. 네트워크를 연결하는 프로토콜(Protocol: "약속", 즉, 의사소통을 위해 맞추어야 하는 규약)

IP를 이해하기 위해서는 인터넷(Internet)의 초기 목표를 살펴볼 필요가 있다.
ARPANET이라는 네트워크와 ARPA라는 패킷 라디오 네트워크의 관리를 위한 도구였다. ARPANET을 운용하기 위한 조건은 다음과 같았다.
- 구성하는 네트워크 중 일부가 동작하지 않아도 계속 작동해야 함
- 다양한 통신 서비스 지원
- 다양한 네트워크 수용 가능
- 중앙집중식이 아닌 분산처리 방식의 자원 관리
- 비용 효율적
- 적은 비용으로 호스트 추가 가능
- 누가 어느 정도 리소스를 쓰는지 추적 가능
(출처: The Design Philosophy of the DARPA Internet Protocols, David D. Clark et al., Proc. SIGCOMM ‘88)

![IP의 역할](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216616&authkey=%21AGAkwseoOeX8LEM&width=781&height=319)

IP의 역할은 Hour Glass 모델로 구현되어 있다. ~~잘 보면 모래시계 허리에 IP가 있다~~ IP가 네트워크의 작동에서 hour glass의 허리라는 것에는 다음과 같은 의미가 있다.
- 어떤 물리적 연결 기술이던 IP만 구현하면 다양한 서비스(소프트웨어)를 구동할 수 있다. 즉, 어떤 서비스(소프트웨어) 든 **IP로만 구현하면 다양한 물리적 연결 기술로 된 네트워크에서 동작**시킬 수 있는 것이다.
## IPv4 와 IPv6

## 서브넷 마스크
Subnet mask는 IP 주소에서 IP 망을 지닌 네트워크 장치를 식별하기 위해 사용한다. 다음 예시의 설명을 보면 쉽게 이해할 수 있다.
### 192.168.1.0/24


## CIDR 표기법