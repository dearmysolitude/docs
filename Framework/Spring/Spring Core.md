## Spring core 모식도
![Spring 구조](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%218124&authkey=%21ADAG76_11mKBdsQ&width=911&height=615)


### Dispatcher Servlet
[Dispatcher Servlet](https://mangkyu.tistory.com/18)
### Handler Mapping
컴파일을 진행하면서 여러 Mapping Annotation에 의해 붙은 주소로 연결한다.
[ **추천**: Spring MVC - HandlerMapping의 동작방식 이해하기](https://velog.io/@hsw0194/Spring-MVC-HandlerMapping%EC%9D%98-%EB%8F%99%EC%9E%91%EB%B0%A9%EC%8B%9D-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-1%ED%8E%B8)
### Controller
Mapping한 주소로 데이터를 보낸다.
### ViewResolver
View name과 데이터를 가지고 어떤 뷰를 만들지 prefix를 붙여준다.
(Model에 데이터를 넣어주면 Spring이 알아서 Injection해준다)

## Spring 핵심 모듈

![Spring Core](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%218125&authkey=%21ACJ0xvQSL7Q2nhs&width=833&height=652)

AOP와 Core만 가로로 되어있고, 나머지는 세로로 도식화되어 있다는 점을 주목하라.