## Aop

## 언제 사용되는가?
- **성능검사**
- **트랜잭션 처리**
- **로깅**
- **예외 반환**
- 검증
## 구성 요소
- JoinPoint: 모듈의 기능이 삽입되어 동작할 수 있는 실행 가능한 특정 위치
- PointCut: 어떤 클래스의 어느 JoinPoint를 사용할 것인지를 결정
- Advice: 각 JoinPoint에 삽입되어져 동작할 수 있는 코드
- Interceptor: InterceptorChain 방식의 AOP 툴에서 사용하는 용어로 주로 한개의 호출 메소드를 가지는 Advice
- Weaving: PointCut에 의해서 결정된 JoinPoint에 지정된 Advice를 삽입하는 과정(CrossCutting)
- Introduction: 정적인 방식의 AOP 기술
- Aspect: PointCut + Advice + (Introduction)
## 예제
> AspectJConfig.java
```java
@Configuration
@EnableAspectJAutoProxy
public class AspectJConfig {

}
```

> LogAspect.java
```java
@Aspect
@Component
public class LogAspect {
@Before("execution(* com.example.demo.service.*.*AopBefore(..))")
public void onBeforeHandler() {
System.out.println("LogAspect.onBeforeHandler() 핸들러 호출");
}

@After("execution(* com.example.demo.service.*.*AopAfter(..))")
public void onAfterHandler() {
System.out.println("LogAspect.onAfterHandler() 핸들러 호출");
}

@AfterReturning(value="execution(* com.example.demo.service.*.*AopAfterReturning(..))", returning="returnValue")
public void onAfterReturningHandler(Object returnValue) {
System.out.println("LogAspect.onAfterReturningHandler() 핸들러 호출");
System.out.println("ReturnValue: " + returnValue);
}

@AfterThrowing(value="execution(* com.example.demo.service.*.*AopAfterThrowing(..))", throwing="exception")
public void onAfterThrowingHandler(Exception exception) {
System.out.println("LogAspect.onAfterThrowingHandler() 핸들러 호출");
System.out.println("Exception: " + exception.getMessage());
}

@AfterReturning(value="execution(* com.example.demo.service.*.*AopAround(..))")
public void onAroundHandler(ProceedingJoinPoint pjp) {
System.out.println("LogAspect.onAroundHandler() 핸들러 호출, 시작점");
try {
pjp.proceed();
} catch(Throwable e) {
e.printStackTrace();
}
System.out.println("LogAspect.onAroundHandler() 핸들러 호출, 끝점");
}
}
```
## Spring에서 대표적인 AOP의 구현: 트랜잭션
@Transactional


* Test에도 @Transactional을 붙일 수 있음: 테스트가 종료되면 DB를 원래 상태로 롤백한다.