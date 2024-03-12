네트워크 통신을 위한 장비(서버, PC등)는 고유의 IP등을 가져야 한다. 따라서 **(1) IP주소 (2) net mask (3) default gateway (4) DNS의 주소** 를 설정해야 함
이러한 설정은 DHCP 에서 설정하도록 하거나 static하게 환경설정에서 고정값으로 부여해 주어야 한다.
## 네트워크 연결 설정
### netplan
`/etc/netplan/00-installer-config.yml` 파일을 수정하여 네트워크 연결 설정을 수정할 수 있다. 
이후 `netplan apply`로 설정 사항을 반영한다.
```yml
network:
	ethernets:
		enp0s3:
		dhcp4: false # DHCP deactivate
		addresses:
		- 10.0.2.13/24 # IP Address / SubnetMask
		routes:
		  - to: default # Default
		    via: 10.0.2.2 # Gateway
		nameservers:
		  addreses: [168.126.63.1] # DNS
	version: 2
```
dhcp를 비활성화하고 IP 주소, 서브넷 마스크, default gateway, DNS 주소를 추가한 화면.
### 임시 변경(명령어 사용)
명령어를 사용하여 네트워크 환경 설정을 변경하면 부팅 시 원상태로 되돌아온다.
- `ifconfig enp0s3 192.168.1.10 netmask 255.255.255.0 up`: enp0s3 인터페이스가 192.168.1.10 IP 주소를 사용하여 네트워크에 연결한다. 
- `route add -net 192.168.1.0 enp0s3`
	192.168.1.0/24 네트워크에 대한 경로를 추가한다. `-net`은 네트워크 주소와 넷마스크를 지정한다. 
- `route add default gw 192.168.1.1 enp0s3`
	192.168.1.1 게이트웨이를 사용하는 기본 경로를 추가한다.
- **이 설정들을 진행하면 게이트웨이의 주소가 192.168.1.1 이 아니라면 연결이 성립되지 않는다: 이 명령어를 실행하는 것은 VM 환경에서 였음. VM 게이트웨이는 리눅스에서 바꿀 수가 없으므로(외부 주소를 바꿀 수가 없는게 당연하다) 이 설정을 하면 네트워크 연결이 될 수 없다!** netplan의 경우는 원래 gateway 가 10.0.2.2 였다.
## 명령어
### ifconfig
네트워크 인터페이스 카드(NIC, 랜카드)의 상황 / 설정 / 재가동을 위한 명령어
`ifconfig` 로 출력되는 데이터
- inet addr : IP주소
- Bcast  :  브로드 캐스트 주소.
- Mask  : 넷마스크(Netmask)값
- UP : 인터페이스가 활성화되어 있음을 나타냄
- BROADCAST : 브로드 캐스트를 사용함
- RUNNING : 동작중임을 나타냄
- MULTICAST : 멀티 캐스트 사용
- MTU : Maximum Transmission Unit,한 번에 전송할 수 있는 최대패킷의 크기
- Metric : 라우팅할 때 참조되는 거리로 로컬인 경우 값이 1임
- RX/TX : 받은 패킷/전송한 패킷의 총 개수(packets)
- errors : 에러가 발생한 패킷의 수
- dropped :버려진 패킷의 수
- overruns : 손실된 패킷의 수
- collisions : 충돌이 발생한 패킷의 수
### ping
대상 장비가 네트워크상에서 응답하는지 확인하는 명령. ICMP(Internet Control Message Protocol) 에코 패킷을 보내 에코 응답 패킷을 수신하여 연결이 가능한지 확인하는 명령어이다. 대상 장비에서 ICMP가 비활성화 되어있다면 응답을 받을 수 없으므로 네트워크가 단절된 것은 아니다(ping은 공격 수단이 되기도 하기 때문).
### netstat
현재 컴퓨터와 연결되었거나 연결될 목록을 프로토콜과 함께 보여주는 명령어.
`netstat -ant`: 자주 사용하는 옵션
	`a`: listening / established connections 의 모든 연결 표시
	`n`: 호스트 이름 대신 IP 주소를 보여준다.
	`t`: tcp 프로토콜만
	`u`: udp 프로토콜만

> 핸드셰이크만 22번 포트로 진행 > 통신은 다른 포트로 진행 > 22 번 포트는 계속해서 요청을 받을 수 있다: 포트는 한 번에 하나의 연결만 가능함