# role based authorization은 앱을 개발할때 어디에 사용 될까?
Spring Boot에서 Role-Based Authorization은 애플리케이션의 보안을 강화하고 특정 사용자 또는 사용자 그룹이 특정 작업에 대한 권한을 가지도록 하는 데 사용됩니다. Role-Based Authorization은 주로 다음과 같은 상황에서 활용될 수 있습니다:

1. **웹 애플리케이션 보안:**
    
    - 사용자가 로그인하고 특정 페이지에 액세스하려는 경우, 해당 사용자에게 필요한 역할(role)이 있는지 확인합니다. 예를 들어, 관리자는 관리자 페이지에만 액세스할 수 있도록 권한을 가지게 할 수 있습니다.
2. **API 엔드포인트 보호:**
    
    - API 엔드포인트에 대한 액세스를 제한하고, 특정 역할을 가진 사용자만이 특정 API를 호출할 수 있도록 합니다. 이는 애플리케이션의 민감한 기능에 대한 보안을 강화하는 데 도움이 됩니다.
3. **메소드 수준의 보안 제어:**
    
    - 특정 메소드 또는 서비스에 대한 권한을 설정하여, 해당 메소드를 호출하려면 특정 역할이 필요하도록 만들 수 있습니다. 이는 애플리케이션 내의 특정 로직이나 작업을 보호하는 데 사용될 수 있습니다.
4. **사용자 그룹 관리:**
    
    - 사용자 그룹을 만들고, 각 그룹에 특정 역할을 할당하여 사용자를 조직화할 수 있습니다. 이는 팀 또는 부서별로 액세스 권한을 관리하려는 경우에 유용합니다.
5. **동적인 권한 부여:**
    
    - 사용자의 행동이나 특정 조건에 따라 권한을 동적으로 부여할 수 있습니다. 예를 들어, 특정 조건이 충족되면 사용자에게 새로운 권한을 부여하거나 회수할 수 있습니다.
6. **보안 정책 적용:**
    
    - 애플리케이션 내에서 일관된 보안 정책을 적용하여 사용자 인증 및 권한 부여를 통합적으로 관리할 수 있습니다.

Spring Security와 같은 라이브러리를 사용하여 Spring Boot 애플리케이션에서 Role-Based Authorization을 구현할 수 있습니다. 이를 통해 보안적인 측면에서 더 견고하고 유연한 애플리케이션을 개발할 수 있습니다.

==아래는 구현예시이다==

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

아래는 Privilege 엔티티
```java
@Entity
public class Privilege {
 
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    private String name;

    @ManyToMany(mappedBy = "privileges")
    private Collection<Role> roles;
}
```
User <-> Role <-> Privilege 관계를 ManyToMany 관계로 양방향으로 간주하고 있습니다. 

## Privileges 와 Roles 설정하기
다음은, 시스템에서 권한과 역할을 초기에 설정하는데 중점을 두겠습니다.
이를 애플리케이션 시작과 연결하고 ContextRefreshedEvent에서 ApplicationListener를 사용하여 서버 시작 시 초기 데이터를 로드합니다.
```java
@Component
public class SetupDataLoader implements
  ApplicationListener<ContextRefreshedEvent> {

    boolean alreadySetup = false;

    @Autowired
    private UserRepository userRepository;
 
    @Autowired
    private RoleRepository roleRepository;
 
    @Autowired
    private PrivilegeRepository privilegeRepository;
 
    @Autowired
    private PasswordEncoder passwordEncoder;
 
    @Override
    @Transactional
    public void onApplicationEvent(ContextRefreshedEvent event) {
 
        if (alreadySetup)
            return;
        Privilege readPrivilege
          = createPrivilegeIfNotFound("READ_PRIVILEGE");
        Privilege writePrivilege
          = createPrivilegeIfNotFound("WRITE_PRIVILEGE");
 
        List<Privilege> adminPrivileges = Arrays.asList(
          readPrivilege, writePrivilege);
        createRoleIfNotFound("ROLE_ADMIN", adminPrivileges);
        createRoleIfNotFound("ROLE_USER", Arrays.asList(readPrivilege));

        Role adminRole = roleRepository.findByName("ROLE_ADMIN");
        User user = new User();
        user.setFirstName("Test");
        user.setLastName("Test");
        user.setPassword(passwordEncoder.encode("test"));
        user.setEmail("test@test.com");
        user.setRoles(Arrays.asList(adminRole));
        user.setEnabled(true);
        userRepository.save(user);

        alreadySetup = true;
    }

    @Transactional
    Privilege createPrivilegeIfNotFound(String name) {
 
        Privilege privilege = privilegeRepository.findByName(name);
        if (privilege == null) {
            privilege = new Privilege(name);
            privilegeRepository.save(privilege);
        }
        return privilege;
    }

    @Transactional
    Role createRoleIfNotFound(
      String name, Collection<Privilege> privileges) {
 
        Role role = roleRepository.findByName(name);
        if (role == null) {
            role = new Role(name);
            role.setPrivileges(privileges);
            roleRepository.save(role);
        }
        return role;
    }
}
```

설정 단계에서는 
- privileges를 생성했다
- roles를 생성하고 roles에 privileges를 부여했다.
- 마지막으로 a user를 생성하고 a user에 role을 부여했다.

설정을 실행해야 하는지 여부를 결정하기 위해 이미 설정 플래그를 사용하는 방법에 유의하세요.
이는 애플리케이션에서 구성한 컨텍스트 수에 따라 ContextRefreshedEvent가 여러 번 실행될 수 있기 때문입니다. 그리고 우리는 설정을 한 번만 실행하려고 합니다.

여기에 두 가지 간단한 메모가 있습니다. 먼저 용어를 살펴보겠습니다. 여기서는 권한 – Role 용어를 사용하고 있습니다. 그러나 spring에는 이것이 약간 다릅니다.
우리의 권한은 역할이라고도 하며 (부여된) 권한이라고도 하는데 이는 약간 혼란스럽습니다.

## **Custom _UserDetailsService_**
이제 인증 과정을 살펴보겠습니다.

Custom UserDetailsService 내에서 사용자를 검색하는 방법과 사용자에게 할당한 역할 및 권한에서 올바른 권한 집합을 매핑하는 방법을 살펴보겠습니다.

```java
@Service("userDetailsService")
@Transactional
public class MyUserDetailsService implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;
 
    @Autowired
    private IUserService service;
 
    @Autowired
    private MessageSource messages;
 
    @Autowired
    private RoleRepository roleRepository;

    @Override
    public UserDetails loadUserByUsername(String email)
      throws UsernameNotFoundException {
 
        User user = userRepository.findByEmail(email);
        if (user == null) {
            return new org.springframework.security.core.userdetails.User(
              " ", " ", true, true, true, true, 
              getAuthorities(Arrays.asList(
                roleRepository.findByName("ROLE_USER"))));
        }

        return new org.springframework.security.core.userdetails.User(
          user.getEmail(), user.getPassword(), user.isEnabled(), true, true, 
          true, getAuthorities(user.getRoles()));
    }

    private Collection<? extends GrantedAuthority> getAuthorities(
      Collection<Role> roles) {
 
        return getGrantedAuthorities(getPrivileges(roles));
    }

    private List<String> getPrivileges(Collection<Role> roles) {
 
        List<String> privileges = new ArrayList<>();
        List<Privilege> collection = new ArrayList<>();
        for (Role role : roles) {
            privileges.add(role.getName());
            collection.addAll(role.getPrivileges());
        }
        for (Privilege item : collection) {
            privileges.add(item.getName());
        }
        return privileges;
    }

    private List<GrantedAuthority> getGrantedAuthorities(List<String> privileges) {
        List<GrantedAuthority> authorities = new ArrayList<>();
        for (String privilege : privileges) {
            authorities.add(new SimpleGrantedAuthority(privilege));
        }
        return authorities;
    }
}
```
여기서 따라야 할 흥미로운 점은 권한(및 역할)이 GrantedAuthority 엔터티에 매핑되는 방식입니다.
이 매핑은 전체 보안 구성을 매우 유연하고 강력하게 만듭니다. 필요에 따라 역할과 권한을 세부적으로 혼합하고 일치시킬 수 있으며, 최종적으로 권한에 올바르게 매핑되고 프레임워크로 다시 반환됩니다.
## Role Hierarchy(역할 계층)
또한 역할을 계층 구조로 구성해 보겠습니다.
우리는 권한을 역할에 매핑하여 역할 기반 액세스 제어를 구현하는 방법을 살펴보았습니다. 이를 통해 모든 개별 권한을 할당할 필요 없이 사용자에게 단일 역할을 할당할 수 있습니다.

그러나 역할 수가 증가하면 사용자에게 여러 역할이 필요할 수 있으며 이로 인해 역할이 폭발적으로 증가할 수 있습니다:
![사진](https://www.baeldung.com/wp-content/uploads/2015/01/role-explosion.jpg)
이를 극복하기 위해 Spring Security의 역할 계층을 사용할 수 있습니다.
![사진](https://www.baeldung.com/wp-content/uploads/2015/01/role-h.jpg)
ADMIN 역할을 할당하면 사용자에게 STAFF 및 USER 역할 모두의 권한이 자동으로 부여됩니다.

그러나 STAFF 역할을 가진 사용자는 STAFF 및 USER 역할 작업만 수행할 수 있습니다.

RoleHierarchy 유형의 빈을 생성하여 Spring Security에서 이 계층 구조를 생성해 보겠습니다.

```java
@Bean
public RoleHierarchy roleHierarchy() {
    RoleHierarchyImpl roleHierarchy = new RoleHierarchyImpl();
    String hierarchy = "ROLE_ADMIN > ROLE_STAFF \n ROLE_STAFF > ROLE_USER";
    roleHierarchy.setHierarchy(hierarchy);
    return roleHierarchy;
}
```

역할 계층을 정의하기 위해 표현식에서 > 기호를 사용합니다. 여기서는 STAFF 역할을 포함하도록 ADMIN 역할을 구성했으며, 이 역할에는 USER 역할도 포함됩니다.

Spring Web Expressions에 이 역할 계층을 포함하기 위해 WebSecurityExpressionHandler에 roleHierarchy 인스턴스를 추가합니다.

```java
@Bean
public DefaultWebSecurityExpressionHandler customWebSecurityExpressionHandler() {
    DefaultWebSecurityExpressionHandler expressionHandler = new DefaultWebSecurityExpressionHandler();
    expressionHandler.setRoleHierarchy(roleHierarchy());
    return expressionHandler;
}
```

마지막으로 http.authorizeRequests()에 표현식 핸들러를 추가합니다.

```java
@Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf()
            .disable()
            .authorizeRequests()
                .expressionHandler(webSecurityExpressionHandler())
                .antMatchers(HttpMethod.GET, "/roleHierarchy")
                .hasRole("STAFF")
    ...
}
```
webSecurityExpressionHandler가 작동하고 있음을 증명하기 위해 엔드포인트 /roleHierarchy가 ROLE_STAFF로 보호됩니다. 보시다시피 역할 계층은 사용자에게 추가해야 하는 역할과 권한의 수를 줄이는 좋은 방법입니다.

## User 등록

마지막으로 신규 사용자 등록에 대해 살펴보겠습니다.
우리는 사용자를 생성하고 사용자에게 역할(및 권한)을 할당하는 설정 방법을 살펴보았습니다. 이제 새 사용자를 등록하는 동안 이 작업이 어떻게 수행되어야 하는지 살펴보겠습니다.
```java
@Override
public User registerNewUserAccount(UserDto accountDto) throws EmailExistsException {
 
    if (emailExist(accountDto.getEmail())) {
        throw new EmailExistsException
          ("There is an account with that email adress: " + accountDto.getEmail());
    }
    User user = new User();

    user.setFirstName(accountDto.getFirstName());
    user.setLastName(accountDto.getLastName());
    user.setPassword(passwordEncoder.encode(accountDto.getPassword()));
    user.setEmail(accountDto.getEmail());

    user.setRoles(Arrays.asList(roleRepository.findByName("ROLE_USER")));
    return repository.save(user);
}
```

이 간단한 구현에서는 표준 사용자가 등록된다고 가정하므로 ROLE_USER 역할을 할당합니다.

물론, 여러 개의 하드 코딩된 등록 방법을 사용하거나 클라이언트가 등록 중인 사용자 유형을 보낼 수 있도록 허용함으로써 더 복잡한 논리를 동일한 방식으로 쉽게 구현할 수 있습니다.

[출처](https://www.baeldung.com/role-and-privilege-for-spring-security-registration)

[한글 자료 출처](https://velog.io/@code12/Spring-Security-Spring-Boot%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-Role-Based-Authorization)