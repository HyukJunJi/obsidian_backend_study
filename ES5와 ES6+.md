# 소개
---
**ES**란 **ECMA**라는 국제 기구에서 만든 표준 문서인 **ECMAScript(=ES)** 이고 **ES5**와 **ES6**는 5번째와  6번째 개정판 문서에 있는 표준 스펙을 말합니다. 6번째 버전은 2015년에 나왔기 때문에 **ES2015**라고도 합니다.
# 목차
---
- **var, let, const 변수선언 방법**
- **블록 스코프와 함수 스코프 (Block Scope and Function Scope)**
- **템플릿 리터럴 (Template Literal)**
- **디폴트 파라미터 (Default Parameter)**
- **레스트 파라미터 (Rest Parameter)**
- **스프레드 파라미터 (Spread Parameter)**
- **객체의 향산된 기능들 (Enhanced Object functionalities)**
- **화살표 함수 (Arrow Function)**
- **함수 (Function)**
- **디스트럭쳐링 (Destructuring)**
- **제너레이터 (Generator)**
- **클래스 (Class)**
- **프로미스 (Promise)**
- **aync/await**
- **모듈 (ES Module)**

## **var, let, const 변수선언 방법**
---
### ==**ES5**==
#### var
- 특징
	- 재정의와 재선언이 모두 가능하다.
	- 함수 스코프를 가집니다. 함수 안에서 선언되면 해당 함수 내에서만 유요하고 함수 바깥에서 선언되면 전역 변수가 됩니다.
	- **호이스팅** 특성 때문에 `undefined`를 가집니다.
>[!info Hoisting (호이스팅)이란?]
> - 코드가 실행하기 전 **변수선언/함수선언**이 해당 스코프의 최상단으로 끌어 올려진 **것 같은** 현상을 말한다.
```js
function fruit() {
	console.log(a);
	var a = "apple";
}
fruit();
```
만약 이렇게 `var`을 선언하고 실행하게 되면 호이스팅으로 인해서 `var a`가 최상단으로 끌어올려지고 이때 들어가는 값은 `undefined`이 됩니다.
### ==ES6==
#### let
-  특징
	- 가변변수입니다.
	- 재정의 가능하고 재선언은 불가능합니다.
	- 호이스팅시 `undefined`를 할당 받지 않습니다.
```js
function fruit() {
	console.log(a);
	let a = "apple";
}
fruit(); // ReferenceError
```
호이스팅은 되지만 TDZ로 인해서 `ReferenceError`가 나오게 됩니다.
>[!info TDZ(Temporal Dead Zone)이란?]
> - let, const로 선언한 변수명은 호이스팅이 되었지만 **접근만** 못하게 된 것이다.
> - 위에 코드에서 console.log(a)가 작성된 라인, 해당 **Zone**은 일시적으로 죽은 구역이다.
> - a는 선언문이 나오기 전까지 a에 접근 할 수 없다.
#### const
- 특징
	- 불변변수입니다.
	- 재선언과 재정의 모두 불가능합니다.
	- 호이스팅시 `let`과 같은 특징을 가지고 있습니다.
```js
if(true) {
    const a = 1;
}
console.log(a); // ReferenceError
```
#### 결론
- `var`를 쓰는 것은 좋지않다. 디버깅시에 에러를 잡기 힘들어지기 때문입니다.
- 함수스코프 안에서만 영향을 받기 때문에 블록스코프에서 선언할시에 전역 변수로 할당이 되어 버립니다.
```js
if(true) {
    var a = 1;
}
console.log(a); // 1
window.a; // 1
```
## **블록 스코프와 함수 스코프 (Block Scope and Function Scope)**
---
>[!info 스코프(Scope)란?]
> - 스코프는 변수나 함수가 접근 가능한 범위를 말합니다.
### ==ES5== : 함수 스코프
- ES5에서는 주로 `var`키워드를 사용해서 변수를 선언했습니다. `var`은 선언된 변수는 함수 스코프를 가집니다. 이는 변수가 함수 내부에서 선언되면 그 함수 내부에서만 유효하고, 함수 외부에서 선언된 경우에는 전역 변수가 됩니다.
- `var`은 같은 스코프 내에서 재선언이 가능합니다.

### ==ES6== : 블록 스코프
- ES6에서는 `let`과 `const`를 도입하여 블록 스코프를 지원합니다. 블록 스코프란 중괄호 `{}`로 둘러싸인 "문단"="문"(함수, if문, for문, switch-case문, while문) 내에서 선언된 변수가 그 블록 내에서만 유효하다는 뜻입니다.
- `let`과 `const`는 같은 스코프 내에서 재선언할 수 없습니다.
#### 결론
ES6에 도입으로 인해서 변수의 스코프 관리가 훨씬 명확해지고, 코드의 안정성이 및 예측 가능성이 향상되었습니다.
## **템플릿 리터럴 (Template Literal)**
---
>[!info 템플릿 리터럴이란?]
> - 템플릿 리터럴은 ES6에서 도입된 기능으로, 문자열을 더 편리하게 다룰 수 있게 해줍니다. ES5에서 사용되던 문자열 연결 방식을 향상시키지 위해 만들어졌습니다.
### ==ES5==에서의 문자열 처리
- ES5이하에서 문자열을 처리할 때 주로 `+`연산자를 사용하여 문자열을 연결하였습니다. 이 방법은 복잡한 문자열을 만들때 코드가 길어지고 가독성이 떨어졌습니다.
```js
var name = "지혁준"; 
var greeting = "안녕하세요, " + name + "입니다! 오늘의 날짜는 " + (new Date().toLocaleDateString()) + "입니다."; console.log(greeting); // "안녕하세요, 지혁준입니다! 오늘의 날짜는 4/17/2024입니다."
```
### ==ES6==의 템플릿 리터럴
- ES6에서는 백틱(  \`  \`  )을 사용하여, 내부에서 `${expression}`형식을 통해 직접 표현식을 삽입 할 수 있습니다.
- 특징
	- 다중 줄 지원 : 템플릿 리터럴은 문자열 내에서 직접 줄바꿈을 할 수 있습니다. 즉 여러 줄의 문자열을 쉽게 작성할 수 있습니다. (es5에서는 \n을 사용해야 했습니다.)
	- 표현식 삽입 : `${}`구문을 사용하여 직접적으로 변수뿐만 아니라 표현식을 문자열 내에 삽입 할 수 있습니다.
```js
let name = "지혁준"; 
let greeting = "안녕하세요, ${name}입니다! 오늘의 날짜는 ${new Date().toLocaleDateString()}입니다."; console.log(greeting); // "안녕하세요, 지혁준입니다! 오늘의 날짜는 4/17/2024입니다."
```
#### 결론
ES6에 나온 템플릿 리터럴로 인해서 ES5의 기존 문자열 연결 방식에 비해 코드의 가독성과 작성 효율이 대폭 향상되었습니다. 특히 복잡한 문자열을 다룰 때 아주 효과적입니다.
## **디폴트 파라미터 (Default Parameter)**
---
## **레스트 파라미터 (Rest Parameter)**
---

## **스프레드 파라미터 (Spread Parameter)**
---
## **객체의 향산된 기능들 (Enhanced Object functionalities)**
---

## **화살표 함수 (Arrow Function)**
---

## **함수 (Function)**
---

## **디스트럭쳐링 (Destructuring)**
---

## **제너레이터 (Generator)**
---

## **클래스 (Class)**
---

## **프로미스 (Promise)**
---

## **aync/await**
---

## **모듈 (ES Module)**
---

