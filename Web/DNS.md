
|       DNS 레코드 유형       |                           설명                            |                     예시                     |
| :--------------------: | :-----------------------------------------------------: | :----------------------------------------: |
|      A (Address)       |                  도메인을 IPv4 주소와 매핑합니다.                   |          example.com → 192.0.2.1           |
|          AAAA          |                  도메인을 IPv6 주소와 매핑합니다.                   |         example.com → 2001:0db8::1         |
| CNAME (Canonical Name) |              하나의 도메인을 다른 도메인에 별칭으로 연결합니다.               |       www.example.com → example.com        |
|   MX (Mail Exchange)   |                 이메일을 수신할 서버의 주소를 정의합니다.                 |       example.com → mail.example.com       |
|          TXT           | 도메인에 텍스트 정보를 저장할 수 있으며, SPF, DKIM과 같은 이메일 인증에 주로 사용됩니다. |     example.com → "v=spf1 include:..."     |
|    NS (Name Server)    |              해당 도메인의 권한이 있는 네임 서버를 지정합니다.               |       example.com → ns1.example.com        |
|          SRV           |               특정 서비스를 제공하는 서버의 위치를 지정합니다.               | _sip._tcp.example.com → sipserver.com:5060 |
|          PTR           |          IP 주소를 도메인으로 역으로 매핑합니다 (Reverse DNS).          |          192.0.2.1 → example.com           |

## DNS의 계층 구조
![DNS 구조](https://1drv.ms/i/s!Ano-rmQ7e_nEwB5vO-xZrlZAwM3S?embed=1&width=711&height=282)
- DNS 는 역트리 구조로 Root 로부터 Top-Level, Second-Level, Thrid Level(Subdomain)방향으로 원하는 주소를 단계적으로 찾아간다.
![DNS 구조](https://1drv.ms/i/s!Ano-rmQ7e_nEwB_iDfyXGlp1ZdgL?embed=1&width=664&height=423)
- https://ipinfo.io: 내 public ip를 확인할 수 있다.
## hosts 파일
호스트 이름에 대응하는 IP 주소가 저장되어 있어서 도메인 이름 시스템(DNS)에서 주소 정보를 제공받지 않고도 서버의 위치를 찾게 해줌
- Windows의 hosts 파일: `C:\Windows\System32\drivers\etc`
- 포트 지정이 안됨: 80번 포트만 가능
## 참고
- [DNS 개념잡기 - (2) DNS 구성 요소 및 분류(DNS Resolver, DNS 서버) (tistory.com)](https://anggeum.tistory.com/entry/DNS-%EA%B0%9C%EB%85%90%EC%9E%A1%EA%B8%B0-2-DNS-%EA%B5%AC%EC%84%B1-%EC%9A%94%EC%86%8C-%EB%B0%8F-%EB%B6%84%EB%A5%98DNS-Resolver-DNS-%EC%84%9C%EB%B2%84)
- [만화로 보는 DNS over HTTPS ★ Mozilla 웹 기술 블로그](http://hacks.mozilla.or.kr/2019/10/a-cartoon-intro-to-dns-over-https/#trr-and-doh)
- [DNS에 대한 설명(디테일 하게....) (tistory.com)](https://hwan-shell.tistory.com/320)
- [Dig(DNS 조회) (googleapps.com)](https://toolbox.googleapps.com/apps/dig/)
	- www.google.com을 넣어서 어떤 상태인지 확인해보자: A의 경우 6개가 있는데 로드밸런싱을 위한 것일 것
	- TTL: 업데이트 주기