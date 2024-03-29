---
Created At: 2024-02-19
---
(3) 웹 브라우저에 네이버 를 검색하고 화면에 네이버 화면이 출력이 될 때 까지 내부적으로 어떤 동작들이 수행이 되는지 설명해주세요.

<div align="center"><img src="./Network/Images/브라우저가_하는_일.png" width="300"></div>


1. URL 파싱
    - 사용자가 주소창에 URL을 입력하면, 브라우저는 입력받은 URL 구조를 해석(Parsing)한다.
    - 어떤 프로토콜을 통해 해당URL을 요청할 것인지, 어떤 도메인을 요청할 것인지, 어떤 포트로 요청할 것인지 해석한다.
2. DNS 조회
    - Domain Name Sever(DNS)에 접속해서 해당 도메인에 해당하는 IP 주소를 응답받아 서버에 접속한다.
    - 브라우저는 캐싱된 데이터 중에서 해당 도메인에 해당하는 IP 주소가 있는지를 먼저 확인한다.
    - IP 주소 탐색 과정
3. 서버와 연결
    - 백엔드 개발자는 정보를 수용하기 위해 마련된 서버를 구성, 관리하거나
    - 용도에 맞도록 환경 설정 및 최적화를 하는 역할을 한다.
    1. 패킷 통신
    - 보내고자 하는 데이터를 작은 조각으로 쪼개서 보내는 방식으로, 대부분 인터넷 통신은 패킷 통신을 기본으로 하고 있다.
    2. TCP/IP
    3. 서버와 TCP 소켓 연결
4. HTTP 요청
    1. HTTP
    2. HTTP 메세지 구조
5. 웹 서버와 WAS의 요청 처리 및 응답
    1. 정적 페이지와 동적 페이지
    2. 웹 서버
    3. WAS

네비게이션 실행 =>



# 참고 자료


[브라우저 동작 원리: 백엔드 편](https://velog.io/@gyumin_2/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%A3%BC%EC%86%8C%EC%B0%BD%EC%97%90-URL-%EC%9E%85%EB%A0%A5-%EC%8B%9C-%EC%9D%BC%EC%96%B4%EB%82%98%EB%8A%94-%EA%B3%BC%EC%A0%95-%EB%B0%B1%EC%97%94%EB%93%9C-%ED%8E%B8)
[Chrome browser의 동작: 프론트엔드의 관점에서, 렌더링은 어떻게 이루어지나? - 참고](https://velog.io/@reum107/how-browser-works)
[Inside look at modern web browser, Google developers](https://developer.chrome.com/blog/inside-browser-part1/)