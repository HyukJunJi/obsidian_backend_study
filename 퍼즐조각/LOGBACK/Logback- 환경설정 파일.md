![사진](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvLWWc%2FbtrDrPsYyVt%2FKZHFPKUVIKTlesJG8CKxe0%2Fimg.webp)

오늘은 로그백을 사용할 때, `logback-spring.xml` 을 작성하여서 활용을 하는데, 이 환경설정 파일에 대해서 알아보도록 하겠습니다.
## logback 환경설정 파일

스프링부트에서 로그백을 사용하기 위해서 다음 파일을 로드해서 사용합니다.

- `logback-spring.xml`
- `logback-spring.groovy`
- `logback.xml`
- `logback.groovy`
-  `application-dev.yaml 개발 환경 파일`
- `application-prod.yaml 운영 환경 파일`

가능하면 로깅 설정에는 `-spring` 붙어 있는 파일을 사용하는 것을 권장합니다.

- `logback.xml` 파일은 너무 빨리 로드되기 때문에, 이 파일 안에선 Extensions을 사용할 수 없습니다.
- 스프링에서 로그 초기화를 완전히 제어할 수 없습니다.

### 로그백 기본 설정

로그백은 기본적인 설정이 되어 있습니다. Console 및 File 로그 패턴이 설정되어 있으며, 초기 로거 또한 설정되어 있습니다. 그리고 `appender` 별로 각 설정 파일들이 분리되어 있으며, `base.xml` 에서 분리된 설정 파일들 포함하여 제공합니다.

`base.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<included>
    <include resource="org/springframework/boot/logging/logback/defaults.xml" />
    <property name="LOG_FILE" value="${LOG_FILE:-${LOG_PATH:-${LOG_TEMP:-${java.io.tmpdir:-/tmp}}}/spring.log}"/>
    <include resource="org/springframework/boot/logging/logback/console-appender.xml" />
    <include resource="org/springframework/boot/logging/logback/file-appender.xml" />
    <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="FILE" />
    </root>
</included>
```

<included>
    <include resource="org/springframework/boot/logging/logback/defaults.xml" />
    <property name="LOG_FILE" value="${LOG_FILE:-${LOG_PATH:-${LOG_TEMP:-${java.io.tmpdir:-/tmp}}}/spring.log}"/>
    <include resource="org/springframework/boot/logging/logback/console-appender.xml" />
    <include resource="org/springframework/boot/logging/logback/file-appender.xml" />
    <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="FILE" />
    </root>
</included>아래는 기본적으로 설정된 패턴 및 로거 정보 입니다.

`defaults.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<included>
    <conversionRule conversionWord="clr" converterClass="org.springframework.boot.logging.logback.ColorConverter" />
    <conversionRule conversionWord="wex" converterClass="org.springframework.boot.logging.logback.WhitespaceThrowableProxyConverter" />
    <conversionRule conversionWord="wEx" converterClass="org.springframework.boot.logging.logback.ExtendedWhitespaceThrowableProxyConverter" />

    <property name="CONSOLE_LOG_PATTERN" value="${CONSOLE_LOG_PATTERN:-%clr(%d{${LOG_DATEFORMAT_PATTERN:-yyyy-MM-dd HH:mm:ss.SSS}}){faint} %clr(${LOG_LEVEL_PATTERN:-%5p}) %clr(${PID:- }){magenta} %clr(---){faint} %clr([%15.15t]){faint} %clr(%-40.40logger{39}){cyan} %clr(:){faint} %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}}"/>
    <property name="CONSOLE_LOG_CHARSET" value="${CONSOLE_LOG_CHARSET:-${file.encoding:-UTF-8}}"/>
    <property name="FILE_LOG_PATTERN" value="${FILE_LOG_PATTERN:-%d{${LOG_DATEFORMAT_PATTERN:-yyyy-MM-dd HH:mm:ss.SSS}} ${LOG_LEVEL_PATTERN:-%5p} ${PID:- } --- [%t] %-40.40logger{39} : %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}}"/>
    <property name="FILE_LOG_CHARSET" value="${FILE_LOG_CHARSET:-${file.encoding:-UTF-8}}"/>

    <logger name="org.apache.catalina.startup.DigesterFactory" level="ERROR"/>
    <logger name="org.apache.catalina.util.LifecycleBase" level="ERROR"/>
    <logger name="org.apache.coyote.http11.Http11NioProtocol" level="WARN"/>
    <logger name="org.apache.sshd.common.util.SecurityUtils" level="WARN"/>
    <logger name="org.apache.tomcat.util.net.NioSelectorPool" level="WARN"/>
    <logger name="org.eclipse.jetty.util.component.AbstractLifeCycle" level="ERROR"/>
    <logger name="org.hibernate.validator.internal.util.Version" level="WARN"/>
    <logger name="org.springframework.boot.actuate.endpoint.jmx" level="WARN"/>
</included>
```
### 환경 설정 - logback-spring.xml 활용

`ConsoleAppender` 를 활용한 기본 예제는 다음과 같습니다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<included>
    <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
        <!-- 해당 로깅의 패턴을 설정 -->
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>%cyan([%contextName]) - %magenta([%d{yyyy.MM.dd HH:mm:ss.SSS}]) - %highlight([%-5level]) - [%thread] - [%X{traceId}] - %green([%logger{5}]) - [%marker] - %msg%n</pattern>
        </encoder>
    </appender>
</included>
```

`encoder`의 기본 설정은 `PatternLayoutEncoder` 입니다.
### 상태 출력

환경설정 파일을 분석하는 동안 경고나 오류가 발생하면 Logback은 콘솔에 상태 메시지를 기록합니다. 해당 로그를 보기 위해서는 다음과 같이 설정하면 됩니다.
```xml
<configuration>
  <statusListener class="ch.qos.logback.core.status.OnConsoleStatusListener" />

  ... the rest of the configuration file
</configuration>
```
### 환경설정파일 위치 지정

시스템 속성을 사용하여 구성 파일의 위치를 지정할 수 있습니다.
```
java jar -Dlogging.config=file:/path/to/config.xml MyApp1.jar
```
>[!info]
>로컬에서 개발할때는 사용하진 않지만 개발 서버 또는 운영 서버에서 환경 설정 파일을 외부로 관리하면서 어플리케이션을 실행 및 관리할때 자주 사용합니다.

### 자동 로드 스캔 환경설정 파일

구성 파일의 변경 사항을 확인하고 자동으로 재구성됩니다. 기본적으로 구성 파일의 변경 내용은 1분마다 검색합니다.

```xml
<configuration scan="true">   ... </configuration>
```
아래 내용을 참고하면 설정을 통해서 시간 변경을 할 수 있습니다.
```xml
<configuration scan="true" scanPeriod="30 seconds" >   ... </configuration> 
```
로그백의 ==Extensions==기능을 사용한다면 자동 로드 스캔 사용할 수 없습니다.
#### ==Extensions이란?==

스프링 부트의 고급 설정을 제공하며, `<springProfiles>` 태그 및 `<springProperty>` 태그를 활용할 수 있습니다.

이러한 설정은 `logback-spring.xml`에서만 사용할 수 있습니다.

### 스택트레이스 활성화

로그백은 출력하는 스택 추적 라인에 대한 패키징 데이터를 포함할 수 있습니다.

- 패키징 데이터는 소프트웨어 버전 관리 문제를 식별하는 데 매우 유용할 수 있습니다.
- 데이터 패키징은 유용하지만 빈번한 예외가 있는 경우 애플리케이션에서 계산 비용이 많이 듭니다.

```xml
<configuration packagingData="true">   ... </configuration>
```
### 파일 작성 참고

- logback 구성 파일의 가장 기본적인 큰 구조는 다음과 같습니다.
    
    - `<appender>` 요소
    - `<logger>` 요소
    - `<root>` 요소
    
- 규칙과 관련된 태그 이름은 대소문자를 구분하지 않습니다.
    
    - `<logger>`와 `<Logger>` , `<LOGGER>` 는 동일한 방식으로 해석됩니다.
    
- xml 태그는 작성 규칙을 따라야 합니다.
    
    - `<xyz>` 로 여는 경우 태그는 `</xyz>` 로 닫아야 합니다.
    - `</XyZ>` 는 작동하지 않습니다.

### 로거 설정 `<logger>`
| 구성       | 필수값 | 값                                         |
| ---------- | ------ | ------------------------------------------ |
| name       | YES    | 이름, 패키지 등                            |
| level      | NO     | TRACE, DEBUG, INFO, WAR, ERROR, ALL or OFF |
| additivity | NO     | true or false                               |
- `<logger>`요소는 `<appender-ref>` 요소를 포함할 수 있습니다.
	- 이렇게 참조된 각 appender는 해당 로거에 추가됩니다.

### 루트 로거 설정 `<root>`
| 구성  | 필수값 | 값  |
| ----- | ------ | --- |
| level | NO     | TRACE, DEBUG, INFO, WAR, ERROR, ALL or OFF    |

루트 로거는 다음과 같은 특징을 갖고 있습니다.

- 이미 `ROOT`로 이름이 지정되어 있으므로 name 속성을 허용하지 않습니다.
- level 속성의 값은 대소문자를 구분하지 않습니다.
- 루트 로거는 상속 또는 NULL로 설정할 수 없습니다.
- `<root>` 요소는 `<appender-ref>` 요소를 포함할 수 있습니다.
    - 이렇게 참조된 각 appender는 루트 로거에 추가됩니다.
아래는 루트 로거와 로거를 이용한 샘플입니다.

```XML
<configuration>

  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>
        %d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n
     </pattern>
    </encoder>
  </appender>

  <logger name="chapters.configuration" level="INFO" />
  <logger name="chapters.configuration.Foo" level="DEBUG" />

  <root level="DEBUG">
    <appender-ref ref="STDOUT" />
  </root>

</configuration>
```
- `<encoder>` 의 기본 설정은 `ch.qos.logback.classic.encoder.PatternLayoutEncoder` 입니다
- `<root>` 의 log level 설정은 `DEBUG` 입니다.
- 위 샘플 `logback-spring.xml` 의 설정된 로그 레벨을 나타내면 다음과 같습니다.

| 로거이름                      | 설정된 로그 레벨 | 영향을 받은 로그 레벨 |
| ----------------------------- | ---------------- | --------------------- |
| root                          | `DEBUG`          | `DEBUG`               |
| chapters.configuration        | `INFO`           | `INFO`                |
| chapters.configuration.MyApp3 | `null`           | `INFO`                |
| chapters.configuration.Foo    | `DEBUG`          | `DEBUG`                      |

### 어펜더 설정 `<appender>`
| 구성  | 필수값 | 값        |
| ----- | ------ | --------- |
| name  | YES    | 이름 지정 |
| class | YES    | 인스턴스화할 appender 클래스 이름          |

`<appender>`요소는 다음을 포함할 수 있습니다.
- `<layout>`
	- 사용자의 요청에 따라 로그 메세지의 포맷을 지정합니다.
	- 레이아웃 클래스가 `PatternLayout`인 경우 기본 클래스 매칭 규칙으로 생략이 가능합니다.
- `<encoder>`
	- `Appender`에 포함되어 사용자가 지정한 형식으로 표현될 로그 메세지를 변환하는 역할 담당합니다.
	- 바이트를 소유하고 있는 `appender`가 관리하는 `OutputStream`에 쓸 시간과 내용을 제어 할 수 있습니다.
	- `FileAppender`와 하위 클래스는 `encoder`를 필요로하므로 `layout`대신 `encoder`를 사용하면 됩니다.
	- 인코더 클래스가 `PatternlayoutEncoder`인 경우 기본 클래스 매핑 규칙에 지정된 대로 클래스 속성을 생략 가능합니다.
- `<filter>`
	- 해당 패키지에 반드시 로그를 찍지 않고 필터링이 필요한 경우에 사용하는 기능입니다.
	- 예를 들어, 레벨 필터를 추가해서 error 단계인 로그만 찍도록 설정 가능합니다.
##### 어펜더 로깅 중복

다음과 같이 `appender-ref` 설정하면 로깅이 중복이 됩니다.
```xml
<configuration>

  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
  </appender>

  <logger name="chapters.configuration">
    <appender-ref ref="STDOUT" />
  </logger>

  <root level="debug">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```


[base.xml, default.xml 위치](https://jaehyun8719.github.io/2019/05/17/springboot/logback-spring/)