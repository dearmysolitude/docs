## 서버와 클라이언트
- 서버: 클라이언트의 요청에 따라 서비스를 제공하는 컴퓨터 세스템 혹은 프로그램. 클라이언트보다 강력한 성능과 용량을 가지고 있으며, 다수의 클라이언트를 동시에 처리할 수 있다.
- 클라이언트: 서버에 서비스를 요청하는 사용자 혹은 사용자의 단말기
### 서버-클라이언트 모델
1. 서버는 클라이언트에 필요한 서비스를 제공한다(서비스 제공).
2. 클라이언트는 서버에 요청을 보내고, 서버는 요청에 대한 응답을 보낸다(요청-응답).
3. 서버와 클라이언트는 네트워크를 통해 통신한다(통신).
## NAT(Network Address Translation)
- 하위 네트워크를 구성하는 방법. DHCP를 적용하여 간편하게 IP를 배정할 수 있다.
- DHCP: 네트워크 IP를 접속 시마다 임의로 배정하는 유동 IP 방식. 공유기는 DHCP를 통해 접속하는 PC에 IP 주소를 할당한다.
## IP(Internet Protocol)
네트워크에서 장치를 식별하는 고유 숫자.
- IPv4(32 bit, 8 bit X 4) / IPv6(128 bit, 8 bit X 6)
- 네트워크는 IPv4(8 비트 (0~255) 숫자 4개로 이루어진 주소를 사용한다.) 
- 가용 주소: 2^32(약 43 억) 개
- 현재 IP 주소 소요에 대한 대안: NAT / IPv6(2^128개의 주소를 가질 수 있지만 사람에게는 조금 사용성이 낮다)
## IP 주소 소요의 대안: NAT
Virtual box: NAT방식으로 IP를 관리한다.
### 예시: 공유기
공유기에서는 NAT를 사용하여 IP 주소를 할당한다.
공유기 포트: | WAN |  LAN | LAN | LAN |
- WAN만 외부 공개된 IP를 사용하고, subnet을 기반으로 내부 네트워크 생성,  나머지 Lan포트는 공유기가 설정한 IP를 사용할 수 있도록 한다.
- 공유기는 연결하기만 하면 IP를 설정해주어 NAT 내부에 DHCP를 사용하여 유동적인 IP를 배정하는 것이라고 짐작할 수 있다.
## 포트 포워딩
특정 포트로 들어오는 트래픽을 다른 컴퓨터의 특정 포트로 전달하는 기능.
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217064&authkey=%21AKDQ38_sq7aTSrA&width=1280&height=468" alt="">
  <figcaption>VM ware를 사용하기 위한 포트 포워딩</figcaption>
</figure>
### 로컬 IP 확인해보기
Windows IP주소: cmd > ipconfig
```
이더넷 어댑터 이더넷:
   연결별 DNS 접미사. . . . :
   링크-로컬 IPv6 주소 . . . . : fe80::bc8a:896c:e5d5:19ff%14
   IPv4 주소 . . . . . . . . . : 192.168.23.218
   서브넷 마스크 . . . . . . . : 255.255.255.0
   기본 게이트웨이 . . . . . . : 192.168.23.1
```
(주의) 네트워크 설정을 확인해서 수동으로 설정되어 있는 경우: 이더넷 ->인터넷 프로토콜 버전4: 이더넷이다보니 임의로 바꾸면 다른 PC 설정과 충돌할 수 있다.
### VMware IP 확인해보기

## 네트워크 설정하기
### VMware 포트포워딩
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217065&authkey=%21AIPBqzRoBgGhmqA&width=831&height=553" alt="">
  <figcaption>VM ware에서 포트 포워딩 설정하기</figcaption>
</figure>
### SSH, Putty
SSH: telnet에서 보안 기능이 추가 됨
Putty: 윈도우를 클라이언트로서 리눅스에 접속할 수 있다. Putty를 통해 접속 설정을 마쳤다면 vs code를 통해 vm에 접속할 수 있도록 설정할 수 있따.
- vscode -> remote ssh(extension) -> f1을 눌러 remote : add new ssh host를 실행하자
- `ssh [사용자 ID]@[ip주소]` 를 입력하고 enter’
- f1 -> connect to host 혹은 아래에 있는 >< 을 눌러 ssh로 접속하자
- 비밀 번호를 입력하면 vs code로 리눅스에 접속할 수 있다.
- 터미널을 열어보면 bash가 실행되는 것을 확인할 수 있다.
### FileZilla
접속할 IP/사용자 명/사용자 비밀번호/포트번호 22입력해주자.
사용자명을 root로는 접속할 수 없다: 관리자 권한을 remote로 줄 수 없게 하기 위한 조처(정책)
#### (실습) 한번 바꿔볼까?
- sudo vim /etc/ssh/sshd_config를 실행하여 설정을 변경해주자.
- i로 입력 모드에 들어간 후 수정 후에 esc, `:wq`를 입력하여 저장, 종료해 준다.
- `systemctl restart ssh`명령어로 시스템 재시작
- filezilla로 root 접속해 보면 접속이 되는 것을 확인할 수 있다.
### VSCode
1. 환경 설정
	- vscode실행 -> remote ssh(extension) 설치 -> f1을 눌러 remote-ssh : add new ssh host를 실행
	- `ssh [리눅스 유저id]@[본인 로컬 pc ip주소]` 를 입력하고 enter
	- f1 -> connect to host 혹은 아래에 있는 >< 을 눌러 ssh로 접속하자
	- 앞서 설정한 리눅스 비밀 번호를 입력하면 vs code로 리눅스에 접속할 수 있다.
	- 터미널을 열어보면 bash가 실행되는 것을 확인할 수 있다.
2. vscode가 root 계정으로 리눅스에 접속하도록 하기
	- f1을 눌러 remote ssh: 새 HOST 추가하기
	- `ssh root@[로컬 IP주소]` 입력
	- config를 실행, 새로운 호스트에 대한 정보를 입력하고 저장한다.
	- 이제 다시 remote를 통해 ssh 접속을 시도하면, root계정으로 접속할 수 있는 것을 확인할 수 있다.
	- /var/lib/tomcat9 폴더를 vscode에서 열어주자
	- webapps > index.html 파일을 열어 수정해주자: hello world! 
	- 저장하고 나면 localhost:8080 으로 접속 시 해당 문구가 출력되는 것을 확인할 수 있다.