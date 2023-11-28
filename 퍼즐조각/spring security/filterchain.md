스프링 부트 3.0에서는 스프링 시큐리티 6.0.0 버전이 적용되었습니다. 2.7.3 이후로 시큐리티 설정쪽에 deprecated 되는게 추가되었고 3.0부터는 아예 삭제된 설정 방식들이 있습니다.
기존의 방식과 다른 점은 반환 값이 있고 Bean으로 등족한다는 점입니다.
==**SecurityFilterChain을 반환하고 Bean으로 등록함으로써 컴포넌트 기반의 보안설정이 가능해진다.**==

>주로 등록되는 Bean 종류
```java
@Bean
public UserDetailsService userDetailsService()

@Bean
public PasswordEncoder passwordEncoder()

@Bean
public SecurityFilterChain filterChain(HttpSecurity http)

@Bean
public WebSecurityCustomizer webSecurityCustomizer()
```
### Spring Security 적용방법
##### Gradle
```java
implementation 'org.springframework.boot:spring-boot-starter-security'
```
##### Maven
```java
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```
의존성을 추가해주게 되면 모든 경로에 대해서 로긍
