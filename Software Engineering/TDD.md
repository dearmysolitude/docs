1999년 켄트 백이 익스트림 프로그래밍의 일부로 제안한 프로그래밍 패러다임의 하나. 동작하는 코드를 작성하기 이전에 테스트를 먼저 작성하고, 그 테스트를 통과하는 코드를 작성함으로써 테스트된 동작하는 코드를 얻는 개발 방법. 세가지 사이클로 진행된다.
1. 테스트를 작성: 빨간 막대
2. 실행 가능하게 만든다: 초록 막대
3. 올바르게 (고쳐) 만든다.
red-green-refactoring cycle이라고 부르며, 컴파일도 되지 않는 코드를 대상으로 먼저 테스트를 진행하고, 테스트 코드가 컴파일되고 실행까지 되도록 코드를 작성 후, 테스트가 통과되면 테스트의 보호 하에 코드를 다듬는다.
## xUnit
자바에서는 jUnit 프레임워크를 사용하여 테스트 코드를 작성하게 된다. jUnit은 테스트 클래스와 메서드명을 전달받아 실행한다. 실행결과 예외가 발생하지 않으면 테스트가 성공했다고 간주한다.
테스트 케이스의 사전조건/실행/결과 확인을 arrange/act/assert라고 패턴을 명확하게 나누어 [3A pattern](https://xp123.com/3a-arrange-act-assert/)이라고 부른다. Given/when/then으로 나눈 GWT pattern으로 나누기도 한다(BDD, Behaviour Driven Development: 행동 기반 개발에서 사용되는 용어)



[실전에서 TDD 하기 | 카카오페이 기술 블로그](https://tech.kakaopay.com/post/implementing-tdd-in-practical-applications/)