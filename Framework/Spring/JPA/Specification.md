## 예제
> UserPecs.java
```java
public class UserSpecs {
	public static Specification<User> search(Map<String, Object> filter) {
		return (root, query, builder) -> {
			List<Predicate> predicates = new ArrayList<>();
			filter.forEach((key, value) -> {
				switch (key) {
				case "name":
					predicates.add(builder.equal(root.get(key).as(String.class), value));
					}
				});
		return builder.and(predicates.toArray(new Predicate[0]));
		};
	}
}
```
> UserRepository
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long>, JpaSpecificationExecutor<User>{
Optional<User> findOneByName(String name);
Page<User> findAll(Specification<User> spec, Pageable pageable);
Page<User> findAllByName(String name, Pageable pageable);
List<User> findAllBytype(String type);
}
```
> UserController: 테스트해보기 위한 컨트롤러\
```java
@Controller
public class UserController {
	@Autowired
	UserRepository userRepository;
  
@RequestMapping("/test1")
@ResponseBody
public void test1() {
	Map<String, Object> filter = new HashMap<>();
	filter.put("name", "a");
	PageRequest pageable = PageRequest.of(0, 10);
	Page<User> page = userRepository.findAll(UserSpecs.search(filter), pageable);
	for(User u: page) {
		System.out.println(u.getName());
	}
}
```

람다 함수로 작성된 filter 메서드를 살펴보라.

JPA Criteria를 활용하는 방법으로, 동적으로 직접 쿼리를 일반적인 상황에 맞게 상용하도록 작성할 수 있다. 하지만 조금만 수정을 가해도 코드가 미친듯이 복잡해지기 때문에, 실무에서는  QueryDSL을 사용하는 것이 권장된다.

[JPA Specification](https://velog.io/@hellozin/JPA-Specification%EC%9C%BC%EB%A1%9C-%EC%BF%BC%EB%A6%AC-%EC%A1%B0%EA%B1%B4-%EC%B2%98%EB%A6%AC%ED%95%98%EA%B8%B0)