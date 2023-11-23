## URI

URI(Uniform Resource Identifier)의 약자인 URI는 '**통합 자원 식별자**'입니다.

- Uniform은 리소스를 식별하는 통일된 방식을 말합니다.
    
- Resource란, URI로 식별이 가능한 모든 종류의 자원(웹 브라우저 파일 및 그 이외의 리소스 포함)을 지칭합니다.
    
- Identifier는 다른 항목과 구분하기 위해 필요한 정보입니다.

즉, URI는 인터넷상의 리소스 ==자원자체==를 식별하는 고유한 문자열 시퀀스

## URL

URL(Uniform Resource Locator)는 ==네트워크 상에서 통합 자원(리소스)의== <u>위치</u>==를 나타내기 위한 규약입니다.==  즉, 자원 식별자와 위치를 동시에 보여준다.
  
웹 사이트 주소 + 컴퓨터 네트워크 상의 자원


## URI와 URL의 차이점

	URI= 식별자, URL=식별자+위치

- elancer.co.kr은 URI입니다. 리소스의 이름만 나타내기 때문입니다.
    

- 반면, https://elancer.co.kr은 URL입니다. 이름과 더불어, 어떻게 도달할 수 있는지 위치까지 함께 나타내기 때문이죠. (프로토콜 ‘https’ 포함)
### URL은 일종의 URI이다.
![사진](https://lh6.googleusercontent.com/2dC-UX4G66CKAuXJuN0JIYmwo7rWdV8cxhOXllbn75D1zNcPqZjgjdusnFpSfuZRaY3cWUzZCZne7TRVhONhXnDiD8OLkGpQIxnu7N35P-20o6I8onKmc2mju_i3HdAK7vtjATNGdUPKlsoFoIMTFwDXUTCJShY9GwsnFgzkgPYPPK8rVJJgU-toMbilow)


### URL은 프로토콜과 결합한 형태이다.

https://www.elancer.co.kr > URL

즉, 어떻게 위치를 찾고 도달할 수 있는지까지 포함되어야 하기 때문에 URL은 프로토콜 + 이름(또는 번호)의 형태여야만 합니다. 

프로토콜(protocol)이란, 리소스에 접근하는 방법을 지정하는 방식입니다. 일반적으로 https, http, ftp 또는 file 등이 여기에 해당할 수 있습니다.

### URI는 그 자체로 이름이 될 수 있다.

>elancer.co.kr > URI

https://www.elancer.co.kr > URL, URI

URI는 그 자체로 이름(elancer.co.kr)이거나, 

이름 + 위치(https://www.elancer.co.kr)를 나타낸 형태 모두가 해당합니다.

식별자+위치를 나타내는 URL은 위의 1번에서 설명했듯이 URI의 일종이기 때문이죠.

## URI URL 구조

![사진](https://lh4.googleusercontent.com/K9t30Sjp6cIbsLg67B-amTH927yS3q06E68DvSLK_4t364b38otQLLibFbyj_I0JH6fLB8tp5zEgVObcaSj810X1Xs2XWuwxD-_ibK8JtfsUaGgm5JNVhUhfjcH3vairIchwhKfviglEmKYYk5aH4cc74ncEaW6N9fRS5VZtKS4EGmvGSYIUSLvx04r0xQ)
`URI, URL, URN 비교`
- Scheme: 리소스에 접근하는 데 사용할 프로토콜. 웹에서는 http 또는 https를 사용
    

- Host: 접근할 대상(서버)의 호스트 명
    

- Path: 접근할 대상(서버)의 경로에 대한 상세 정보

>[!info]
>URN (Uniform Resource Name)
>
>URN은 리소스의 위치, 프로토콜, 호스트 등과는 상관없이 각 자원에 이름을 부여한 것인데요. 즉, URL은 어떤 특정 서버에 있는 웹 문서를 가리키는 반변, URN은 웹 문서의 물리적인 위치와 상관없이 웹 문서 자체를 나타냅니다.
>
>이처럼 개별 자원에 식별자를 부여하게 되면 해당 정보에 대한 URN은 일정하게 유지되며 리소스의 위치, 프로토콜, 호스트와 관계없이 위치를 파악할 수 있다는 장점이 있습니다.
>
>예를 들어, 웹 문서가 다른 웹 서버로 이동하거나 주소가 바뀌는 등 물리적 위치가 변경되더라도 해당 문서를 찾을 수 있습니다.

[출처](https://www.elancer.co.kr/blog/view?seq=74)