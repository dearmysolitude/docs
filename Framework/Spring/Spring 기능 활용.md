## \[토막글] @Component를 상속하는 것들
 Repository Controller Service는 모두 component를 상속함. 이들은 스프링에서 관리하여 singleton으로 Spring Bean으로 존재하며, autowired를 통해 injection할 수 있다. Entity를 Component를 상속받지 않는 것을 확인해보자.
## 뷰 접근 설정
```application.properties
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

spring에 의해 관리되는 파일 경로이므로 public에서는 접근할 수 없어 prefix / suffix를 붙이는 방식으로 접근시킨다.
따라서 위 설정을 추가하면: http://localhost:8080/SpringBoard/hello 요청시 src/main/webapp **/WEB-INF/views/** hello **.jsp** 페이지를 돌려준다.
##  기능 활용하기
### @Configuration
[@Configuration 이란?](https://blogshine.tistory.com/551)
### Spring Interceptor
[스프링 인터셉터란?](https://kimvampa.tistory.com/127)
### Handler Method Argument Resolver
Argument Resolver를 만들기 위해서는 Handler Method Argument Resolver인터페이스를 구현해야 한다.
[Argument Resolver를 사용하여 사용자 정보 불러오기](https://velog.io/@uiurihappy/Spring-Argument-Resolver-%EC%A0%81%EC%9A%A9%ED%95%98%EC%97%AC-%EC%9C%A0%EC%A0%80-%EC%A0%95%EB%B3%B4-%EB%B6%88%EB%9F%AC%EC%98%A4%EA%B8%B0)
### Spring Annotation
 [Spring Annotation의 원리와 Custom Annotation 만들어보기](https://donghyeon.dev/spring/2020/08/18/Spring-Annotation%EC%9D%98-%EC%9B%90%EB%A6%AC%EC%99%80-Custom-Annotation-%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B3%B4%EA%B8%B0/)
###  @SpringMvcConfigurer
[Spring MVC Configurer](https://jake-seo-dev.tistory.com/605)

## 관련 문제(블로그로 이전 예정)
Spring에서 api에 접근할 때 로그인 한 경우와 로그인 하지 않은 경우에 대해서 다른 처리를 해야 할 때, 처음에는 api를 분리해서 처리하였는데, 멘토님으로부터 api를 분리하는 기준은 사용자가 아닌, 해당 요청이 어떤 기능인지이므로 이런 식의 접근은 바람직하지 않다는 피드백을 받았다. 따라서, Spring에서 지원하는 사용자 정의 annotation을 통해 로그인한 유저와 아닌 유저를 구분하여 처리하도록 코드를 작성하였다.
그런데 프로젝트 기간이 짧다보니 해당 기술에 대한 얕은 이해를 토대로 팀원의 코드에 의존하였다. 따라서 이를 이해하기 위해 Spring에서 요청에 대해 처리하는 기능인 Arguement Resolver와 Interceptor에 대해 알아보았다.

https://tecoble.techcourse.co.kr/post/2021-05-24-spring-interceptor/