---
tags: network, http, Internet
---
**HTTP는 HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는** [[Protocol]]입니다. HTTP는 웹에서 이루어지는 모든 데이터 교환의 기초이며, 클라이언트-서버 프로토콜이기도 합니다. 클라이언트-서버 프로토콜이란 (보통 웹브라우저인) 수신자 측에 의해 요청이 초기화되는 프로토콜을 의미합니다.
![사진](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview/fetching_a_page.png)
클라이언트와 서버들은 (데이터 스트림과 대조적으로) 개별적인 메시지 교환에 의해 통신합니다. 보통 브라우저인 클라이언트에 의해 전송되는 메시지를 요청(_requests_)이라고 부르며, 그에 대해 서버에서 응답으로 전송되는 메시지를 응답(_responses_)이라고 부릅니다.
![사진](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview/http-layers.png)
HTTP는 애플리케이션 계층의 프로토콜로, 신뢰 가능한 전송 프로토콜이라면 이론상으로는 무엇이든 사용할 수 있으나 [[TCP]] 혹은 암호화된 TCP 연결인 [TLS](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%EB%A1%9C%EB%93%9C%EB%A7%B5%2F1.%20Internet%2FHow%20does%20the%20internet%20work%2FSSL%2CTLS)를 통해 전송됩니다.
HTTP의 확장성 덕분에, 오늘날 하이퍼텍스트 문서 뿐만 아니라 이미지와 비디오 혹은 HTML 폼 결과와 같은 내용을 서버로 포스트(POST)하기 위해서도 사용됩니다. HTTP는 또한 필요할 때마다 웹 페이지를 갱신하기 위해 문서의 일부를 가져오는데 사용될 수도 있습니다.
## HTTP 기반 시스템의 구성요소
==HTTP는 클라이언트-서버 프로토콜입니다.==
요청은 하나의 개체, 사용자 에이전트(또는 그것을 대신하는 프록시)에 의해 전송됩니다.
사용자 에이전트는 브라우저지만, 무엇이든 될 수 있습니다.

[출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)