### 엔티티 헤더(Entity Header)

- Content-Length: 요청과 응답 메세지의 본문 크기를 바이트 단위로 표시해준다.
- Content-Type: 컨텐츠 타입과 문자열 인코딩(utf-8 등)을 명시할 수 있다.

```js
text/html;charset=UTF-8
```

- Content-Language: 사용자의 언어를 뜻한다,

```js
Content-language: en-Us
```

- Content-encoding: 컨텐츠의 압축된 방식을 담는 헤더이다. br, gzip, deflate 등이 있으며 브라우저가 해제해서 사용한다. 데이터 소모량도 줄어들고, 요청, 응답에 대해서 전송속도도 빨라지기 때문에 압축을 권장함