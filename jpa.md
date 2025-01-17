
## TIL 주제
JPA란?

## 주요 내용

### 간단 정리
- JPA는 자바의 ORM 기술을 쉽게 구현하도록 도와주는 API이다. JpaRepository를 상속하는 인터페이스에 메서드 이름만 적어놓으면
자동으로 처리해주는 ORM이다(ex. 구현체 생성, 쿼리문 구현 등).
- 메소드 이름은 findby(필드명), deleteby(필드명) 처럼 메소드 명칭만 적어주면 개발자는 SQL을 작성하지 않아도 쿼리문을 만들어준다.

### JPA(Java Persistence API)란?
- 자바에서 객체를 데이터베이스에 저장하고 관리하기 위한 인터페이스와 기능을 제공하는 API.
- JPA를 사용하면 객체와 관계형 데이터베이스 간의 매핑을 손쉽게 처리할 수 있으며 데이터베이스의 CRUD(Create, Read, Update, Delete) 작업을 간편하게 수행할 수 있다.

### 예제코드
- 엔티티 클래스 정의
  - 데이터베이스 테이블과 매핑될 엔티티 클래스를 정의한다.
  - 이때, 엔티티 클래스는 @Entity 어노테이션을 이용하여 정의한다. 
```java
@Entity                     // 엔티티 클래스임을 선언.
@Table(name = "users")      // 해당 엔티티 클래스와 매핑될 데이터베이스 테이블 이름을 지정.
public class User {

    @Id // 엔티티 클래스의 주요 식별자(primary key)임을 선언
    @GeneratedValue(strategy = GenerationType.IDENTITY) // 엔티티의 식별자 값을 자동으로 생성
    private Long id;

    // 해당 엔티티 클래스의 필드가 데이터베이스의 칼럼으로 매핑될 때,
    // 해당 칼럼의 제약 조건을 설정하는 어노테이션. (널허용 = x 등)
    @Column(nullable = false, unique = true) 
    private String username;

    @Column(nullable = false)
    private String password;

    // getters and setters
}
```

- Repository 인터페이스 정의
  - 엔티티를 조작하기 위한 Repository 인터페이스를 정의한다.
  - 이때 JpaRepository 인터페이스를 상속받아 사용한다.
  - JpaRepository 인터페이스를 상속받기에 따로 메서드 구현, 구현체 작성을 하지 않아도 자동으로 생성된다.

```java
// 해당 인터페이스가 스프링의 데이터 접근 계층(Data Access Layer)의 컴포넌트임을 선언.
// 간단하게 말하자면 "레파지토리"를 의미.
@Repository
// JpaRepository<User, Long> 인터페이스 : 스프링 데이터 JPA에서 제공하는 CRUD 메서드를 상속받아 사용할 수 있는 인터페이스.
public interface UserRepository extends JpaRepository<User, Long> {
	
    // 데이터베이스에서 username 필드 값이 일치하는 User 엔티티 객체를 반환하는 메서드.
    Optional<User> findByUsername(String username);

}
```

- 엔티티 매니저 팩토리 설정
  - application.properties 또는 application.yml 파일을 이용하여 JPA 설정 정보를 지정.

```yaml
# H2 인-메모리 데이터베이스를 사용하기 위한 데이터 소스 설정(application.yml)
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# JPA를 사용하기 위한 설정
spring.jpa.hibernate.ddl-auto=create                                        # 애플리케이션 실행 시 엔티티를 대상으로 DDL 실행
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.H2Dialect     # H2 데이터베이스 방언 지정
```

- 서비스 클래스 정의
  - 엔티티를 사용하는 비즈니스 로직을 구현하는 서비스 클래스를 정의.
  - 이때, UserRepository 인터페이스를 주입받아 사용한다.
  
```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    /**
     * 새로운 사용자를 생성하고, 생성된 사용자를 반환.
     * param : user 새로 생성할 사용자 정보.
     * return : 생성된 사용자 정보.
     */
    public User createUser(User user) {
        return userRepository.save(user);
    }

    /**
     * 주어진 사용자명(username)에 해당하는 사용자 정보를 조회.
     * param : username 조회할 사용자명.
     * return 사용자 정보가 존재하는 경우 해당 정보를, 그렇지 않은 경우 null을 반환.
     */
    public Optional<User> findByUsername(String username) {
        return userRepository.findByUsername(username);
    }

    // other methods
}
```

- 컨트롤러 클래스 정의
  - HTTP 요청을 처리하는 컨트롤러 클래스를 정의.
  - 이때, UserService 클래스를 주입받아 사용한다.
  
```java
@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserService userService;

    /**
     * 새로운 사용자를 생성하고, 생성된 사용자 정보를 반환.
     * param : user 생성할 사용자 정보
     * return : 생성된 사용자 정보
     */
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }

    /**
     * 주어진 사용자명(username)에 해당하는 사용자 정보를 조회.
     * param : username 조회할 사용자명
     * return : 사용자 정보가 존재하는 경우 해당 정보를, 그렇지 않은 경우 null을 반환.
     */
    @GetMapping("/{username}")
    public User getUser(@PathVariable String username) {
        Optional<User> user = userService.findByUsername(username);
        if (user.isPresent()) {
            return user.get();
        } else {
            throw new UserNotFoundException(username);
        }
    }

    // other methods
}
```

JPA는 이외에도 다양한 기능을 제공한다. JPQL(Java Persistence Query Language)을 사용하여 객체를 검색하고 조작하는 등의
작업을 수행할 수 있다.