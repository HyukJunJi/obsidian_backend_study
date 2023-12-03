1.access = AccessLevel.PROTECTED 이거뭐야
2.strategy = GenerationType.IDENTITY 이거 뭐야
3.fetch = FetchType.LAZY 이거뭐야
4.@Enumerated(EnumType.STRING)
5.엔티티 잘 작성하는법?
```java
	@JsonIgnore 이거뭐야
``` 
[@JsonIgnore](https://velog.io/@hth9876/JsonIgnorePropertiesignoreUnknown-true)
어노테이션 manytomany가 있는데 왜 manytoone으로 하는지?
## access = AccessLevel.PROTECTED 이거뭐야
`@NoArgsConstructor(access = AccessLevel.PROTECTED)`은 Lombok 어노테이션 중 하나로, 해당 클래스에 인자가 없는 디폴트 생성자를 생성하되, 생성자의 접근 레벨(access level)을 `protected`로 지정하는 것을 나타냅니다.

여기에서 `@NoArgsConstructor`는 Lombok이 제공하는 어노테이션 중 하나로, 클래스에 디폴트 생성자를 자동으로 생성해줍니다. 디폴트 생성자란 매개변수를 가지지 않는 생성자를 말합니다.

`access` 속성은 생성된 생성자의 접근 레벨을 지정할 때 사용됩니다. `AccessLevel.PROTECTED`는 생성된 생성자를 `protected`로 만들라는 것을 의미합니다. 따라서 외부에서는 해당 생성자에 접근할 수 없고, 서브클래스에서는 사용할 수 있습니다.
```java
import lombok.NoArgsConstructor;
import lombok.AccessLevel;

@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Example {
    // 클래스 내용
}

```

이 클래스를 상속받은 서브클래스에서는 `protected` 생성자에 접근 가능합니다:

```java
public class SubExample extends Example { 
	public SubExample() { 
	// 부모 클래스인 Example의 protected 생성자에 접근 가능 
		super(); 
	} 
}
```

하지만 패키지 외부에서는 다음과 같이 직접 생성자를 호출할 수 없습니다:

```java
public class OtherClass {

    public void someMethod() {
        // Example 클래스의 protected 생성자에 직접 접근할 수 없음
        // 아래 코드는 컴파일 에러를 발생시킵니다.
        // Example example = new Example();
    }
}
```
즉, `@NoArgsConstructor(access = AccessLevel.PROTECTED)`를 사용하면 해당 클래스의 인스턴스를 직접 생성하는 것을 외부에서 막을 수 있고, 상속받은 서브클래스에서만 생성을 허용할 수 있습니다.

## @GeneratedValue(strategy = GenerationType.IDENTITY) 가 뭐야?

`@GeneratedValue(strategy = GenerationType.IDENTITY)`는 JPA(Java Persistence API)에서 엔터티의 기본 키를 자동으로 생성하는 전략(strategy) 중 하나를 설정하는 어노테이션입니다.

여기에서 `GenerationType.IDENTITY`는 데이터베이스에 의해 자동으로 생성된 키를 사용하는 전략을 나타냅니다. 이는 주로 데이터베이스에서 제공하는 자동 증가(auto-increment) 혹은 시퀀스(sequence)와 같은 방법을 활용하여 기본 키를 생성합니다.

**IDENTITY 전략:**

- 해당 전략은 데이터베이스에 의해 기본 키가 생성되는 경우에 사용됩니다.
- 대부분의 RDBMS에서는 AUTO_INCREMENT(또는 IDENTITY, SERIAL 등의 이름) 컬럼을 사용하여 자동으로 증가하는 값을 생성할 수 있습니다.
- 이 전략을 사용하면 JPA가 데이터베이스에 의존하여 새로운 엔터티의 기본 키를 생성합니다.

## fetch = FetchType.LAZY가 뭐야?

`fetch = FetchType.LAZY`는 JPA(Java Persistence API)에서 엔터티 간의 관계 매핑에서 사용되는 어노테이션 중 하나로, 연관된 엔터티를 지연 로딩(lazy loading)으로 설정하는 옵션입니다.

`FetchType.LAZY`는 연관된 엔터티의 데이터를 실제로 사용할 때 로딩하도록 지정하는 것입니다. 반면에 `FetchType.EAGER`는 연관된 엔터티를 즉시 로딩하도록 지정합니다.

```java
@Entity
public class ParentEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany(mappedBy = "parent", fetch = FetchType.LAZY)
    private List<ChildEntity> children;
    
    // 다른 필드 및 메서드들
}
```
위 코드에서 `@OneToMany` 어노테이션은 1:N(N:1) 관계를 매핑하는데 사용되었습니다. `fetch` 속성이 `FetchType.LAZY`로 설정되었으므로, `ParentEntity`를 조회할 때에는 `children` 필드에 대한 데이터를 가져오지 않고, 해당 필드에 접근하여 실제로 데이터를 사용할 때에 비로소 데이터를 로딩하게 됩니다.

이를 통해, 프로그램이 특정 연관된 엔터티의 데이터가 필요한 경우에만 로딩되도록 하여 성능을 최적화할 수 있습니다. 특히, 연관된 엔터티의 데이터가 크거나 불필요한 경우에는 지연 로딩을 사용하는 것이 권장됩니다.

## @Enumerated(EnumType.STRING)

`@Enumerated(EnumType.STRING)`은 JPA(Java Persistence API)에서 열거형(Enum) 타입을 매핑할 때 사용되는 어노테이션 중 하나입니다. 이 어노테이션은 열거형 상수를 데이터베이스의 문자열 값과 매핑하도록 지정합니다.

```java
public enum Status {
    ACTIVE, INACTIVE, PENDING
}
```
이 열거형을 JPA 엔터티 클래스에서 사용할 때, `@Enumerated(EnumType.STRING)`을 이용하여 데이터베이스에 문자열로 저장하도록 할 수 있습니다:

```java
@Entity
public class ExampleEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Enumerated(EnumType.STRING)
    @Column(name = "status")
    private Status status;

    // 다른 필드 및 메서드들
}
```
위의 예시에서 `@Enumerated(EnumType.STRING)`은 `status` 필드가 데이터베이스에 매핑될 때 문자열로 저장되어야 함을 나타냅니다. 이를 통해 데이터베이스에 저장될 때는 "ACTIVE", "INACTIVE", "PENDING"과 같이 문자열 형태로 저장되고, 조회할 때도 해당 문자열 값을 열거형으로 변환하여 사용할 수 있습니다.

이렇게 함으로써 데이터베이스에 저장되는 값이 의미를 가지고 있고, 가독성이 높아지며, 열거형의 순서가 변경되어도 안전하게 사용할 수 있습니다.

## @JoinColumn(name = "user_id", referencedColumnName = "id")가 뭐야

`@JoinColumn(name = "user_id", referencedColumnName = "id")`은 JPA(Java Persistence API)에서 엔터티 간의 관계를 매핑할 때 사용되는 어노테이션 중 하나입니다. 이 어노테이션은 두 엔터티 사이의 관계에서 외래 키(Foreign Key)를 지정하는 데 사용됩니다.

**`name` 속성:**

- 외래 키 컬럼의 이름을 지정합니다. 이는 현재 엔터티가 참조하는 대상 엔터티의 테이블에 생성될 외래 키의 이름입니다.
- 위 예제에서는 현재 엔터티의 테이블에 있는 외래 키 컬럼의 이름이 "user_id"로 지정되어 있습니다.

**`referencedColumnName` 속성:**

- 현재 엔터티가 참조하는 대상 엔터티의 기본 키(PK) 컬럼의 이름을 지정합니다. 즉, 현재 엔터티의 "user_id" 컬럼이 대상 엔터티의 "id" 컬럼을 참조하도록 지정됩니다.
- 이 속성을 생략할 경우, 기본적으로 대상 엔터티의 기본 키 컬럼명("id")이 사용됩니다.
```java
@Entity
public class Post {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    @JoinColumn(name = "user_id", referencedColumnName = "id")
    private User user;

    // 다른 필드 및 메서드들
}
```

위 코드에서 `@JoinColumn` 어노테이션을 통해 `Post` 엔터티의 "user_id" 컬럼이 `User` 엔터티의 "id" 컬럼을 참조하도록 설정되었습니다. 이를 통해 두 엔터티 간의 관계가 매핑되고, 외래 키가 설정됩니다.



[jpa 작성 요령](https://velog.io/@sonchanwoo/ERD-%EA%B4%80%EA%B3%84relationship%EC%99%80-JPA)