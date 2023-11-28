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
### requestMatchers(new AntPathRequestMatchers())
특정 리소스에 대해서 권한 설정
```java
requestMatchers(new AntPathRequestMatchers(리소스))
```
---
### permitAll()
requestMatchers 설정한 리소스의 접근을 인증절차 없이 허용의미

```java
	requestMatchers(new AntPathRequestMatchers(리소스)).permitAll()
```
---
### hasAnyRole()

리소스에 대한 모든 URL은 인증 후 ==ADMIN 레벨의 권한을 가진 사용자만 접근 허용==

```java
	requestMatchers(new AntPathRequestMatchers(리소스)).hasAnyRole("ADMIN")
```
---
### anyRequest()

모든 리소스를 의미하며 접근허용 리소스 및 인증 후 특정 레벨의 권한을 가진 사용자만 접근가능한 리소스를 설정하고 ==그 외 나머지 리소스들을 의미==

```java
	anyRequest().authenticated()
```

따라서 authenticated()일 경우, 나머지 리소스들은 무조건 인증을 완료해야한다는 의미

## 로그인 처리 설정
로그인 처리 설정을 하지 않으면 허용되지 않는 리소스에 대해서 403에러가 발생한다.
```java
@Bean
SecurityFilterChain filterChain(HttpSecurity http){
	http.formLogin()
    	.loginPage("/login")
        .loginProcessingUrl("/login-process")
        .defaultSuccessUrl("/main")
        .successHandler(new CustomAuthenticationSuccessHandler("/main"))
        .failureUrl("login-fail")
        .failureHandler(new CustomAuthenticationSuccessHandler("/login-fail"))
}
```
---
### formLogin()
로그인 페이지와 기타 로그인 처리 및 성공 실패 처리를 사용하겠다는 의미
```java
http.formLogin()
```
---
### loginPage()
Spring에서 제공하는 login 페이지가 아니라 사용자가 커스텀한 로그인 페이지를 사용할때 사용
```java
loginPage("/login-page")
```
---
### loginProcessingUrl()
인증처리를 하는 URL 설정하며, "/login-process"가 호출되면 인증처리를 수행하는 필터 호출
```java
loginProcessingUrl("/login-process")
```
---
### defaultSuccessUrl()
정상적으로 인증 성공 시 이동하는 페이지
```java
defaultSuccessUrl("/main")
```
---
### successHandler()

정상 인증 성공 후 별도의 처리가 필요한 경우 커스텀 핸들러 생성하여 등록

```java
successHandler(new CustomAuthenticationSuccessHandler("/main"))
```

❗️ ==커스텀 핸들러를 사용하기 때문에 CustomAuthenticationSuccessHandler는 직접 구현해야한다.==

---
### failureUrl()
인증 실패 시 이동하는 페이지
```java
failureUrl("/login-fail")
```

---
### failureHandler()

인증 실패 후 별도의 처리가 필요한 경우 커스텀 핸들러를 생성하여 등록

```java
failureHandler(new CustomAuthenticationSuccessHandler("/login-fail"))
```

[출처](https://velog.io/@wooyong99/Spring-Security)