네트워크 프로토콜에는 실제 데이터를 실어 나르는 데이터 프로토콜과 이 데이터 프로토콜이 잘 동작하도록 도와주는 컨트롤 프로토콜이 존재한다. 컨트롤 프로토콜에는 ARP,ICMP[^1],DNS가 있다. 이 중 DNS는 

***자세한 컨트롤 프로토콜에 대해선 -> 여기서 [[Protocol]]***

웹사이트의 주소는 IP 주소보다 도메인 주소 기반을 자주 사용하는데 이는 사용자가 쉽게 인식하고 기억하기에는 도메인 주소가 적합하기 때문이다.
물론 도메인 주소를 이용하더라도 [[3계층]]/IP 주소를 알아야 하고 이를 위해 문자열로 된 도메인 주소를 실제 통신에 필요한 IP 주소로 변환하는 DNS 정보를 알아야 한다.
사용자가 도메인 주소를 이용하여 서비스를 요청하면 <U>네트워크 설정에 입력한 DNS로 해당 도메인에 대한 IP주소 질의를 보내고 그 결과값으로 요청한 도메인 서비스IP주소를 받게 된다.</U>
![DNS구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb9W3uZ%2FbtrGC67fm31%2FaMF0K0CSUw7X01UAs1ZcjK%2Fimg.png)
1. 사용자가 naver.com을 입력하면 DNS 서버에 naver.com 주소가 무엇인지 질의
2. DNS 서버는 naver.com의 IP 주소가 202.179.177.21이라는 주소를 이용해 실제 naver.com에 접속
>[!info] 참고
>사용자가 서비스를 찾아갈 때뿐만 아니라 내부 시스템의 서비스 간 연결에도 DNS를 사용한다.
>서비스간 연결을 IP로 한다면 IP변경이 필요할 경우, 여러 가지 설정을 변경하거나 프로그램을 재배포해야 한다.
>이때 도메인 주소로 연결하면 DNS서버에서 간단한 설정 변경만으로 복잡한 서비스 간 연결을 쉽게 변경 가능하다.

-----
DNS 구조와 명명 규칙
도메인은 계층 구조여서 수많은 인터넷 주소 중 원하는 주소를 효율적으로 찾아갈 수 있다.
역 트리 구조로 최상위부터 Top-Level, Second-Level, Third-Level와 같이 하위 레벨로 원하는 주소를 단계적으로 찾아간다.
![사진](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbsKWsj%2FbtrGEKhOkq6%2F3zRkAzsjKc0RZgNKeQgr8k%2Fimg.png)
도메인 계층은 최대 128계층까지 구성할 수 있다. 계층별 길이는 최대 63바이트까지 사용할 수 있고 구분자 "."를 포함한 전체 도메인 네임의 길이는 최대 255바이트까지 사용할 수 있다.

#### 루트 도메인
도메인을 구성하는 최상위 영역이고 DNS 서버는 사용자가 쿼리 한 도메인에 대한 값을 직접 갖고 있거나 캐시에 저장된 정보를 이용해 응답한다. 해당 도메인 정보가 없다면 루트 도메인을 관리하는 DNS에 쿼리 한다.

#### Top-Level Domain(TLD)
최상위 도메인인 TLD는 IANA(Internet Assigned Numbers Authority)에서 구분한 6가지 유형으로 구분할 수 있다.
- Generic(gTLD[^2])
- country-code(ccTLD[^3])
- sponsored(sTLD[^4])
- infrastructure[^5]
- generic-restricted(grTLS[^6])
- test(tTLD)[^7]
---
### DNS 동작 방식
도메인을 IP 주소로 변환하려면 <U>DNS 서버에 도메인 쿼리 하는</U> 과정을 거쳐야 한다.
도메인을 쿼리 하면 DNS 서버에 쿼리를 하기 전 로컬에 있는 DNS 캐시 정보를 먼저 확인한다. 이는 동일한 도메인을 매번 질의하지 않고 캐시를 통해 성능을 향상하기 위해서이다.
캐시 정보에는 DNS조회를 통해 확인한 동적 DNS 캐시와 hosts 파일에 저장되어 있는 정적 DNS캐시가 존재한다.
캐시에 필요한 도메인 정보가 없다면 DNS 서버로 쿼리를 수행하고 DNS 서버로부터 응답을 받으면 그 결과를 캐시에 먼저 저장한다.
윈도우에서 DNS캐시를 확인하는 방법은 cmd창에서 `ipconfig/displaydns` 명령을 사용한다.
아래는 캐시와 DNS를 이용해 도메인 이름 쿼리를 하는 예제이다. 로컬 캐시를 조회 후 로컬 캐시에 없다면 DNS 서버로 다시 쿼리한다.
![사진](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbqNeYV%2FbtrGEw5hldT%2FKymhUUfYQpunQ6bi50fH0K%2Fimg.png)
로컬 캐시 유무에 따른 도메인 쿼리 방법

해당 과정은 DNS 시스템 관점에서 도메인에 대한 결과값을 클라이언트에 보내주는 과정이다.
기본적으로 DNS는 분산된 데이터베이스로 서로 도와주도록 설계되었는데 자신이 가진 도메인 정보가 아니면 다른 DNS에 질의해 결과를 받는다.
DNS 기능을 서버에 올리면 DNS서버는 기존적으로 루트 DNS 관련 정보를 가지고 있다. 클라이언트 쿼리가 자신에게 없다면 루트 DNS에 쿼리하고 루트 DSN에서는 쿼리 한 도메인의 TLD 값을 확인해 해당 TLD 값을 관리하는 DNS가 어디인지 응답한다.
전체 과정을 보면 DNS가 중심이 되어 루트부터 상위까지 차근차근 쿼리를 보내 결과값을 알아내고 클라이언트에 응답한다. 호스트는 DNS 서버에 질의했던 방식을 <U> 재귀적 방식</U>, DNS 서버는 여러 단계로 쿼리를 상위 DNS 서버에 보내는데 이를 <U>반복적 쿼리</U>라고 한다.

아래의 예를 통해 쿼리 과정을 설명해보면
![사진](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeQCmm2%2FbtrGEmIGfyc%2FW8I7YHBJxtksN6PiQHR3l1%2Fimg.png)
1. 사용자는 `zigispace.net`이라는 도메인 주소의 IP주소가 로컬 캐시에 저장되어 있는지 확인
2. 로컬 캐시에 저장되어 있지 않으면 사용자 호스트에 설정된 DNS에 `zipispace.net`에 대해 쿼리
3. DNS 서버는 `zipispace.net`이 로컬 캐시와 자체에 설정되어 있는지 직접 확인하고 없으면 해당 도메인을 찾기 위해 루트 NS에 .net에 대한 TLD 정보를 가진 도메인 주소를 쿼리
4. 루트 DNS는 `zigispace`의 TLD인 `.net`을 관리하는 TLD 네임 서버 정보를 DNS에 응답
5. DNS는 TLD 네임 서버에 `zigispace.net`에 대한 정보를 다시 쿼리
6. TLD 네임 서버는 `zigspace.net`에 대한 정보를 가진 zigi 네임 서버에 대한 정보를 DNS 서버로 응답
7. DNS는 zigi 네임 서버에 `zigispace.net`에 대한 정보를 다시 쿼리
8. zigi 네임 서버는 `zigispace.net`에 대한 정보를 DNS 응답
9. DNS는 `zigispace.net`에 대한 정보를 로컬 캐시에 저장하고 사용자 호스트에 `zigispace.net`에 대한 정보를 응답
10. 사용자 호스트는 DNS로부터 받은 `zigispace.net`에 대한 IP정보를 이용해 사이트 접속
[출처](https://rooftoproom-whale.tistory.com/36)

[^1]:ICMP : port(1)
운영체제에서 오류메세지를 전송받는데 주로 쓰인다.
몇몇 진단프로그램을 제외한 남지는 데이터를 교환하지 않는다.
[^2]: com = 일반 기업체, edu = 4년제 이상 교육기관, gov = 미국 연방정부 기관, int = 국제기구, 기관 , mil = 미국 연방군사 기관 , net= 네트워크 관련 기관, org = 비영리기관
[^3]:국가 최상위 도메인으로 ISO 3166 표준에 의해 규정된 두 글자의 국가 코드를 사용한다. 우리나하는 "kr"을 사용한다. ccTLD를 사용하는 경우, gTLD처럼 사이트 용도에 따른 코드를 사용한다. 예를 들어 일반 회사는 co.kr 정부기관은 go.kr을 사용하는 방법이다.
[^4]:특정 목적을 위한 스폰서를 두고 있는 최상위 도메인이다. 종류오른 ".aero", ".asia", ".edu"등이 있다.
[^5]:운용상 중요한 인프라 식별자 공간을 지원하기 위해 전용으로 사용되는 최상위 도메인이다. 종류에는 ".arpa"가 있다.
[^6]:특정 기준으로 충족하는 사람이나 단체가 사용할 수있는 최상위 도메인이다. 종류에는 ".biz", ".name", ".pro"가 있다.
[^7]:IDN(Internationalized Doamin Names) 개발 프로세스에서 테스트 목적으로 사용하는 최상위 도메인이다. 종류에는 ".test"가 있다.
