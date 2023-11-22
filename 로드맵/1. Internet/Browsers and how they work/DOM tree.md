[중요한 렌더링 경로](https://developer.mozilla.org/en-US/docs/Web/Performance/Critical_rendering_path)의 5단계를 설명합니다.

첫 번째 단계는 HTML 마크업을 처리하고 DOM 트리를 빌드하는 것입니다. HTML 구문 분석에는 [토큰화](https://developer.mozilla.org/en-US/docs/Web/API/DOMTokenList) 및 트리 구성이 포함됩니다. HTML 토큰에는 시작 및 끝 태그와 속성 이름 및 값이 포함됩니다. 문서가 올바른 형식이면 구문 분석이 간단하고 빠릅니다. 파서는 토큰화된 입력을 문서에 구문 분석하여 문서 트리를 구축합니다.

DOM 트리는 문서의 내용을 설명합니다. [`<html>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/html) 요소는 문서 트리의 첫 번째 요소이자 루트 노드입니다. 트리는 서로 다른 요소 간의 관계와 계층 구조를 반영합니다. 다른 요소 내에 중첩된 요소는 자식 노드입니다. DOM 노드 수가 많을수록 DOM 트리를 구성하는 데 더 오래 걸립니다.

![사진](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work/dom.gif)

파서가 이미지와 같은 비차단 리소스를 찾으면 브라우저는 해당 리소스를 요청하고 구문 분석을 계속합니다. CSS 파일이 발견되면 구문 분석을 계속할 수 있지만 요소, 특히 [`async`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) 또는 특성이 없는 요소는 렌더링을 차단하고 HTML 구문 분석을 일시 중지합니다. 브라우저의 사전 로드 스캐너가 이 프로세스를 가속화하지만 과도한 스크립트는 여전히 심각한 병목 현상이 될 수 있습니다.`<script>``defer`

[출처](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work#building_the_dom_tree)
[Populating the page: how browsers work - Web performance | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work#building_the_dom_tree)