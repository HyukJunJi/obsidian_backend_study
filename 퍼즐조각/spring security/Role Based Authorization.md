## **_User_, _Role_ and _Privilege_**

시작하기전에 세가지 메인 entity를 가집니다.

- 유저
- 역할은 시스텀에서 사용자의 상위 레벨의 역할을 나타냅니다. 각 역할에는 일련의 낮은 레벨의 권한이 있습니다.
- 권한은 시스템의 낮은 수준의 세부적인 특권/권한을 나타냅니다.

아래는 User 엔티티
```java
@Entity
public class User {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    private String firstName;
    private String lastName;
    private String email;
    private String password;
    private boolean enabled;
    private boolean tokenExpired;

    @ManyToMany 
    @JoinTable( 
        name = "users_roles", 
        joinColumns = @JoinColumn(
          name = "user_id", referencedColumnName = "id"), 
        inverseJoinColumns = @JoinColumn(
          name = "role_id", referencedColumnName = "id")) 
    private Collection<Role> roles;
}
```
User는 적절한 등록 메커니즘에 필요한 역할과 몇 가지 추가 세부 정보가 포함되어 있습니다.

아래는 Role 엔티티
```java
@Entity
public class Role {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    private String name;
    @ManyToMany(mappedBy = "roles")
    private Collection<User> users;

    @ManyToMany
    @JoinTable(
        name = "roles_privileges", 
        joinColumns = @JoinColumn(
          name = "role_id", referencedColumnName = "id"), 
        inverseJoinColumns = @JoinColumn(
          name = "privilege_id", referencedColumnName = "id"))
    private Collection<Privilege> privileges;
}
```

[출처](https://www.baeldung.com/role-and-privilege-for-spring-security-registration)