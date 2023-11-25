1. **불편한 예외 처리**

Java는 다른 언어와 다르게 프로그래머의 검사가 필요한 예외 (Exception을 직접 상속하는 예외 클래스)가 등장한다면 반드시 프로그래머가 선언을 해줘야 하고 만약 하지 않는다면 컴파일을 거부한다.

코드의 가독성과 간결성이 떨어지게 되고, 예외 처리 방법의 오용으로 인해 성능 저하가 발생할 수 있다.

<br/>

2. **코드 작성의 복잡성**

얼마나 복잡한 프로그램을 작성하던지 Class를 무조건 작성해야 한다. Boilerplate라고 부르는 기본 구조를 작성하기 위해 의무적으로 작성해야 하는 서식을 코드로 작성해야 하는 것이다.

자바에서는 Java 8부터 람다 표현식, 스트림 등을 지원함으로써 이러한 불편함을 줄이려 하고 있다. 자바의 이러한 특성은 OOP(객체지향 프로그래밍)을 기반으로 하고 있기 때문에 발생하는 것으로, 메서드나 필드에 대한 소속을 파악하기 쉬워 코드 이해가 용이하다.

<br/>

3. **가비지 컬렉션의 오버헤드**

여타의 기존 프로그래밍 언어와는 다르게 java의 경우 JVM의 heap 메모리 영역을 free() 메서드를 사용하지 않고도 메로리를 해제해주는 Garbage Collector 를 가지고 있다.

동적으로 할당된 메모리 영역 중 필요 없는 역역을 주기적으로 삭제하는 프로세스를 가비지 컬렉션이라고 부른다. 개발자 입장에서는 이러한 과정을 통해 메모리 관리에 대해 크게 신경쓰지 않고 개발에 집중할 수 있는 장점을 지닌다.

<br/>

4. **Unsigned integer types가 없음**

자바에는 기본적으로 c, c++등에서 존재하는 unsigned 자료형이 없다. 암호학과 같이 매우 큰 양의 정수를 활용해 다양한 처리를 하는 분야에서 사용하기에 부적합할 수 있다.

<br/>

5. **Operator overloading을 할 수 없음**

연산자 오버로딩을 허용하지 않는다. 수학적인 내용의 객체들에 대해서 가독성을 떨어뜨리고 활용하는데에 불편한 점이 있다.
연산자 오버로딩이란, 객체지향 프로그래밍에서 다형성의 특정 경우이다. 연산자들이 함수 인자를 통해 구현할 때를 이른다. 

6. **Generics 타입을 Runtime에 활용할 수 없다**

Generics를 활용해 컴파일 타임에 타입 체크를 하고 나면 제네릭 인자로 넘겨져 온 타입은 Type Erasure라는 절차를 통해 제거된다. 따라서 인자로 넘겨진 타입은 Runtime에 활용될 수 없다.

```
ArrayList<Integer> integerList = new ArrayList<>();
ArrayList<Float> floatList = new ArrayList<>();

if (integerList.getClass() == floatList.getClass()) {
    System.out.println("equal");
}
```

위와 같은 코드에서는 제네릭 타입 인자 값이 다르지만 같은 클래스로 인정된다. Runtime에 타입이 지워지기 때문이다.
다음과 같은 코드에서 `instanceof`, `new`등 연산자를 활용할 수 없는 이유도 그 때문이다.
```
public class Foo<E> {
    public static void foo(Object item) {
        if (item instanceof E) { // Complie Error
            // ...
        }

        E item2 = new E(); // Compile Error
        E[] arr = new E[10]; // Compile Error
    }
}
```


<br/>

### 출처

(1) [JAVA JAVA의 설계적 결함 - 벨로그](https://velog.io/@sung8881/JAVA-JAVA%EC%9D%98-%EC%84%A4%EA%B3%84%EC%A0%81-%EA%B2%B0%ED%95%A8)

(2) [JAVA:: JAVA의 설계적 결함 한 가지 - Garbage Collection에 대하여 - 벨로그](https://velog.io/@ecvheo1/Java의-설계적-결함-한-가지-Garbage-Collection에-대하여)

(3) [자바 설계 결함 - 벨로그](https://velog.io/@xogml951/자바-설계-결함)

(4) [자바의 설계 결함 - Github.io/KimYoungJ](https://github.com/KimYongJ/wanted-pre-onboarding-challenge-be-task-July/blob/main/4%EB%B2%88%20%EB%AC%B8%EC%A0%9C.md)