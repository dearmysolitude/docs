## Java Basic
### 1. 자바 기초
[[1. 기초]]
[[2. Static]]
[[3. 데이터형]]
[[4. Switch_모던자바]]
[[5. for 문의 라벨링 & 향상된 for 문]]
[[6. Expression vs. Statement]]
[[7. 자바 프로그램 실행 과정 - Briefly]]
### 2. 자바의 객체 지향
[[8. 상속(Inheritance)]]
[[9. Object 제공 메서드 - toString(), equals() & hashCode()]]
[[10. Package]]
[[11. 다형성(Polymorphism)]]
[[12. 추상 클래스(Abstract class)]]
[[13. Immutable, 자바 생성자와 super()]]
[[14. Java의 String 클래스]]
[[15. 인터페이스(Interface)]]
[[16. 익명 클래스와 람다]]
[[17. 배열]]
[[18. 깊은 복사(Deep copy)와 얕은 복사(Shallow copy)]]
[[19. 팩토리 메서드 패턴(Factory method pattern)과 Java Reflection]]
### 3. 자바 라이브러리
[[20. Collection 프레임 워크(+ Generic 조금)]]
[[21. 자바의 주석문]]
[[22. 자바의 예외처리]]
[[23. Enum]]
### 4. 자바 입출력과 네트워크 프로그래밍
[[24. 자바 IO 01 - Java IO, 데코레이터]]
[[25. 자바 IO 02 - 파일입출력]]
[[26. 자바 IO 03 - 입출력 클래스 상세]]
[[27. 자바 IO 04 - Composite 패턴]]
[[28. 자바 IO 05 - 객체 직렬화]]
[[29. 자바 스레드]]
[[30. 자바 네트워크 프로그래밍 01]]
[[31. 자바 네트워크 프로그래밍 02]]
## Java Advanced
### 모던 자바
[[TODO. 자바 동적 프록시]]
[[TODO. 함수형 프로그래밍]]
### 자바 구조
[[Java의 설계적 결함]]
[[자바 코드의 동작을 JVM 단에서 살펴보자]]
[[JVM]]
[[자바 Class 와 Reflection]]
[[자바의 문자열 관리 - Buffer, Stream, StringBuffer]]
### 자바 최적화
## 기초
### 단위
1byte = 8 bits
1KB = 1024 Byte
1MB = 1024 KBytes
1GB = 1024 MBytes
### 변수
변수의 선언: 데이터가 저장된 메모리 주소는 컴퓨터가 사용(특히 java에서는). 변수의 선언은 프로그래머가 사용할 수 있는 논리성(논리적 명칭)을 부여하는 것이다.
### 자동 형변환
큰 데이터 타입 = 작은 데이터 타입으로 할당 가능(자동 형변환)
예외: char 타입보다 허용 범위가 작은 byte 타입은 char 타입으로 자동 변환될 수 없음
→ byte는 변수로 사용할 때 내부적으로 int형으로 바꾸어서 사용하기 때문이다(성능 때문).
- 💕 메모리 → CPU로 데이터를 이동시킬 때(변수값을 읽어서 CPU로) 다양한 데이터를 어떻게 전달하는 것이 효율적일까?
- 4 byte 단위로 전달: 이보다 작은 데이터는 마스킹 해서 빼야하는 과정이 더 필요: 성능 저하 <<
- 자바에서는 이런 과정을 줄이기 위해 4byte 로 내부적으로 변환하여 전달한다.
    - 초기화: 컴파일 시
    - 변수 대입: 런타임 시
```cpp
char ch = 'A';
ch = 65; // 컴파일

byte b = 65;

ch = b; // 런타임: 이 경우에 해당한다 <- 💕
```
연산식에서 int 타입의 자동 변환
- 정수 타입 변수가 산술 연산식에서 피연산자로 사용되면 int 타입보다 작은byte, short 타입 변수는int 타입으로 자동 변환되어 연산 수행: 역시 이 것도 앞선 것처럼 성능 이슈 때문임
- 실수 결과를 내는 나누기 연산식을 int 변수에 대입하면 나머지 부분은 사라진다.
### 오류
- 구문 오류: 컴파일러가 잡아줌
- 논리 오류: 디버깅 필요, 시간 많이 걸림
## Java Articles
[Speed of Java Stream](https://medium.com/@daniel.las/speed-of-java-stream-1cc3a94b44c2)
[20 Essential Java/Spring Interview Questions with Answers](https://medium.com/@lotfi-habbiche/20-essential-java-spring-interview-questions-with-answers-7bbcdb89cb1c)