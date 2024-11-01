JPQL이 엔티티 조회 시 쿼리문을 자동으로 생성하면서 join이 바로 되지 않아 쿼리문이 n개씩 더 발생하는 문제.

- 단계적으로는 lazy loading을 일시적인 해결책이라고 볼 수 있으나, 역시 값을 조회해야할 때에는 N+1 문제가 여전히 발생한다.
- fetch join: JPA와 JQPL에서 제공하는 엔티티들을 효율적으로 로딩하는 최적화 기능이다.
- 연관된 엔티티나 컬렉션을 한 번의 쿼리로 함께 조회한다.
	- 하지만 페이징 처리 시, 특히 JPA의 페이징 API를 사용하는 경우 메모리에서 페이징이 일어나 OOM(Out of memory)이 발생한다.
	- 일대 다 관계에서 데이터 중복 발생
	- 둘 이상의 관계에서 cartesian 곱이 발생하여 조회 데이터가 불어날 수 있다.
- 페이지네이션을 직접 구현하였으므로, 해당 문제는 발생하지 않음. Repository에서 QueryDSL을 사용하여 쿼리문을 구성할 때 이 단계에서 바로 DTO를 생성(리플렉션을 적용) 하였으므로 불필요한 데이터를 조회하지 않고, N+1 문제를 해결(DTO로 직접 매핑, 엔티티 매핑 문제 회피),  
- QueryDSL을 사용하여 복잡한 쿼리를 동적으로 구성.
[JPA 모든 N1 발생 케이스와 해결책](https://velog.io/@jinyoungchoi95/JPA-모든-N1-발생-케이스과-해결책)