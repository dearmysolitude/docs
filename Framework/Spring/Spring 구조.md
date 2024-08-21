

### Handler Mapping
컴파일을 진행하면서 여러 Mapping Annotation에 의해 붙은 주소로 연결한다.
[ **추천**: Spring MVC - HandlerMapping의 동작방식 이해하기](https://velog.io/@hsw0194/Spring-MVC-HandlerMapping%EC%9D%98-%EB%8F%99%EC%9E%91%EB%B0%A9%EC%8B%9D-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-1%ED%8E%B8)
### Controller
Mapping한 주소로 데이터를 보낸다.

### ViewResolver
View name과 데이터를 가지고 어떤 뷰를 만들지 prefix를 붙여준다.

### View
Model에 넣어주면 view에 자동으로 injection 되기 때문에 



## Maven Repository
Maven Repository(원격지)로부터 다양한 dependency를 불러와 저장하여(.m2 디렉터리) 필요할 때 jar파일을 사용한다.


## 뷰 접근시키도록 설정하기
```properties
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

spring에 의해 관리되는 파일 경로이므로 public에서는 접근할 수 없어 prefix / suffix를 붙이는 방식으로 접근시킨다.
따라서 위 설정을 추가하면: http://localhost:8080/SpringBoard/hello 요청시 src/main/webapp **/WEB-INF/views/** hello **.jsp** 페이지를 돌려준다.



## Argument Resolver
[Argument Resolver를 사용하여 사용자 정보 불러오기](https://velog.io/@uiurihappy/Spring-Argument-Resolver-%EC%A0%81%EC%9A%A9%ED%95%98%EC%97%AC-%EC%9C%A0%EC%A0%80-%EC%A0%95%EB%B3%B4-%EB%B6%88%EB%9F%AC%EC%98%A4%EA%B8%B0)

##  기능 활용하기
### ✨ @SpringMvcConfigurer
[Spring MVC Configurer](https://jake-seo-dev.tistory.com/605)
### ✨ @Configuration
[@Configuration 이란?](https://blogshine.tistory.com/551)
### ✨ Spring Interceptor
[스프링 인터셉터란?](https://kimvampa.tistory.com/127)
### Spring Annotation
 [Spring Annotation의 원리와 Custom Annotation 만들어보기](https://donghyeon.dev/spring/2020/08/18/Spring-Annotation%EC%9D%98-%EC%9B%90%EB%A6%AC%EC%99%80-Custom-Annotation-%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B3%B4%EA%B8%B0/)