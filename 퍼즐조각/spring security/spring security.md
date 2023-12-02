## Starting Hello Spring Security Boot
spring boot에 build.gradle 부분에 
spring security를 implementation을 해주면 실행하고 만들어 놓은 controller에 path로 접근하면
![[Pasted image 20231130112208.png]]
이러한 창이 뜨는데 spring security에서 막는 것이다. 이것에 대한 비번은 실행할때
![[Pasted image 20231130112259.png]]
나오고 Username은 user이다.
접근을 하면 
```HTML
<html xmlns:th="https://www.thymeleaf.org">  
<head>  
    <title>Hello Security!</title>  
</head>  
<body>  
    <h1>Hello Security</h1>  
    <a th:href="@{/logout}">Log Out</a>  
</body>  
</html>
```
![[Pasted image 20231130112416.png]]
실행이되고 
![[Pasted image 20231130112431.png]]
![[Pasted image 20231130112448.png]]
로그아웃도 할 수 있다.
Spring Boot와 Spring Security의 기본 배치는 런타임 시 다음 동작을 제공합니다.
## Runtime Expecations (런타임 기대치)

- 인증된 사용자가 필요합니다[모든 엔드포인트](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FAuthorize%20HttpServletRequests)(Boot의 `/error` 엔드포인트 포함)
    
- [시작 시 생성된 비밀번호로 기본 사용자를 등록](UserDetailsService)합니다(비밀번호는 콘솔에 기록됩니다. 앞의 예에서 비밀번호는 `8e557245-73e2-4286-969a-ff57fe326336` )
    
- BCrypt를 사용하여 [비밀번호 저장](PasswordEncoder) 및 기타를 보호합니다.
    
- 양식 기반 [로그인](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FHTTP%20Basic%20Authentication%2FForm%20Login) 및 [로그아웃](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FHTTP%20Basic%20Authentication%2FHandling%20Logouts) 흐름 제공< /span>
    
- 인증[양식 기반 로그인](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FHTTP%20Basic%20Authentication%2FForm%20Login) 및 [HTTP 기본 인증](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FHTTP%20Basic%20Authentication%2FHTTP%20Basic%20Authentication)
    
- 콘텐츠 협상을 제공합니다. 웹 요청의 경우 로그인 페이지로 리디렉션됩니다. 서비스 요청의 경우`401 Unauthorized`
    
- [CSRF 완화](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FCross%20Site%20Request%20Forgery%20(CSRF)) 공격
    
- [세션 고정](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FUnderstanding%20Session%20Fixation%20Attack%20Protection) 공격 완화
    
- 쓰기[엄격한 전송 보안](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FHTTP%20Strict%20Transport%20Security%20(HSTS))하여 [HTTPS 보장](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security)
    
- 쓰기[X-Content-Type-Options](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FContent%20Type%20Options)를 통해 [스니핑 공격 완화](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html#x-content-type-options)
    
- 인증된 리소스를 보호하는 쓰기[캐시 제어 헤더](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FCache%20Control)
    
- 쓰기 [X-Frame-Options](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FX-Frame-Options) 완화 [클릭재킹](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html#x-frame-options)
    
- [`HttpServletRequest`의 인증 방법과 통합](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FServlet%20API%20integration)
    
- 게시[인증 성공 및 실패 이벤트](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FAuthentication%20Events)

[[Role Based Authorization]]
[[퍼즐조각/spring security/filterchain]]
[[로그인 실패 시 오류 메세지]]
[[Authorize HttpServletRequests]]
[[Servlet Authentication Architecture]]
[Spring Security 6.2 build Gradle](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%ED%8D%BC%EC%A6%90%EC%A1%B0%EA%B0%81%2Fspring%20security%2FSpring%20Security%206.2%20build%20Gradle)

[이거 한번보자](https://velog.io/@shon5544/Spring-Security-1.-%EC%84%A4%EC%A0%95)