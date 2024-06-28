
## Web Intro
ui: HTML CSS JavaScript
----------------------------------프론트
controller:  UI와 Service를 연결
srvice: 도메인에 국한된 로직, 스펠링 체크라던가
----------------------------------백
이 아래는 자동화가 가능함: 기계적인 작업이 가능 - 규칙이 정해져 있음, 누가 작업하나 결과물이 동일함. 이 아래 계층이 제대로 되어있지 않으면 코드의 복잡성이 증가한다. 
DAO: 서비스를 제공하기 위한 로직 - CRUD, 페이지네이션 등등... 제일 중요함
Domain: 데이터 storage를 다루는 계층
- java 클래스와 table을 1대 1로 매칭하는 부분: 자동화
- 도메인부터 만드는 경우: 즉 DB 설계부터, 새로운 서비스를 만들 때 - 이 때는 이 부분이 자동화가 아님

레거시와 호환도 프로그램 작성에 중요: 자바는 특히 - 이미 생태계가 꾸려져있기 때문에 기술적인 측면은 크게 의미가 없음. 당장은 database ~ service 부분까지 엄청 할 것

### Domain ✨✨✨
관습적 규칙, 모든 코드는 동일.
db와 연결된 계층
- DTO: Domain과 생긴 건 같지만 역할이 다름. 계층간에 정보를 전달하기 위한 객체

### Spring의 Web application layer
> image
> 

### 참고
[Web application의 layer: Internet Tools and Services - Lecture Notes](https://gyires.inf.unideb.hu/GyBITT/08/ch04.html)
[Spring Web Architecture](https://www.petrikainulainen.net/software-development/design/understanding-spring-web-application-architecture-the-classic-way/)

### 계층 간 데이터 전송 객체들
https://ccomccomhan.tistory.com/35

## 다형성

### Interface의 존재
실체 구현체를 사용할 경우의 문제점
- 의존성은 service가 dao에 직접 의존하게 된다.
- interface를 넣음으로써 main(service)이 dao에 의존하게 하는 대신 main 이 control을 가져가게 된다(main은 interface에만 의존한다).
- 서비스단과 dao의 분리가 제대로 안 되는 경우, 어느 단에서 요류가 발생했는지 알아채기가 어려워진다: 범위가 넓어짐.
	- 분리함으로써, mock(개발시, 완벽히 작동하는 dao의 구현체) / impl(실제 서비스에 사용하는 dao 구현체)를 바꿔가며 오류를 발견할 수 있다.
- 인터페이스가 없다면, main은 dao에, dao는 domain에 직접 의존하여 domain이 바뀌기만 해도 모든 코드가 영향을 받는다.
- dao는 코드에 대한 규칙이 정해져있지만, 어떤 자료를 어떤 이름으로, 어떤 형식으로 저장할 지 고민하는 것이 결국에는 이슈이다: Domain에 대한 이해가 필요, 필요에 따라 뽑아낼 수 있는 능력이 있어야 한다.

## 정적 페이지 / 동적 페이지

**단계 1**: 정적 페이지
**단계 2**: page 요청, data를 가져와 페이지 구성: JSP, PHP, ASP ← 은행권의 경우 legacy 기술이 많기 때문에 이 단계 기술을 많이 사용한다.
	WAS, web application server가 db와 연결되어 페이지를 구성한다.
**단계 3**: 데이터만 요청하여 가져옴: RESTful한 architecture로 요청 - 응답이 이뤄진다.
	 전달받은 데이터로 클라이언트에서 html, css, js로 페이지를 구성
		 - 웹: React , View,  Next.js, Nest.js, Angular
		 - 모바일: React Native
	왜? 클라이언트의 리소스가 발달하면서 좋은 경험을 주기위해 클라이언트에서 구성하는 기술이 발전함. 최전방: Node

- 정적페이지에 대한 성능은 web server가 월등하여, 개발할 때는 was를 쓰더라도, 실제 서비스에서는 web서버를 가운데에 두고 그 뒤에 was를 둔다. 동적 페이지에 대한 요청이 들어온 경우 webserver는 was에 요청하여 페이지 구성을 처리한다.
- 코드 템플릿이 포함된 JSP 페이지가 들어오면 서버 단으로 JSP 코드를 전달, 이를 HTML로 바꾸어 완성된 html을 리턴한다. 이렇게 리턴된 후에 jsp의 코드가 처리된다.

학습 route: html, css, js -> JSP -> RESTful

## HTML, CSS, JS 기초
### HTML
- Tag들은 block 타입과 inline타입의 component로 이루어져 있음
- HTML5에 들어와서는 Symantic 한 태그 구성도 가능함. 잘 모르겠으면 다음과 같이 div/span으로만 정의해도 작성은 가능함.
```html
<div>
<span>
```
- Google / Naver / Kakao에의 노출을 쉽게 하기 위해 symantic한 태그 구성이 필요할 수 있다.