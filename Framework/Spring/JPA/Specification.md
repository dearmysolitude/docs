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

