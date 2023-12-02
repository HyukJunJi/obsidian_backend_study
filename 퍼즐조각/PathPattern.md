---
tags:
  - "#필터"
  - "#인터셉터"
---
PathPattern이란, 스프링 부트에서 `@RequestMapping` 시에 사용되는 문자 패턴이다.

공식 문서에 따르면, 특정성을 위해 패턴을 비교할 때, 모호성을 제거하기 위해 사용한다고 되어있다.
### `?`: 한 문자 일치

- `/pages/t?st.html`
    - YES: `/pages/test.html`, `/pages/tXst.html`
    - NO : `/pages/toast.html`

### `*`: 경로(`/`) 안의 모든 문자 일치

- `/resources/*.png`
    - YES: `/resources/photo.png`
    - NO : `/resources/favority.ico`

### `{spring}`: spring 이라는 변수로 캡처

- `/resources/{path}`
    - `/resources/robot.txt` -> `path`변수에 "robot.txt" 할당
    - `@PathVariable("path")`로 접근 가능

### `{*spring}`: 하위 경로 끝까지 `spring`변수에 캡쳐

- `/items/{*path}`
    - `/items/1/add` -> `path`변수에 "/1/add" 할당

### `{spring:[a-z]+}`: 정규식 이용

- `/items/{path:[a-z]+}`
    - YES: `/items/robots`
    - NO : `/items/123`

## 예제1 - `{*spring}`

```java
@GetMapping("/hello/{*name}")
@ResponseBody
public String handleTest(
        @PathVariable String name
) {
    log.info("name = {}", name);
    return name;
}
```

### 결과 1

```http
[REQUEST]
GET http://localhost:8080/hello/path-test

[LOG]
name = /path-test

[RESPONSE]
/path-test
```

### 결과 2

```java
[REQUEST]
GET http://localhost:8080/hello/path-test/other

[LOG]
name = /path-test/other

[RESPONSE]
/path-test/other
```

## 예제 2 - 정규식

```java
@GetMapping("/static/{name:[a-z-]+}-{version:\\d\\.\\d\\.\\d}{ext:\\.[a-z]+}")
@ResponseBody
public String handle(
        @PathVariable String name, 
        @PathVariable String version, 
        @PathVariable String ext
) {
    log.info("name = {}", name);
    log.info("version = {}", version);
    log.info("ext = {}", ext);
    
    return "/" + name + "-" + version + "." + ext;
}
```

### 결과

```java
[REQUEST]
GET http://localhost:8080/pathtest-1.0.0.jar

[LOG]
name = pathtest
version = 1.0.0
ext = .jar

[RESPONSE]
/pathtest-1.0.0.jar
```

## 잘못된 사용

```java
@GetMapping("/static/{*fullpath}{name}")
@ResponseBody
public String runtimeError(
        @PathVariable String fullpath,
        @PathVariable String name
) {
    log.info("fullpath = {}", fullpath);
    log.info("name = {}", name);

    return name;
}
```

```java
Description:
Invalid mapping pattern detected: /static/{*fullpath}{name}
                   ^
No more pattern data allowed after {*...} or ** pattern element
```
[출처](https://velog.io/@ksk7584/PathPattern)
[공식문서](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/util/pattern/PathPattern.html)