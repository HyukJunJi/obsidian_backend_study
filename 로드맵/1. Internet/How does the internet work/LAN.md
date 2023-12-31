---
tags: Internet, network
---
근거리 통신망이며 일반적으로 같은 건물에 있는 작은 지리적 영역 내에 포함된 네트워크이다.
가정용 Wi-Fi 네트워크와 소규모 비즈니스 네트워크는 LAN의 일반적인 예이다.

![LAN예시 사진](https://www.cloudflare.com/resources/images/slt3lc6tev37/78rJr5URxwDD9uyxKNpsiJ/d220f31e4b59c89290f04eed689ab5bb/what_is_LAN_diagram.png)
## LAN은 어떻게 작동할까?

대부분은 LAN은 [라우터](Router)라는 중앙 지점에서 [인터넷](obsidian://open?vault=%EB%A1%9C%EB%93%9C%EB%A7%B5%20%EA%B3%B5%EB%B6%80&file=%EB%A1%9C%EB%93%9C%EB%A7%B5%2F1.%20Internet%2F1.%20Internet)에 연결됩니다. 홈 LAN은 종종 단일 라우터를 사용하는 반면, 더 큰 공간의 LAN은 보다 효율적인 패킷 전달을 위해 [[네트워크 스위치]]를 추가로 사용할 수 있습니다. 

LAN은 네트워크 내의 장치를 연결하기 위해 거의 항상 [[이더넷]]이나 WiFi를 사용하거나 둘 모두를 사용합니다. 이더넷은 이더넷 케이블을 사용해야 하는 물리적 네트워크 연결용 프로토콜입니다. WiFi는 전파를 통해 네트워크에 연결하기 위한 프로토콜입니다.

서버, 데스크톱 컴퓨터, 랩톱, 프린터, IoT장치, 게임 콘솔 등 다양한 장치를 LAN에 연결할 수 있습니다. 사무실에서, LAN은 내부 직원이 연결된 프린터 또는 서버에 대하여 공유 액세스를 이용하는데 자주 사용됩니다.

## 가상 LAN이란?

가상 LAN(VLAN)은 동일한 물리적 네트워크의 트래픽을 두 개의 네트워크로 분할 하는 방법입니다. 같은 방에 각각 자체 라우터와 인터넷 연결이 있는 두 개의 별도 LAN을 설정한다고 상상해 보세요. VLAN도 이와 비슷하지만, 물리적으로 하드웨어를 사용하는 대신 소프트웨어를 사용하여 가상으로 분할되므로 하나의 인터넷 연결이 있는 하나의 라우터만 필요합니다.

VLAN은 네트워크 관리, 특히 아주 대규모의 LAN에 도움이 됩니다. 관리자는 네트워크를 세분화함으로써 네트워크를 훨씬 더 쉽게 관리할 수 있습니다. (VLAN은 [서브넷](https://www.cloudflare.com/learning/network-layer/what-is-a-subnet/)과는 아주 다릅니다. 서브넷은 효율을 높이려고 네트워크를 세분화하는 또 다른 방법입니다.)

## LAN은 인터넷의 나머지 부분과 어떤 관련이 있을까요?

인터넷은 네트워크의 네트워크입니다. LAN은 일반적으로 훨씬 더 큰 네트워크인 [자율 시스템(AS)](https://www.cloudflare.com/learning/network-layer/what-is-an-autonomous-system/)에 연결됩니다. AS는 자체 [라우팅](Routing) 정책을 갖추고 특정 IP 주소를 제어하는 아주 규모가 큰 네트워크입니다. 인터넷 서비스 공급자(ISP)는 AS의 한 예입니다.

LAN을 이렇게 상상해보세요. 작은 네트워크가 훨씬 더 큰 네트워크에 연결되고, 그 네트워크는 다시 LAN을 포함하는 다른 아주 큰 네트워크에 연결되어 구성되는 네트워크라고요. 이것은 인터넷이며, 수천 마일 떨어진 두 개의 서로 다른 LAN에 연결된 두 대의 컴퓨터가 네트워크 간에 이러한 연결을 통해 데이터를 전송하여 서로 통신할 수 있습니다.
[출처](https://www.cloudflare.com/ko-kr/learning/network-layer/what-is-a-lan/)