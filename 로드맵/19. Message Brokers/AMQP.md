![사진](https://velog.velcdn.com/images/holicme7/post/129dddac-1eab-4f60-91de-6913ba3b0402/image.png)
**AMQP (Advanced Message Queue Protocol) 다른것으로 MQTT가 있다**

AMQP는 메세지 지향 [미들웨어 (MOM)](미들웨어.md) 을 위한 표준 응용 계층 프로토콜입니다. 간단히 말하면, **메세지 통신을 위한 규약 스펙** 입니다.

### 구현체

- RabbitMQ
- OpenAMQ
- StormMQ
- Apache Qpid

### 특징

AMQP의 주요 기능으로는 메세지 기반, Queuing, 라우팅, 신뢰성 및 보안 등으로 정의할 수 있습니다.

과거의 미들웨어 표준들은 API 레벨(ex: JMS)에서 등장하였습니다. 이러한 방식은 여러 구현체 간 상호 운용성을 제공하기 보다는, 다른 미들웨어 구현체들과 프로그래머의 상호작용을 표준화에 중점을 두었습니다.

API 및 메세지 구현체들이 제공해야 하는 일련의 행동 양식을 정의하는 JMS와 다르게 AMQP는 와이어 레벨 프로토콜입니다. 와이어 레벨 프로토콜은 네트워크를 통해 바이트 스트림으로 전송되는 데이터 형식에 대한 설명입니다.

결과적으로 이 데이터 형식을 준수하는 메세지를 만들고 해석할 수 있는 모든 도구는 구현 언어에 관계없이 다른 호환 도구와 상호 운용이 가능합니다.

### 단점

- 성능 오버헤드
    - 유연성과 안정성을 보장하지만, 매우 낮은 대기시간이 필요하거나 매우 높은 메세지를 처리할때는 약간의 성능 오버헤드가 있음
    - 풍부한 기능 집합과 정교한 라우팅 메커니즘은 오버헤드를 증가시키면서 고급 기능을 제공합니다.
    - 사용 사례에 따라 MQTT와 같은 다른 메세징 프로토콜이나 특정 사용 사례에 최적화된 프로토콜이 더 나은 성능을 제공할 수 있음
- MQTT 보다 더 높은 대역폭을 필요로 함
- 버전 호환성 문제
    - 이전 버전과 호환되지 않음 (버전 문제를 고려해야 함)
- 상호 운용성 문제
    - AMQP는 서로 다른 시스템 간의 상호 운용성을 제공하는 것을 목표로 하지만 일부 메시징 브로커 또는 클라이언트는 AMQP 사양을 완전히 준수하지 않아 호환성 문제가 발생할 수 있습니다.
    - **AMQP와 호환 가능한 프로토콜들**
        - SOTMP
        - XMPP
        - MQPP
        - OpenWire

AMQP는 메세지 큐 구조에 `Exchage` 라는 라우터 역할이 존재합니다. `Exchange`는 `Publisher`에게 메세지를 수신 받으면 그것을 `Queue`에 분배해주는 역할을 합니다(큐에 직접 메세지를 전달하지 않음). 이렇게 `Queue`에 저장된 메세지를 `Consumer`가 소비하게 됩니다.

한개의 `Queue`만 있다면 일반적인 메세지 큐와 다를 것 없지만, 여러 개의 `Consumer`와 `Queue`가 존재할 때 높은 효율을 보여줍니다.

- Exchange : Publisher(Producer)로 부터 수신한 메세지를 큐에 분배하는 라우터 역할
- Queue : 메세지를 메모리나 디스크에 저장했다가 Consumer에게 메세지를 전달하는 역할. 스스로가 관심있는 메세지 타입을 지정한 Binding을 통해 Exchange에 말 그대로 Bind 된다.
- Binding : Exchange에 전달된 메세지가 어떤 Queue에 저장되어야 하는지 정의

![사진](https://velog.velcdn.com/images/holicme7/post/a8c941b2-24fc-4003-84ba-60a4340e41a3/image.png)
**Exchange의 종류**

- Direct Exchange
    - 메세지의 라우팅 키를 큐에 1:1로 매칭 시키는 방법
    - Direct Exchange는 Exchange로 수신된 메시지의 routing key와 Queue의 Binding key가 정확히 매칭되는 Queue로 전달됩니다.
- Fanout Exchange
    - 모든 메세지를 모든 큐로 라우팅하는 유형
    - Fanout Exchange는 Exchange와 매칭된(바인딩 된) 모든 Queue에 메시지를 전달합니다. 이때 라우팅 키는 무시가 됩니다.
    - 1:N 관계로 메시지를 브로드캐스트하는 용도로 사용합니다.
- Topic Exchange
    - Direct Exchange와 유사한 방식이지만 고정된 routing key를 사용하는 것 대신 와일드카드(Wildcard)를 사용합니다. 메시지는 하나 또는 여러개의 routing pattern이 맞는 Queue에 보내집니다.
    - 라우팅 키는 ","으로 구분된 0개 이상의 단어의 집합으로 간주 되고 와일드카드 문자들을 이용해 특정 큐에 바인딩합니다.
    - '*' : 하나의 단어
    - '#' : 0개 이상의 단어
- Headers Exchange
    - 마지막으로 Headers Exchange는 routing key 대신에 헤더 속성을 통해 라우팅됩니다.
    - key-value로 정의된 헤더에 의해 라우팅을 결정합니다.
    - 큐를 바인딩 할 때 x-match라는 특별한 argument로 헤더를 어떤식으로 해석하고 바인딩 할지를 결정합니다.
        - all : 모두 충족 시켜야함 (and)
        - any : 하나만 충족시키면 됨 (or)
[출처](https://velog.io/@holicme7/%ED%91%9C%EC%A4%80-%EB%A9%94%EC%84%B8%EC%A7%95-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EC%A0%95%EB%A6%AC-AMQP-STOMP-MQTT)