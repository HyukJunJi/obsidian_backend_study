---
tags: Internet, network, OSI
---
### **Segment Structure (세그먼트 구조)**

TCP는 데이터 스트림으로부터 데이터를 받아 들여 이것을 일정 단위로 분할한 뒤 TCP 헤더를 덧붙여 TCP 세그먼트를 생성한다. TCP 세그먼트는 ***IP 데이터그램**에 캡슐화되어 상대방과 주고 받게 된다.
>[!question]
>**IP 데이터그램**: IP에서 사용하는 
>**[패킷](Packet)**을 말함 = IP PDU
>**패킷**: 패킷 방식의 컴퓨터 네트워크가 전달하는 데이터의 형식화된 블록이다.

>[!important]
>TCP 패킷이라는 용어가 종종 사용되지만 이는 정확한 표현이 아니다.
>**세그먼트**가 **TCP 프로토콜 데이터 유닛(PDU)을 의미**하는 정확한 표현이며 **데이터그램은 IP PDU**를, **프레임은 데이터 링크 계층 PDU를 의미**한다.

>**TCP 세그먼트 =  세그먼트 헤더(TCP 헤더) + 데이터**

TCP 헤더는 10개의 필수 필드 및 옵션 확장 필드들을 포함한다.

헤더 뒤에는 데이터 섹션이 따라 온다. 

데이터 섹션의 내용은 애플리케이션의 ***페이로드(payload)** 데이터이다.

>[!question]
>*페이로드: 전송의 근본적인 목적이 되는 데이터의 일부분으로 그 데이터와 함께 전송되는 헤더와 메타데이터와 같은 데이터는 제외한다.

데이터 섹션의 길이는 TCP 세그먼트 헤더에서 결정되지 않으며, 전체 IP 데이터그램의 길이에서 TCP 헤더와 캡슐화된 IP 헤더의 길이를 뺀 값으로 계산하게 된다. **즉, 데이터 섹션의 길이는 IP 헤더에 의해 결정된다.**

TCP 헤더 구조
![사진](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F1hSYx%2FbtqwIKyRTIP%2FkmBeBgpBCecIVavlCZYeM1%2Fimg.png)

**Source port (16 비트)**

송신측의 포트 번호 (로컬포트)

[+ ) 커널이 로컬 포트를 선택하는 과정](https://brunch.co.kr/@alden/19)

[[커널]]이란?
  
**Destination port (16 비트)**

수신측의 포트 번호

[+ ) source port vs destination port](https://stackoverflow.com/questions/21253474/source-port-vs-destination-port)

  
**Sequence number (32 비트)**

TCP 세그먼트 안의 데이터의 송신 바이트 흐름의 위치를 가리킨다. 

다른 호스트로 전달되는 패킷은 여러 개의 서로 다른 경로를 거치면서 전달되다 보니 **패킷의 순서가 뒤바뀔 수 있다.** 

이를 수신 측에서는 **재 조립**해야 할 필요가 있는데 이 때 Sequence Number를 이용하여 조립하게 된다.

  
SYN 플래그가 (1)로 설정된 경우, 이것은 초기 시퀀스 번호가 된다. 

SYN 플래그가 (0)으로 해제된 경우, 이것은 현재 세션의 세그먼트 데이터의 누적 시퀀스 번호이다. 

**Acknowledgment number (32 비트)**

ACK 플래그가 설정된 경우 이 필드의 값은 **수신자가 예상하는 다음 시퀀스 번호이다.** 

한쪽이 보낸 최초의 ACK는 반대쪽의 초기 시퀀스 번호 자체에 대한 확인응답이 되며, 데이터에 대한 응답은 포함되지 않는다.

[+ )  [TCP] 신뢰적인 전송 5 : TCP는 실제 Sequence number와 Ack number field에 무엇을 채우는가?](https://blog.naver.com/eleexpert/140056428736)

  
**Data offset (4 비트)**

32-bit 워드 단위로 나타낸 TCP 헤더 크기값이다. 

**헤더의 최소 크기는 5 워드이며 (TCP Option이 없는 경우)** **최대 크기는 15 워드이다.** 

따라서 최솟값은 20바이트, 최댓값은 60바이트가 되며, 헤더에 선택 값을을 위해 최대 40 바이트가 더 추가될 수 있다. 

  
**Reserved (3 비트)**

미래에 사용하기 위해 남겨둔 예비 필드이며 사용중이지 않다. 0으로 채워져야 한다.

  
**Flags (9 **비트**) (혹은 Control bits)**

9개의 1-비트 플래그를 포함
![사진](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbvWC7N%2FbtqwHqBb0XV%2F9wTHTf0xveQGF0oLOfpbEk%2Fimg.png)
[+ ) ECN에 대해서](https://mr-zero.tistory.com/20)

  
**Window size (16 비트)**

수신 윈도의 크기. 해당 세그먼트의 송신측이 현재 수신하고자 하는 윈도 크기(기본 단위는 바이트). 

acknowledgment 필드의 시퀀스 번호보다 큰 값이어야 한다.

[+ ) [오리뎅이의 TCP 이야기 - 4] Window Scaling 옵션은 이젠 선택이 아닌 필수외다](https://m.blog.naver.com/PostView.nhn?blogId=goduck2&logNo=221115915444&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

[+ ) [오리뎅이의 TCP 이야기 - 5] Window Full과 Zero Window는 어떤 관계도 아니다?](https://m.blog.naver.com/goduck2/221117234954)

  
**Checksum:검사합 (16 비트)**

헤더 및 데이터의 에러 확인을 위해 사용되는 16 비트 필드

(별도 문서로 공부)

  
**Urgent pointer (16 비트)**

URG 플래그가 설정된 경우, 이 16 비트 필드는 시퀀스 번호로부터의 오프셋을 나타낸다. 

이 오프셋이 마지막 긴급 데이터 바이트를 가리킨다.

  
**Options (가변 0–320 비트, 32의 배수)**

이 필드의 길이는 데이터 오프셋 필드에 의해 결정된다. 

이 부분은 Option-Kind (1 바이트), Option-Length (1 바이트), Option-Data (가변) 이렇게 최대 3개의 필드로 구성될 수 있다. 

(TCP Options에 대해서는 이해가 잘 안되고 다룰 양이 많아보이므로 추후에 별도 문서로 공부)

**참고문헌**
[출처](https://m.blog.naver.com/PostView.nhn?blogId=ljh0326s&logNo=220888531881&proxyReferer=https%3A%2F%2Fwww.google.com%2F)
[출처](http://m.blog.daum.net/tlos6733/47?np_nil_b=1)
[출처](https://ko.wikipedia.org/wiki/%ED%8E%98%EC%9D%B4%EB%A1%9C%EB%93%9C_(%EC%BB%B4%ED%93%A8%ED%8C%85))
[출처](https://ko.wikipedia.org/wiki/%EC%A0%84%EC%86%A1_%EC%A0%9C%EC%96%B4_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)
[출처](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C_%EB%8D%B0%EC%9D%B4%ED%84%B0_%EB%8B%A8%EC%9C%84)
[출처](https://nogan.tistory.com/20)