# 브라우저에서 렌더링 엔진의 역할 이해
브라우저는 모든 웹 사이트의 기능에서 중추적인 역할을 합니다. 응용 프로그램이 렌더링되는 방식에서 작동 방식에 이르기까지 모든 것은 브라우저와 그 기능에 달려 있습니다. 브라우저 간 호환성 테스트를 통해 원활한 사용자 경험을 제공하려면 브라우저에서 렌더링 엔진의 역할을 이해해야 합니다.
## 웹 브라우저의 아키텍처 이해
브라우저는 프론트엔드 및 백엔드로 빌드됩니다. 프런트엔드는 웹 페이지가 브라우저에 표시되는 방식을 보장하지만 백 엔드는 요청을 처리하고 정보를 전달합니다. 다양한 구성 요소가 조화를 이루어 원할한 웹 경험을 제공합니다.

각 브라우저는 7가지 구성 요소로 구성됩니다.


![사진](https://browserstack.wpenginepowered.com/wp-content/uploads/2022/12/Architecture-of-Web-Browsers-700x564.png)

1. 사용자 인터페이스
	사용자 인터페이스를 사용하면 사용자가 웹페이지에서 사용할 수 있는 모든 시각적 요소와 상호 작용할 수 있습니다. 시각적 요소에는 **주소 표시줄, 홈 버튼, 다음 버튼** 및 최종 사용자가 요청한 웹 페이지를 가져오고 표시하는 기타 모든 요소가 포함됩니다.
2. 브라우저 엔진
	모든 웹 브라우저의 핵심 구성 요소입니다. 브라우저 엔진은 사용자 인터페이스와 렌더링 엔진 사이의 중개자 또는 다리 역할을 합니다. 사용자 인터페이스에서 받은 입력에 따라 렌더링 엔진을 쿼리하고 처리합니다.
	
	브라우저 엔진의 성능과 기능은 웹 브라우저의 사용자 경험에 큰 영향을 미칠 수 있습니다. 빠르고 효율적인 브라우저 엔진은 웹 페이지를 빠르고 원활하게 로드하는 데 도움이 되는 반면, 느리거나 성능이 떨어지는 엔진은 복잡한 페이지를 렌더링하거나 원활한 브라우징 환경을 제공하는 데 어려움을 겪을 수 있습니다.
3. 렌더링 엔진
	이름에서 알 수 있듯이 이 구성 요소는 사용자가 요청한 특정 웹 페이지를 화면에 렌더링하는 역할을 합니다. CSS를 사용하여 스타일이 지정되거나 서식이 지정된 이미지와 함께 HTML 및 XML 문서를 해석하고 최종 레이아웃이 생성되어 사용자 인터페이스에 표시됩니다.
>[!info]
>**참고**: 모든 브라우저에는 고유한 렌더링 엔진이 있습니다. 렌더링 엔진은 브라우저 버전에 따라 다를 수도 있습니다. 아래 목록에는 몇 가지 일반적인 브라우저에서 사용하는 브라우저 엔진이 나와 있습니다.
>1. Google 크롬 및 Opera v.15+: **깜박임**
>2. 인터넷 익스플로러: **트라이던트**
>3. 모질라 파이어폭스: **Gecko**
>4. iOS 및 Safari용 Chrome: **WebKit**
4. 네트워킹
	이 구성 요소는 HTTP 또는 FTP와 같은 표준 프로토콜을 사용하여 네트워크 호출을 관리합니다. 또한 인터넷 통신과 관련된 보안 문제도 관리합니다.
5. JavaScript 인터프리터
	이름에서 알 수 있듯이 웹 사이트에 포함된 JavaScript 코드를 구문 분석하고 실행하는 역할을 합니다. 해석된 결과가 생성되면 사용자 인터페이스에 표시하기 위해 렌더링 엔진으로 전달됩니다.
6. UI 백엔드
	이 구성 요소는 기본 운영 체제의 사용자 인터페이스 메서드를 사용합니다. 주로 기본 위젯(창 및 콤보 상자)을 그리는 데 사용됩니다.
7. 데이터 지속성
	영구 레이어입니다. 웹 브라우저는 다양한 유형의 데이터(예: 쿠키)를 로컬에 저장해야 합니다. 결과적으로 브라우저는 WebSQL, IndexedDB, FileSystem 등과 같은 데이터 저장 메커니즘과 호환되어야 합니다.
	
	이제 웹 브라우저 구축과 관련된 주요 구성 요소를 알았으므로 [[렌더링 엔진]]의 역할에 대해 자세히 알아보겠습니다
	[출처](https://www.browserstack.com/guide/browser-rendering-engine)
[프론트 공부할때 이것도 하자](https://web.dev/articles/howbrowserswork?hl=ko#parser_-_lexer_combination)