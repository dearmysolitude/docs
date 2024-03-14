[NAT - MS learn](https://learn.microsoft.com/ko-kr/azure/rtos/netx-duo/netx-duo-nat/chapter1)
[NAT - 테크유람 brunch](https://brunch.co.kr/@sangjinkang/61)
[NAT - GeeksForGeeks](https://www.geeksforgeeks.org/network-address-translation-nat/)
Network Address Translation. 네트워크 주소를 번환하는 것. 네트워크에서 주소, 즉 L3 주소는 IP이므로 NAT는 [[네트워크 주소 체계#^fed353|IP 주소]]를 변환하는 것이다.  하위 네트워크를 구성하는 방법이며 DHCP를 적용하여 간편하게 IP를 배정할 수 있다.
- DHCP: 네트워크 IP를 접속 시마다 임의로 배정하는 유동 IP 방식. 공유기는 DHCP를 통해 접속하는 PC에 IP 주소를 할당한다.
- IPv4에는 각 네트워크가 임의로 쓸 수 있는 사설(private)주소 범위가 정의되어 있다: 사설 주소 범위는 각 네트워크 내에서만 유효. [[네트워크 주소 체계#^54b9a2|하위 네트워크: 서브넷]]
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217086&authkey=%21ANZDQcFsWin4VKw&width=870&height=148" alt="">
  <figcaption>IPv4</figcaption>
</figure>
사설 IP는 특정 네트워크 안에서 유효하므로, 사설 IP를 가지고 있는 IP packet은 이 네트워크를 벗어나기 위해 **1) 공용 IP 또는 2) 진입할 네트워크의 사설 IP**로 변환해야 한다.

NAT는 이러한 상황에서 IPv4 변환을 진행해 준다.
	- IP 주소 영역을 다른 IP 주소 영역으로 바꾸어 주는 장치는 모두 NAT이다.
	- 장치라고 해서 반드시 하드웨어이 것은 아니다. 같은 기능을 제공하는 소프트웨어 역시 NAT이다.

NAT를 사용하면
	- 보안상의 이점: 방화벽은 이러한 NAT의 기능을 이용한 것이다.
	- 하위 네트워크를 자유롭게 활용할 수 있다는 점에서 IPv4의 활용성을 높여줌.

## 예시: 공유기
공유기에서는 NAT를 사용하여 IP 주소를 할당한다.
공유기 포트: | WAN |  LAN | LAN | LAN |
- WAN만 외부 공개된 IP를 사용하고, subnet을 기반으로 내부 네트워크 생성,  나머지 Lan포트는 공유기가 설정한 IP를 사용할 수 있도록 한다.
- 공유기는 연결하기만 하면 IP를 설정해주어 NAT 내부에 DHCP를 사용하여 유동적인 IP를 배정하는 것이라고 짐작할 수 있다.
## DHCP
Dynamic Host Configuration Protocol. 네트워크 장치에 IP 주소를 자동으로 할당하는 프로토콜이다. 네트워크 장치에 IP 주소, 서브넷 마스크, 게이트웨이 주소, DNS 서버 주소 등의 네트워크 설정 정보를 제공한다.
`apt install dhcp-client`로 필요시 클라이언트를 설치할 수 있다.
[DHCP - MS learn](https://learn.microsoft.com/ko-kr/windows-server/networking/technologies/dhcp/dhcp-top)
## Loopback
```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network
interface  auto lo
iface lo inet loopback

# The primary network
interface  auto eth0
iface eth0 inet dhcp
```
DHCP 설정을 확인해보다 loopback을 확인해볼 수 있다. 127.0.0.1 부터 127.255.255.255 까지의 주소로 자기 자신을 가르키기 위한 목적으로 예약된 IP 주소이다. IPv6에서는 ::1/128 한개의 주소만 사용한다.
[루프백 주소란? - GeeksforGeeks](https://www.geeksforgeeks.org/what-is-a-loopback-address/)
## 로컬 IP 확인해보기
### Windows
`ipconfig`
```
이더넷 어댑터 이더넷:
   연결별 DNS 접미사. . . . :
   링크-로컬 IPv6 주소 . . . . : fe80::bc8a:896c:e5d5:19ff%14
   IPv4 주소 . . . . . . . . . : 192.168.23.218
   서브넷 마스크 . . . . . . . : 255.255.255.0
   기본 게이트웨이 . . . . . . : 192.168.23.1
```
(주의) 네트워크 설정을 확인해서 수동으로 설정되어 있는 경우: 이더넷 ->인터넷 프로토콜 버전4: 이더넷이다보니 임의로 바꾸면 다른 PC 설정과 충돌할 수 있다.
### Linux
 `apt install net-service` > `ifconfig`
 ```zsh
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.18.143.203  netmask 255.255.240.0  broadcast 172.18.143.255
        inet6 fe80::215:5dff:fe45:5113  prefixlen 64  scopeid 0x20<link>
        ether 00:15:5d:45:51:13  txqueuelen 1000  (Ethernet)
        RX packets 6  bytes 861 (861.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 14  bytes 1066 (1.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
## 포트 포워딩
특정 포트로 들어오는 트래픽을 다른 컴퓨터의 특정 포트로 전달하는 기능.
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217064&authkey=%21AKDQ38_sq7aTSrA&width=1280&height=468" alt="">
  <figcaption>VM ware를 사용하기 위한 포트 포워딩</figcaption>
</figure>
## 게이트웨이
- 내부에서 쓰는 IP(사설 IP): 내부망의 호스트들은 이 주소를 보고 있어야한다: 포트 포워딩으로는 정의가 안되겠지 당연히..
- 외부에서 쓰는 IP(공인 IP)를 가지고 있다.