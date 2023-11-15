---
tags: network, http, Internet
---
- 하이퍼텍스트(HTML) 문서를 교환하기 위해 만들어진 protocol(통신 규약).
- 즉 웹상에서 네트워크로 서버끼리 통신을 할때 어떠한 형식으로 서로 통신을 하자고 규정해 놓은 "통신 형식" 혹은 "통신 구조" 라고 보면 된다.  
    프론트앤드 서버와 클라이언트간의 통신에 사용된다.  
    또한 백앤드와 프론트앤드 서버간에의 통신에도 사용된다.
- HTTP는 TCP/IP 기반으로 되어있다.
- HTTP 기본적으로 **request**(요청)/**response**(응답) 구조로 되어있다.  
    클라이언트가 HTTP request를 서버에 보내면 서버는 HTTP response를 보내는 구조  
    클라이언트와 서버의 모든 통신이 요청과 응답으로 이루어 진다.



--- 
## 공통 헤더(General Header)

- Date : HTTP 메세지가 만들어진 시각이다.
- Connection: HTTP/1.1을 사용하는데 기본적으로 `Keep-alive`로 적용되어있다. 접속 유지 여부라서 큰 의미는 없다.

### Cache-Control을 알기전에 브라우저의 캐시란 무엇인가?

간단히 말해서 서버 지연을 줄이기 위해 정적인 에셋(Image, HTML, CSS, JS), 즉 사본을 임시저장 하기위한 기술로서 웹 캐시의 일종이다.

- Cache-Control: HTTP/1.1 부터 도입된 헤더이며, 설정가능한 여러 옵션이 존재한다.
    - no-cache: 브라우저가 캐시를 참조하지 않고, 서버에 대해 컨텐츠 유효성을 검사하도록 지시한다. 즉 서버에서 캐시 저장여부를 물어본다.
    - no-store: 아무것도 캐싱하지 않겠다는 내용.
    - must-revalidate: 만료된 캐시만 서버에 확인을 받도록 한다.
    - public: 컨텐츠를 공개한다. 브라우저외에 중개 서버에 저장 허용.
    - private: 특정 사용자 환경(오직 브라우저)에만 캐시 저장허용.
    - max-age: 캐시 유효시간을 설정한다. 초(seconds)단위로 값을 할당 한다.
    - s-maxage: 공유캐시에서만 동작하며, Max-age, Expires 헤더를 재정의한다.

```js
cache-control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
```

- Expires: 응답 컨텐츠가 언제 만료되었는지 나타내며, Cache-control의 max-age가 있는 경우 이 헤더는 무시된다.
- ## Cache-Control은 아래에 그림을 보면 나온다.

## 앞에 X가붙은 헤더

일명 커스텀 등록 헤더는 RFC 6648이라는 비표준 빌드가 표준이 되기 이전에 사용자 임의로 붙여 임의로 정의한 헤더라는 것을 설명한다. 2012년 6월에 폐기되어 지금은 사용하지 않지만 대표적으로 몇가지가 눈에 띄는 것들이 있다. 그것들을 알아보자.

- X-Forwarded Series: 요청이 어디서 건너왔는지 알려주는 헤더이다. 보통 클라이언트와 서버간 2단 구조로 보다는 클라 - 중개 - 중개 - 서버 같은 다단 구조로 이루어져있기 때문에, 원래 요청이 누구였는지 밝히기 위해 사용한다.
    
    - X-Forwarded-For: `1.1.1.1`, `2.2.2.2`, `3.3.3.3` 이와같이 현재까지 거쳐온 서버의 IP 정보를 가지고있다.
        
    - X-Forwarded-Host: `www.google.com` 원래서버의 호스트명이다.
        
    - X-Forwarded-Proto: `https` 원래 서버의 프로토콜 명
        
    - X-Frame-Options: frame, iframe, object 태그 안에서 페이지를 렌더링 하는 것을 막을 수 있다.
        

옵션 중 한가지를 설정할 수 있는데 다음과같다.

- X-Frame-Options: `DENY`, : frame, ifrome, object를 안쓸 시  
    `SAMEORIGIN`, : 개인 사이트 자체를 iframe 등으로 불러오는 경우  
    `ALLOW-FROM https//www.google.com` : 특정 사이트만 불러오는 것을 허용할 때
    
- X-Content-Type-Options: 서버에서 보내온 Content-Type 헤더가 잘못 설정되었다고 생각하는 경우, 브라우저는 자체적으로 컨텐츠 타입을 추론한다.

예를 들어 css파일인데 text/html로 전달 받은 경우 브라우저가 text/css로 추론이 가능하다는 뜻이다. 이러한 방법보다는 차라리 `nosniff`를 보내주어, 브라우저가 서버가 보낸 컨텐츠 타입을 따르게 강제할 수 있다.

`X-Content-Type-Options: nosniff`

### X가 붙은 헤더도 아래 그림에 나온다.

## ==**Request Message**==

HTTP Request Message는 공백(blank line)을 제외하고 3가지 부분으로 나누어진다.

HTTP Request Message 구조

- **Start Line**
- **Headers**
- **Body**
![사진](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FceqzRL%2FbtrFHEJFUZb%2FMeb7vQf0y3oeB0ZU7xV8eK%2Fimg.png)

## start line

HTTP Request Message의 시작 라인

HTTP request의 start line 3가지 부분으로 구성

- HTTP method
- Request target
- HTTP version
```http
GET /test.html HTTP/1.1
[HTTP Method] [Request target] [HTTP version]
```
- `HTTP method`는 요청의 의도를 담고 있는 GET, POST, PUT, DELETE 등이 있습니다. GET은 존재하는 자원에 대한 요청, POST는 새로운 자원을 생성, PUT은 존재하는 자원에 대한 변경, DELETE는 존재하는 자원에 대한 삭제와 같은 기능을 가지고 있습니다.
- `Request target`은 HTTP Request가 전송되는 목표 주소입니다.
- `HTTP version`은 version에 따라 Request 메시지 구조나 데이터가 다를 수 있어서 version을 명시합니다.
## headers
해당 request에 대한 추가 정보(addtional information)를 담고 있는 부분  
예를 들어, request 메세지 body의 총 길이 (Content-Length) 등 Key:Value 형태로 구성

headers도 크게 3가지 부분으로 나뉨(general headers, request headers, [[entity headers]])
![사진](https://velog.velcdn.com/images%2Fpixelstudio%2Fpost%2F362841e4-ace5-466e-acb1-f22b440cfda4%2FScreenshot%20from%202021-08-31%2005-56-43.png)

- HOST: 서버의 도메인 네임이 나타는 부분이다.

`www.tistory.com`

- User-Agent: 현재 사용자가 어떤 운영체제와 브라우저를 이용해 요청을 보냈는지 나온다.

```js
Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/53
```

## 요청헤더(Request Header)

- HOST: 서버의 도메인 네임이 나타는 부분이다.

`www.tistory.com`

- User-Agent: 현재 사용자가 어떤 운영체제와 브라우저를 이용해 요청을 보냈는지 나온다.

```js
Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36
```

- Accept: 서버에 요청을 보낼 때 해당 타입(MIME)의 데이터를 보내줬으면 좋겠다고 명시할때 사용한다.

```null
html 형식만 요청 시 Accept: text/html
텍스스트 와일드카드 Accept: text/*
이미지 특정 형식   Accept: image/png, image/gif
```

- Authorization: 이 헤더는 인증 토큰(Basic나 Bearer 토큰)을 서버로 보낼 때 사용하는 헤더이다. 보통 Basic나 Bearer 같은 토큰의 타입과 사용자 에이전트임을 증명하는 자격을 실어서 보낸다.

```js
Authorization: <type> <credentials>

Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJ2ZWxvcGVydC5jb20iLCJleHAiOiIxNDg1MjcwMDAwMDAwIiwiaHR0cHM6Ly92ZWxvcGVydC5jb20vand0X2NsYWltcy9pc19hZG1pbiI6dHJ1ZSwidXNlcklkIjoiMTEwMjgzNzM3MjcxMDIiLCJ1c2VybmFtZSI6InZlbG9wZXJ0In0.WE5fMufM0NDSVGJ8cAolXGkyB5RmYwCto1pQwDIqo2w
```

**Basic(RFC 7617)**: 사용자 아이디와 암호를 Base64로 인코딩한 값을 토큰으로 사용한다.

```js
A. 사용자명과 비밀번호가 콜론을 이용하여 합쳐진다 ex) kimcoding:qlalfqjsgh12

B. 결과에 대한 문자열을 `Base64`로 인코딩한다 (a2ltY29kaW5nOnFsYWxmcWpzZ2gxMg==)
                                
C. 결과  Authorization: Basic a2ltY29kaW5nOnFsYWxmcWpzZ2gxMg==                         
```

**Bearer(RFC 6750)**: OAuth나 JWT에 대한 접근을 위해 발급받는 자격증명 타입이다.

정보 교류나 회원인증을 위주로 사용하며, `헤더.내용.서명` 을 기본적으로 HMAC SHA256 알고리즘을 사용하는데, base64UrlEncode(header), base64UrlEncode(payload)과 비밀키를 인코딩 한 후 검증에 활용하기 위해 쓰인다.

**AWS4-HMAC-SHA256**: AWS 전자 서명 기반 인증 타입이다.

```js
Authorization: AWS4-HMAC-SHA256 
Credential=AKIAIOSFODNN7EXAMPLE/20130524/us-east-1/s3/aws4_request, 
SignedHeaders=host;range;x-amz-date,
Signature=fe5f80f77d5fa3beca038a248ff027d0445342fe2855ddc963176630326f1024
```

- Origin: POST 같은 요청을 보낼 때, 요청이 어느 주소에서 시작되었는지 나타낸다. 요청 주소와 받는 주소가 다를 경우 CORS 문제가 발생한다.
    
- Cookie: 클라이언트가 서버한테 쿠키를 보낼 때, 헤더에 담아 보낸다.
    

```js
cookie: SEARCH_SAMESITE=CgQIsZMB; ab.storage.userId.7af503ae-0c84-478f-98b0-ecfff5d67750=%7B%22g%22%3A%22browser-1629963143862-c%22%2C%22c%22%3A1629963277478%2C%22l%22%3A1629963277478%7D; ab.storage.deviceId.7af503ae-0c84-478f-98b0-ecfff5d67750=%7B%22g%22%3A%22217b5680-be7a-a876-6177-b1fc3231bdda%22%2C%22c%22%3A1629963277484%2C%22l%22%3A1629963277484%7D; OTZ=6127857_20_20__20_; ....
```

보는 바와 같이  
`Cookie: <key> = <Value>, <Key> = <Value>`

이런식으로 이루어져 있다.

- Referer: 해당 페이지 이전의 주소가 담겨있다. 이 헤더를 사용할 경우 어떤 페이지에서 현재 페이지로 넘어왔는지 알 수 있다.
    
- Refrrer-Policy: 크롬에서는 General Header에 속해있다. Referer는 방문시 흔적을 남기기 때문에(A에서 B에게 방문정보 전달) 전달되는 주소의 노출 정도를 정책을 정할 수 있다.
    

```html
<meta name"referrer" content="이곳에 작성" />
```

`이곳에 작성`에 들어가야할 필드

- no-referrer: 요청해도 referrer을 전송하지 않음.
- no-referrer-when-downgrade: 대상 주소가 `https` 일 때만 전송한다.
- origin-when-cross-origin: 같은 홈페이지 일때는 전체주소, 다른 페이지로는 도메인 주소로 보낸다.
- strict-origin-when-cross-origin: 같은 홈페이지 일때는 전체 주소, 다른 https 페이지로 갈때는 도메인 주소만, http로 갈때는 referrer 전송하지 않음

## body

HTTP Request가 전송하는 데이터를 담고 있는 부분

전송하는 데이터가 없다면 body 부분은 비어있습니다.

보통 post 요청일 경우, HTML 폼 데이터가 포함되어 있습니다.

```http
POST /test HTTP/1.1

Accept: application/json
Accept-Encoding: gzip, deflate
Connection: keep-alive
Content-Length: 83
Content-Type: application/json
Host: google.com
User-Agent: HTTPie/0.9.3

{
    "test_id": "tmp_1234567",
    "order_id": "8237352"
}
```
---

## ==Response Message==

HTTP Response Message는 request와 동일하게 공백(blank line)을 제외하고 3가지 부분으로 나누어진다.
- **Status Line**
- **Headers**
- **Body**
![사진](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbhue8H%2FbtrFFB1OcZf%2F6PMLEI7rbXyaUQny5CRizk%2Fimg.png)

### Status line
HTTP Response의 상태를 간략하게 나타내주는 부분

HTTP Response의 status line또한 3가지 부분으로 구성

- HTTP version
- Status Code
- Status Text

```http
HTTP/1.1 200 OK
[HTTP version] [Status Code] [Status Text]
```

## headers

Request의 headers와 동일하다.
![사진](https://velog.velcdn.com/images%2Fpixelstudio%2Fpost%2F362841e4-ace5-466e-acb1-f22b440cfda4%2FScreenshot%20from%202021-08-31%2005-56-43.png)
- Access-Control-Allow-Origin: 요청을 보내는 Client 주소와 받는 Server 주소가 다른 경우 CORS 에러를 발생한다.

특정 도메인 `Access-Control-Allow-Origin::www.example.net` 으로 특정 도메인을 작성하여 해결하거나 `Access-Control-Allow-Origin:*`으로 어느 곳에서든지 요청을 보낼수 있게 할 수 있다.

- Allow:
    
- Location : 300 혹은 201 Created 응답일 때 어느 페이지로 이동할지 알려주는 헤더
    

```js
Location: https://www.tistory.com/
```

- Pragma : HTTP/1.1의 Cache-Control 헤더가 생기기전 대용 헤더로 사용되었다. HTTP/1.0을 사용하는 클라이언트들 만을 위한 비공식적인 호환성을 위해 사용한다.

```js
pragma: no-cache // Cache-control: no-cache와 같은 기능
```

- Set-Cookie: 서버에서 클라이언트(브라우저)에게 해당 쿠키를 저장하라는 명령하는 헤더이다.  
    `Set-Cookie: <key>=<Value>; Options....`

server측에서 cookiePaser()라이브러리를 활용 한다면, `res.cookie(key, value, option)`  
이런식으로 활용한다.

Cookie 옵션을 확인해보자  
`Expires:` 쿠키 만료 날짜를 알려준다.  
`Max-Age:` 쿠키 수명을 알려 준다.(Expires는 무시됨)  
`Secure:` Https에서만 쿠기가 전송된다.  
`HttpOnly:` 자바스크립트에서 쿠키에 접근할 수 없다. XSS 공격을 막기 위한 수단.  
`Domain:` 일치한 도메인에서 요청시 쿠키가 전송된다.  
`Path:` 경로가 일치하는 요청에서만 쿠키가 전송된다.
## body

Response의 body와 일반적으로 동일하다.

```http
POST /test HTTP/1.1

Accept: application/json
Accept-Encoding: gzip, deflate
Connection: keep-alive
Content-Length: 83
Content-Type: application/json
Host: google.com
User-Agent: HTTPie/0.9.3

{
    "test_id": "tmp_1234567",
    "order_id": "8237352"
}
```
Request와 마찬가지로 모든 response가 body가 있지는 않다.

데이터를 전송할 필요가 없을경우 body가 비어있게 된다.


출처: [https://hahahoho5915.tistory.com/62](https://hahahoho5915.tistory.com/62) [넌 잘하고 있어:티스토리]
[출처](https://velog.io/@pixelstudio/%ED%81%AC%EB%A1%AC-%EA%B0%9C%EB%B0%9C%EC%9E%90-%EB%8F%84%EA%B5%AC%EB%A1%9C-%EB%B3%B4%EB%8A%94-HTTP-%ED%97%A4%EB%8D%94-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)