1.access = AccessLevel.PROTECTED 이거뭐야
2.strategy = GenerationType.IDENTITY 이거 뭐야
3.fetch = FetchType.LAZY 이거뭐야
4.@Enumerated(EnumType.STRING)
엔티티 잘 작성하는법?
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
[jpa 작성 요령](https://velog.io/@sonchanwoo/ERD-%EA%B4%80%EA%B3%84relationship%EC%99%80-JPA)