---
tags:
  - Internet
---

높은 단계에서 인터넷은 표준화된 프로토콜의 세트를 사용해서 장치들과 컴퓨터들이 함께 연결되는 것이다. 이러한 [프로토콜](Protocol.md)들은 얼마나 정보들이 장치들 사이에서 교환되는지 그리고 그 데이터들이 얼마나 확실하고 안전하게 보장되어지는 지를 정의한다.

인터넷의 핵심은 상호연결된 [라우터](Router.md)들의 글로벌 네트워크이다. 라우터는 서로 다른 장치와 시스템 간의 [트래픽](Traffic.md) 전달을 담당한다. 너가 인터넷으로 데이터를 보낼때, 데이터는 작은 [패킷](Packet.md)들로 나눠지고 너의 작은 장치에서 라우터로 보내진다. 그 라우터는 패킷을 검사하여 목적지를 향한 경로의 다음 라우터에 패킷을 전달하고, 이 과정은 패킷이 원하는 목적지에 도달할 때까지 계속된다.

패킷이 올바르게 전송되고 수신되는지 확인하려면, 인터넷은 다양한 프로토콜들을 사용하는데, [the Internet Protocol (IP)](IP.md)_그리고 [Transmission Control Protocol (TCP)](TCP.md). IP는 패킷을 올바른 대상으로 [라우팅](Routing.md)하는 역할을 담당하고, TCP는 패킷이 올바른 순서로 안정적으로 전송되도록 보장한다.

추가적으로 이러한 핵심 프로토콜, 소통과 데이터 교환을 인터넷에서 가능하게 하기위해 사용되는 넓은 범위의 다른 기술들과 프로토콜들이 있다, Domain Name System [(DNS)](DNS.md), the Hypertext Transfer Protocol [(HTTP)](HTTP.md), 그리고 the Secure Sockets Layer/Transport Layer Security [(SSL/TLS)](SSL,TLS.md) protocol. 

>[!important] 
>이러한 다양한 기술과 프로토콜이 어떻게 함께 작동하여 인터넷을 통한 통신 및 데이터 교환을 가지능하게 하는지에 대한 확실한 이해를 가지는 것은 개발자로서 중요한 임무이다.

## Basic Concepts and Terminology

- **[[Packet]]:** 인터넷을 통해 전송되는 작은 데이터 단위
- **[[Router]]:** 서로 다른 네트워크 간에 데이터 패킷을 전달하는 장치
- **[IP Address](IP):** 네트워크의 각 장치에 할당된 고유 식별자로, 데이터를 올바른 대상으로 라우팅하는 데 사용
- **[[Domain Name]]:** google.com과 같이 웹사이트를 식별하는 데 사용되는 사람이 읽을 수 있는 이름
- **[[DNS]]:** 도메인 이름 시스템은 도메인 이름을 IP 주소로 변환하는 역할을 담당
- **[[HTTP]]:** The Hypertext Transfer Protocol은 클라이언트(예: 웹 브라우저)와 서버(예: 웹 사이트) 간에 데이터를 전송하는 데 사용
- **[[HTTPS]]:** 클라이언트와 서버 간의 보안 통신을 제공하는 데 사용되는 암호화된 HTTP 버전입니다.
- **[SSL/TLS](SSL,TLS.md):** SSL(Secure Sockets Layer) 및 전송 계층 보안(Transport Layer Security) 프로토콜은 인터넷을 통한 보안 통신을 제공하는 데 사용

# The Role of Protocols in Internet
프로토콜은 인터넷을 통한 통신 및 데이터 교환을 가능하게 하는데 중효한 역할을 합니다.
프로토콜은 장치와 시스템 간에 정보가 교환되는 방식을 정의하는 일련의 규칙 및 표준입니다.

인터넷 프로토콜([[IP]]), 전송 제어 프로토콜([[TCP]]), 사용자 데이터그램 프로토콜([[UDP]]), 도메인 이름 시스템([[DNS]])등을 포함하여 인터넷 통신에 사용되는 다양한 프로토콜이 있다.

IP는 데이터 패킷을 올바른 대상으로 라우팅하는 역할을 하며, TCP와 UDP는 패킷이 안정적이고 효율적으로 전송되도록 보장한다. DNS는 도메인 이름을 IP주소로 변환하는데 사용되고, HTTP는 클라이언트와 서버 간에 데이터를 전송하는데 사용됩니다.

표준화된 프로토콜을 사용하는 주요 이점 중 하나는 다양한 제조업체 및 공급업체의 장치와 시스템이 서로 원활하게 통신할 수 있기 때문입니다. 예를 들어, 한 회사에서 개발한 웹 브라우저는 둘 다 HTTP 프로토콜을 준수하는 한 다른 회사에서 개발한 웹 서버와 통신 할 수 있습니다.

개발자로서 인터넷 통신에서 사용되는 다양한 프로토콜과 이러한 프로토콜이 함께 작동하여 인터넷을 통해 데이터와 정보를 전송할 수 있는 방법을 이해하는 것이 중요합니다.

## IP 주소 및 도메인 이름 이해
IP 주소와 도메인 이름은 모두 인터넷 작업 시 이해해야 할 중요한 개념입니다.

IP주소는 네트워크의 각 장치에 할당된 고유 식별자 입니다. 이는 데이터를 올바른 대상으로 라우팅하여 정보가 의도한 수신자에게 전송되도록 하는 데 사용됩니다. IP주소는 일반적으로 "192.168.1.1"과 같이 마침표로 구분된 일련의 4개 숫자로 표시됩니다. 

반면, 도메인 이름은 웹사이트와 기타 인터넷 리소스를 식별하는 데 사용되는 사람이 읽을 수 있는 이름입니다. 일반적으로 두 개 이상의 부분으로 구성되며 마침표로 구분됩니다. 예를 들어 'google.com'은 도메인 이름입니다. 도메인 이름은 DNS(Domain Name System)를 사용하여 IP 주소로 변환됩니다.

[출처](https://cs.fyi/guide/how-does-internet-work)
##### 영단어
>[!note] standardized
>At a high level, the internet works by connecting devices and computer systems together using a set of ==standardized== protocols.

>[!note] transmit, reliably
>These protocols define how information is exchanged between devices and ensure that data is ==transmitted== ==reliably== and securely.

>[!note] interconnected, directing, responsible
The core of the internet is a global network of ==interconnected== routers, which are ==responsible== for ==directing== traffic between different devices and systems.

>[!note] examine, forward
>The router ==examines== the packet and ==forwards== it to the next router in the path towards its destination.

>[!note] ensure
>IP is responsible for routing packets to their ==correct== destination, while TCP ensures that packets are transmitted reliably and in the correct order.

>[!note] Terminology
>Basic Concepts and ==Terminology==