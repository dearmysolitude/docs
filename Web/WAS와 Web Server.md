## Static 데이터와 Dynamic 데이터

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217066&authkey=%21ABQstA5YKB7Mrp8&width=819&height=324" alt="">
  <figcaption>Static vs. Dynamic Page</figcaption>
</figure>
## Web Server
- 하드웨어: Web server가 설치된 컴퓨터 / 소프트웨어: 클라이언트로부터 HTTP 요청을 받아 정적인 컨텐츠를 제공하는 프로그램
- HTTP 프로토콜을 기반으로 클라이언트의 요청을 처리, 응답한다.
기능 1
	- 정적 컨텐츠 제공
	- WAS 거치지 않고 바로 자원을 제공한다.
기능 2
	- 동적인 컨텐츠 제공을 위한 요청 전달
	- Request를 WAS에 보내고, WAS의 처리 결과를 클라이언트에 보낸다(Response).
대표적으로 Apache / Nginx / IIS 가 있다.
## WAS(Web Application Server)
- DB 조회나 다양한 로직 처리를 요구하는 동적 컨텐츠를 제공하기 위해 만들어진 application server
- HTTP를 통해 컴퓨터, 장치에 애플리케이션을 수행해주는 미들웨어이다.
- Web Container / Servlet Container 라고도 불린다: JSP, Servlet의 구동 환경이다.
	기능 1
	- 프로그램 실행 환경과 DB 접속 기능
	- 여러 개의 트랜잭션 관리
기능 2
	- 비즈니스 로직 처리
즉, Web server로부터 동적 컨텐츠를 다루기 위해 분리해낸 것이다. 대표적으로 Tomcat이 있다.
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217067&authkey=%21ALs8LjS7m-7QQl8&width=858&height=301" alt="">
  <figcaption>WAS</figcaption>
</figure>
### WAS의 사용
여러 WAS로 분리하여 사용하는 경우가 많다.
- Load balancing
- Failover, failback 처리에 유리하다.
- 무중단 운영에 유리하다.
또한, web server와 WAS의 물리적 분리로 인해 SSL에 대한 암복호화 처리를 통한 보안 강화가 필요하다.

## 참고 자료
[WAS와 웹서버 - 10분 테코톡](https://www.youtube.com/watch?v=mcnJcjbfjrs)