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
---
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
의존성을 추가해주게 되면 모든 경로에 대해서 로그인화면이 보이게 된다. 즉, `현재 프로젝트는 모든 페이지에 대해서 로그인을 통해 접근할 수 있다.`
- 따라서 원하는 페이지에 대해서는 비회원도 접근 가능하게 하고, 회원만 접근하도록 
  `Spring Security 설정을 추가해주어야한다.`
---
### Spring Security 설정
>Spring Security를 위해서는 Configuration을 추가해주어야합니다.
>아래 사진과 같이 SecurityConfig 클래스를 추가해주었습니다.
>![[Pasted image 20231128222704.png]]
>과거에는 WebSecurityConfigurerAdapter를 상속하여 configure 메소드를 오버라이딩하여 구현하였지만, 현재는 `@Bean` 어노테이션을 사용하고 있습니다.
>
>유의사항
>	1. `@Bean`으로 등록되는 메소드 이름은 filterChain으로 설정
>	2. `@Configuration`,`@EnableWebSecurity` 어노테이션 설정
>	3. Spring Security의 각종 설정은 HttpSecurity로 대부분 이루어진다.
>		1. URL 접근 권한 설정
>		2. 인증전체 흐름에 필요한 Login, Logout 페이지 인증완료 후 페이지 인증 실패 시 이동 페이지 등등 설정
>		3. 인증로직을 커스텀하기 위한 커스텀 필터 설정
>		4. 기타 csrf, 강제 https 호출 등등 거의 모든 설정

### Spring Security 세부 설정
리소스 권한 설정
```java
@Bean
SecurityFilterChain filterChain(HttpSecurity http){
	http.authorizeHttpRequest()
    	.requestMatchers(new
    	AntPathRequestMatchers("/question/create**")).authenticated() 
    	// question/create경로는 모두 인증 
        .requestMatchers(new
        AntPathRequestMatchers("/question/modify/**").authenticated() 
        // question/modify/경로는 모두 인증
        .anyRequest().permitAll();  // 이외 모든 요청은 인증x
}
```
---