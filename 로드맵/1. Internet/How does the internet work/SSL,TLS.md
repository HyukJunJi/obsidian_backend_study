---
tags: network, security, Internet
---
### **SSL**

원래 웹에서의 데이터는 가로채면 누구나 읽을 수 있는 일반 텍스트 형태로 전송 되었다. 이러한 문제때문에 인터넷 통신의 개인정보 보호, 인증, 데이터 무결성을 보장하기 위해 Netscape가 1995년 처음으로 SSL을 개발하였다.

SSL(Secure Scokets Layer)은 암호화 기반 인터넷 보안 프로토콜이다. 전달되는 모든 데이터를 암호화하고 특정한 유형의 사이버 공격도 차단한다. SSL은 TLS(Transport Layer Security) 암호화의 전신이기도 한다.

SSl/TLS 를 사용하는 웹사이트 URL은 HTTP 대신 HTTPS가 사용된다.
![사진](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdLpa9j%2Fbtrnqx2LpIV%2FhKTqMkQDLtQdOiv39j7KPk%2Fimg.png)
SSL은 1996년 SSL 3.0 이후 업데이트되지 않았으며, 앞으로 사라지게 될 것으로 여기지고 있다. 또한 알려진 취약성이 여러가지 있으며 보안 전문가들은 SSL 사용 중단을 권장한다고 한다. 그 대안으로는 앞에서 언급한 TLS가 있다. TLS는 최신 암호화 프로토콜로, SSL 암호화로 혼용해서 부르는 경우도 많다. 실제로 현재 SSL을 인증한 업체 및 제공하는 업체는 사실상 TLS 암호화를 제공하고 있는것이다.

### TLS
TLS는 SSL의 업데이트 버전으로 SSL의 최종버전인 3.0과 TLS의 최초버전의 차이는 크지 않으며, 이름이 바뀐것은 SSL을 개발한 Netscape가 업데이트에 참여하지 않게되어 소유권 변경을 위해서였다고 한다.
결과적으로 TLS는 SSL의 업데이트 버전이며 명칭만 다르다고 볼 수 있다.

#### SSL/TLS의 작동 방식

SSL은 개인정보 보호를 제공하기 위해, 웹에서 전송되는 **데이터를 암호화** 한다. 따라서, 데이터를 가로채려해도 거의 **복호화가 불가능**하다.

SSL은 클라이언트와 서버간에 **핸드셰이크를 통해 인증**이 이루어진다. 또한 **데이터 무결성**을 위해 데이터에 디지털 서명을 하여 데이터가 의도적으로 도착하기 전에 조작된 여부를 확인한다.

### **SSL/TLS 핸드셰이크**

핸드셰이크는 클라이언트와 서버간의 메세지 교환이며,  HTTPS 웹에 처음 커넥션할 때 진행된다. 

핸드셰이크의 단계는 클라이언트와 서버에서 지원하는 암호화 알고리즘, 키 교환 알고리즘에 따라 달라진다.

일반적으로는 **RSA 키 교환 알고리즘**이 사용된다.

##### RSA 키 교환 알고리즘 순서

1. 클라이언트 -> 서버 메세지 전송 - 이때 핸드셰이크가 시작된다. 이 메세지에는 TLS 버전, 암호화 알고리즘, 무작위 바이트 문자열이 포함된다.
2. 서버 -> 클라이언트 메세지 전송 - 클라이언트의 메세지에 응답으로 서버의 SSL인증서, 선택한 암호화 알고리즘, 서버에서 생성한 무작위 바이트 문자열을 포함한 메세지를 전송한다.
3. 인증 - 클라이언트가 서버의 SSL인증서를 인증 발행 기관에 검증한다. 
4.  예비 마스터 암호 - 클라이언트는 무작위 바이트 문자열을 공개 키로 암호화된 premater secret 키를 서버로 전송한다.
5. 개인 키 사용 - 서버가 premaster secret 키를 개인 키를 통해 복호화한다. (개인 키로만 복호화 가능)
6. 세션 키 생성 - 클라이언트와 서버는 클라이언트가 생성한 무작위 키, 서버가 생성한 무작위 키, premaster secret 키를 통해 세션 키를 생성한다. 양쪽은 같은 키가 생성되어야 한다.
7. 클라이언트 완료 전송 - 클라이언트는 세션 키로 암호화된 완료 메세지를 전송한다.
8. 서버 완료 전송 - 서버도 세션 키로 암호화된 완료 메세지를 전송한다.
9. 핸드셰이크 완료 - 핸드셰이크가 완료되고, 세션 키를 이용해 통신을 진행한다.

### SSL/TLS 차이

SSL (Secure Sockets Layer)과 TLS (Transport Layer Security)은 보안 통신 프로토콜로서, 데이터의 기밀성과 무결성을 보호하며, 서버와 클라이언트 간의 안전한 통신을 제공합니다. TLS는 사실상 SSL의 후속 버전으로 개발되었으며, 이 둘 간의 주요 차이점은 다음과 같습니다.

1. 이름과 버전:
    
    - SSL는 초기에 넷스케이프에서 개발되었으며 SSL 2.0 및 SSL 3.0 버전이 있습니다.
    - TLS는 SSL 3.0의 개선 및 보안 결함 수정을 포함하여 개발되었습니다. TLS는 TLS 1.0부터 시작하여 현재까지 TLS 1.3까지 다양한 버전이 존재합니다.
2. 보안 및 암호화:
    
    - TLS는 이전의 SSL 버전에서 발견된 여러 보안 결함을 수정하고, 더 강력한 보안 및 암호화 알고리즘을 도입했습니다. 따라서 TLS는 더 안전한 통신을 제공합니다.
    - SSL 3.0과 이전 버전의 SSL에는 일부 약점이 있어 중요한 보안 문제를 일으킬 수 있으므로 사용을 권장하지 않습니다.
3. 호환성:
    
    - TLS는 SSL과 거의 호환되지만 약간의 프로토콜 차이가 있을 수 있습니다. 예를 들어, SSL 2.0와 TLS는 호환성 문제가 있을 수 있습니다.
    - TLS는 SSL을 대체하는 것으로 간주되므로 보안을 강화하고 최신 보안 표준을 준수하기 위해 TLS를 사용하는 것이 권장됩니다.
4. 기능 및 프로토콜 업데이트:
    
    - TLS는 SSL의 일부 기능을 유지하면서 더 많은 보안 옵션을 제공합니다. 예를 들어, TLS 1.2 및 1.3은 Perfect Forward Secrecy (PFS)를 지원하고, 더 강력한 암호화 알고리즘을 사용합니다.

### SSL/TLS를 사용하여 인터넷 통신을 보호할 때 이해해야 할 몇 가지 주요 개념이 있습니다.

- **인증서:** SSL/TLS 인증서는 클라이언트와 서버 간의 신뢰를 구축하는 데 사용됩니다. 여기에는 서버의 신원에 대한 정보가 포함되어 있으며 신뢰성을 확인하기 위해 신뢰할 수 있는 제3자(인증 기관)의 서명을 받았습니다.
    
- **핸드셰이크:** SSL/TLS 핸드셰이크 프로세스 중에 클라이언트와 서버는 보안 연결을 위한 암호화 알고리즘 및 기타 매개변수를 협상하기 위해 정보를 교환합니다.
    
- **암호화:** 보안 연결이 설정되면 합의된 알고리즘을 사용하여 데이터가 암호화되며 클라이언트와 서버 간에 안전하게 전송될 수 있습니다.
    

인터넷 기반 애플리케이션 및 서비스를 구축할 때 SSL/TLS의 작동 방식을 이해하고 로그인 자격 증명, 결제 정보 및 기타 개인 데이터와 같은 민감한 데이터를 전송할 때 애플리케이션이 SSL/TLS를 사용하도록 설계되었는지 확인하는 것이 중요합니다. 또한 서버에 대해 유효한 SSL/TLS 인증서를 획득 및 유지하고 SSL/TLS 연결 구성 및 보안을 위한 모범 사례를 따르고 있는지 확인해야 합니다. 이렇게 하면 사용자 데이터를 보호하고 인터넷을 통한 애플리케이션 통신의 무결성과 기밀성을 보장할 수 있습니다.

요약하면, TLS는 SSL의 보안 결함을 해결하고, 더 강력한 보안 및 암호화를 제공하는 프로토콜로서 SSL보다 더 안전하며, 현대적인 웹 보안 표준으로 인식됩니다. 따라서 TLS를 사용하는 것이 권장되며, SSL보다는 사용을 피하는 것이 좋습니다.
[출처](https://kanoos-stu.tistory.com/46)

[spring에 tls 적용 나중에 해보기](https://www.baeldung.com/spring-tls-setup)