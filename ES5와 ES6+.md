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
>[!info 디폴트 파라미터]
> - 디폴트 파라미터는 함수 매개변수에 기본 값을 설정하는 기능입니다. 
> - 함수를 호출할때 특정 매개변수를 생략하면 설정된 기본값이 사용되게 합니다.
### ==ES5==에서의 디폴트 파라미터 처리
- ES5에서는 디폴트 파라미터가 지원이 되지 않았기 때문에 개발자가 함수 내에서 조건문을 사용해서 매개변수의 기본값을 수동으로 설정해야 했습니다.
```js
function fruit(name) {
	name = name || 'none';
	if(name==='none') {
		throw new Error(`값을 넣어주세요.`);
	}
	console.log(name);
}
fruit() // Error '값을 넣어주세요.'
fruit(`apple`) // apple
```
### ==ES6==에서의 디폴트 파라미터
- ES6에서는 함수의 매개변수에 직접적으로 기본값을 설정할 수 있습니다.
- 함수의 선언만 보아도 매개변수의 기본값을 쉽게 이해할 수 있게 해 줍니다.
```js
function fruit(name = `apple`) {
	console.log(name);
}
```
#### 결론
- ES6의 디폴트 파라미터는 함수의 매개변수의 기본값을 설정함으로서 함으로서 코드의 길이를 줄이고, 함수의 의도를 더 명확히 전달할 수 있게 도와줍니다.
## **레스트 파라미터 (Rest Parameter)**
---
### ==ES5==
- ES5에서는 `레스트 파라미터(Rest Parameters)`라는 개념이 없었습니다. 대신, 함수 내에서 `arguments`객체를 사용하여 전달된 모든 인자에 접근 할 수 있었습니다.
>[!info argument란?]
> - **유사 배열 객체**로, 함수에 전달된 모든 인자를 인덱스 기반으로 저장합니다. 그러나 `arguments`객체는 진짜 배열이 아니므로 배열 메소드를 **직접**사용할 수 없다는 단점이 있습니다. 
![[Pasted image 20240418002847.png]]
```js
function agrumentsFun() {
  console.log(arguments);
}
agrumentsFun(1, 2, 3);
```
배열 메소드를 **직접**사용하지 않고 값을 불러올려면 아래 코드 같이 직접 arguments 인덱스에 접근해서 값을 가져와야 했습니다.
```js
function showArguments() { 
	console.log(arguments.length); 
	for (let i = 0; i < arguments.length; i++) {
		 console.log(`${arguments[i]}`); 
	} 
} 
showArguments(1, 2, 3);
```
![[Pasted image 20240418001912.png]]
### ==ES6==에서의 레스트 파라미터
- ES6에서는 함수의 마지막 매개변수 앞에 `...`을 붙여 나타냅니다.
- 함수에 전달된 나머지 모든 인자를 배열로 직접 받을 수 있습니다.
- 레스트 파라미터는 진정한 배열이므로 `arguments`와 다르게 배열 메소드를 바로 사용할 수 있습니다.
```js
function agrumentsFun(...arr) {
  console.log(arr);
}
agrumentsFun(1,2,3);
```
#### 결론
ES6의 레스트 파라미터는 함수의 매개변수를 효율적으로 다루게 해줍니다. ES5에서 `arguments`객체를 사용하였을때, 배열이 아니라는 점과 배열 메소드를 직접 사용할 수 없었습니다. 하지만 ES6의 레스트 파라미터는 배열을 제공하여 함수에서 필요한 인자를 유연하게 처리할 수 있게 해줍니다.
## **스프레드 파라미터 (Spread Parameter)**
---
#### ==ES5==
- ES5에서는 스프레드 파라미터라는 개념이 없었습니다.
- 유사한 효과를 위해서 `apply`메서드를 사용하여 배열 요소를 개별 인자로 함수에 전달했었습니다.
```js
function sum(x, y, z) { 
	return x + y + z; 
} 
var numbers = [1, 2, 3]; 
console.log(sum.apply(null, numbers)); // 6
```
`apply`는 첫 번째 인자로 `this`를 지정하고, 두 번째 인자는 배열로 전달됩니다. `apply`는 배열을 개별 인자로 분해하여 함수에 전달합니다.
> [!Question 그럼 계속 apply 써도 되는거 아닌가요?]
> - 스프레드 연산자는 코드를 더 직관적이고 간결하게 만들어 줍니다.
> - `apply`는 두 번째 인자로 배열을 받아야 하며, `this`를 지정하는 첫 번째 인자도  필요합니다.
> - 스프레드 연산자는 배열뿐만 아니라 모든 이터러블 객체에 사용할 수 있습니다. 하지만 `apply`는 주로 배열 같은 객체에 한정되어 사용됩니다.
> 
### ==ES6==의 스프레드 파라미터
- 스프레드는 펼치다라는 뜻으로 스프레드 연산자`...`를 사용합니다.
- 넘기고 싶은 배열이나 이터러블을 펼쳐서 인자 목록으로 전달 할 수 있습니다.
```js
function sum(x, y, z) { 
	return x + y + z; 
} 
const numbers = [1, 2, 3]; 
//...numbers는 펼쳐져서 각각 x = 1, y = 2, z = 3으로 입력된다.
console.log(sum(...numbers)); // 6
```
#### 결론
- 스프레드 연산자의 도입으로 코드의 가독성과 유지 관리를 향상 시켰습니다.
- 스프레드 연산자를 사용하면 배열이나 객체를 다루는 작업에서 효과적입니다.
## **객체의 향상된 기능들 (Enhanced Object functionalities)**
---
### ==ES5==에서의 Short hand properties 처리
- ES5에서는 객체의 속성을 정의할 때, 속성 이름과 변수 이름이 같다고 해도 반복적으로 모두 명시해야 했습니다.
```js
var name = "지혁준"; 
var age = 26; 
var person = { 
	name: name, 
	age: age 
}; 
console.log(person); // {name: "지혁준", age: 26}
```
위에 코드 처럼 `name`과 `age`변수를 객체의 속성으로 할당할 때 같은 이름을 두 번 써야 했습니다.
### ==ES6==에서의 Short hand properties 처리
- ES6에서는 객체 리터럴에서 속성의 이름과 변수의 이름이 같을 경우, 한 번만 작성하여 코드를 간결하게 만들 수 있습니다.
```js
let name = "John"; 
let age = 30; 
let person = { 
	name,
	age 
}; 
console.log(person); // {name: "John", age: 30}
```
##### 결론
- Short hand properties는 객체 리터럴에서 코드의 중복을 줄이고, 코드의 작성 및 유지 보수를 쉽게 만들어 줍니다.
- 객체를 많이 사용하는 현대 자바스크립트에서 좋은 이점을 가지고 있습니다.
### ==ES5==에서의 Concise Method 처리
- ES5에서는 객체 내의 메서드를 정의할 때 `function`키워드를 사용해야 했습니다.
- 기능적 문제를 없지만, 코드가 길어지고 때때로 중복되는 패턴이 발생할 수 있습니다.
```js
var calculator = { 
	add: function(x, y) { 
		return x + y; }, 
	subtract: function(x, y) {
		return x - y; }
}; 
console.log(calculator.add(5, 3)); // 8
console.log(calculator.subtract(5, 3));
```
위에 코드는 기존에 객체 내의 메서드를 정의하는 방식입니다.
### ==ES6==에서의 Concise Method 처리
- ES6에서는 객체 리터럴 문법에 간결한 메서드 정의(Concise method)기능이 추가되었습니다.
- 메서드를 정의할때 `function`키워드를 생략할 수 있습니다.
```js
let calculator = { 
	add(x, y) { 
		return x + y; 
	}, 
	subtract(x, y) {
		return x - y; } 
}; 
console.log(calculator.add(5, 3)); // 8 
console.log(calculator.subtract(5, 3)); // 2
```
`function`키워드를 중복으로 사용하지 않기 때문에 많은 메서드를 가지고 있다면 가독성을 높여주고 메서드를 더욱 직관적으로 표현할 수 있게 해줍니다.
### ==ES5==에서의 Property Enumeration Order
- ES5에서 객체 속성의 열거 순서는 엔진에 따라 다를 수 있었습니다.
- 대부분 `JavaScript`엔진은 속성을 추가된 순서대로 열거하지만, 이는 ECMAScript 명세에서 보장되지 않았습니다.
- 숫자 키를 가진 속성의 경우, 일부 브라우저는 숫자 순서대로 정렬하여 열거하는 경우도 있었습니다.
```js
var obj = {
    "20": "Twenty",
    "10": "Ten",
    "1": "One",
    "3": "Three"
};

for (var key in obj) {
    console.log(key + ': ' + obj[key]);
}
// 출력 순서는 브라우저나 JavaScript 엔진에 따라 다를 수 있음
```
위 코드에서 숫자로 된 키가 숫자 순서로 출력될 수 있으며, 엔진에 따라 다를 수 있었습니다.
### ==ES6==에서의 Property Enumeration Order
- ES6에서는 객체 속성의 열거 순서가 명세에 명확히 정의되어 있어 개발자가 예측할 수 있게 되었습니다.
- ES6 명세에 있는 속성의 열거 순서입니다.
	1. 모든 숫자 키는 숫자 순서대로
	2. 모든 문자열 키는 추가된 순서대로
	3. 모든 Symbol 키는 추가된 순서대로
```js
let obj = {
    "20": "Twenty",
    "10": "Ten",
    "1": "One",
    "3": "Three"
};

Object.keys(obj).forEach(key => {
    console.log(key + ': ' + obj[key]);
});
// 출력:
// 1: One
// 3: Three
// 10: Ten
// 20: Twenty
```
#### 결론
- ES6의 명확한 명세는 개발자들이 엔진에 따라 달라지는 것을 신경쓰지 않고 일관된 동작을 할 수 있게 해줬습니다.
## **화살표 함수 (Arrow Function)**
---
### ==ES5==
 - ES5에는 화살표 함수가 존재하지 않았습니다. 그래서 `function`키워드를 사용해야 했으며, `function`을 사용하는 방식은 몇가지 단점을 가지고 있었습니다. `this`키워드의 동작 방식이 단점이었습니다. `function`키워드로 생성된 함수는 자신의 `this`컨텍스트를 생성하므로, 내부 함수에서 외부 함수로의 `this`에 접근 하려면 별도의 변수에 `this`를 할당하거나 `bind`메소드를 사용해야 했습니다.
 - ![[Pasted image 20240418012043.png]]
다른 컨텍스에 존재하기 때문에 this는 최상위 객체인 window를 바라보게 된다
- 최상위 객체의 호출을 막으려면
```js
var func = function() {
    var a = 1;  .
    var self = this; 
    (function() {
        console.log(a);
    })(self);
};

func(); // 1
```
```js
var func = function() {
    var a = 1;
    (function() {
        console.log(this.a);
    }).bind(this)();
};
func(); // 1
```
위에 방식들로 막았어야 했습니다.
### ==ES6==의 화살표 함수
- ES6에서 도입된 화살표 함수는 `function` 키워드 없이 함수를 정의할 수 있으며, 주로 익명 함수로 사용됩니다.
- 중요한 특징은 화살표 함수가 자체적으로 `this`를 생성하지 않습니다.
- 대신, 화살표 함수는 자신이 생성된 스코프의 `this`를 **상속**받아 사용합니다.
- `this`를 상속받아 사용하는 특성은 콜백 함수와 이벤트 핸들러에서 유용하게 활용합니다.
```js
const func = () => {
    console.log(this);  // 상위 스코프의 this를 사용
};
func();
```
#### 결론
- 화살표 함수는 함수 문법을 간소화하고, `this`바인딩에 관한 혼란을 줄여줍니다.
- ES5의 `function`키워드 방식과 비교하여, 화살표 함수는 코드의 간결성을 높이며 `this`의 스코프 관련 문제를 줄여줍니다.
## **디스트럭쳐링 (Destructuring)**
---
### ==ES5==
- 배열이나 객체에서 여러 값을 추출하려면 각 속성이나 인덱스에 직접 접근해서 해당값을 개별 변수에 할당해야 했습니다.
- 개별 변수에 할당해주는 작업은 반복적이고 오류가 발생하기 쉬웠으며, 코드가 길고 번거로웠습니다.
```js
var person = {
    name: "지혁준",
    age: 26,
    address: {
        city: "Seoul",
        country: "Korea"
    }
};

var name = person.name;
var age = person.age;
var city = person.address.city;
var country = person.address.country;

console.log(name, age, city, country);  // 지혁준, 26, Seoul, Korea
```
### ES6의 디스트럭처링
- ES6에서는 배열과 객체의 요소를 더 간단하고 직관적으로 추출할 수 있도록 디스트럭처링을 지원합니다.
- 객체 또는 배열 속성을 변수에 직접 매핑할 수 있습니다.
- 중첩된 객체와 배열에서도 값을 추출할 수 있습니다.
```js
var person = {
    name: "지혁준",
    age: 26,
    address: {
        city: "Seoul",
        country: "Korea"
    }
};


const { name, age, address: { city, country } } = person;

console.log(name, age, city, country);  // 지혁준, 26, Seoul, Korea
```
#### 결론
- ES5에서는 각 속성에 대해 별도의 변수 할당이 필요했습니다
- ES6의 디스트럭처링을 사용하면 한 줄의 코드로 여러 속성을 추출할 수 있습니다.
- API 호출로부터 데이터를 처리하거나, 복잡한 구조를 가진 데이터를 다룰때 유용합니다.
## **제너레이터 (Generator)**
---
#### ==ES5==
- ES5에서는 제너레이터와 같은 개념이 없어서 ES5에서는 함수가 시작부터 끝까지 실행되고, 중간에 실행을 멈추었다가 다시 시작하는 것은 불가능했습니다.
- 복잡한 상태 관리나 비동기 처리를 위해 콜백 함수와 이벤트를 사용하는 방법이 일반적이었습니다.
##### ES5 제너레이터 모방 코드
- 아래는 간단한 반복 작업을 수동으로 관리하는 코드입니다.
```js
function createCounter() {
    var count = 0;
    return {
        next: function() {
            count += 1;
            return { value: count, done: false };
        }
    };
}

var counter = createCounter();
console.log(counter.next().value);  // 1
console.log(counter.next().value);  // 2
console.log(counter.next().value);  // 3

```
#### ==ES6==의 제너레이터
- ES6에서는 제너레이터 함수를 통해서 실행 중간에 멈추었다가 필요한 시점에 다시 시작할 수 있습니다.
- 제너레이터는 `function*`키워드(==\*==별 필수)를 사용하여 정의합니다.
- `yield`표현식을 사용하여 함수 실행을 일시 중지하고, 나중에 재개 할 수 있습니다.
- 이 기능은 복잡한 iterator의 생성, 비동기 프로그래밍 등에 유용하게 사용됩니다.
```js
function* createCounter() {
    let count = 0;
    while (true) {
        yield count += 1;
    }
}

const counter = createCounter();
console.log(counter.next().value);  // 1
console.log(counter.next().value);  // 2
console.log(counter.next().value);  // 3
```
#### 결론
- 제너레이터는 함수의 실행을 제어하고 데이터 스트림을 관리하는데 유용합니다.
- ES5의 모방코드와 비교할때, 제너레이터를 사용하는 ES6의 방법의 코드가 더 간결하고 관리하기 쉽습니다.
- 비동기 처리와 복잡한 데이터 처리 로직에 유용합니다.
## **클래스 (Class)**
---

### ==ES6==
- ES6에서는 `클래스`가 공식적으로 없어서, 대신에 함수와 프로토타입을 사용하여 클래스와 유사한 기능을 구현했습니다.
```js
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.greet = function() {
    console.log("안녕하세요! 저는 " + this.name + "입니다. 저는 " + this.age + "살입니다.");
};

var john = new Person("지혁준", 26);
john.greet();  // 안녕하세요! 저는 지혁준입니다. 저는 26살입니다.
```
위에 코드는 `Person`함수는 생성자 역할을 하고 `Person.prototype`에 메서드를 추가하여 모든 인스턴스가 해당 메서드를 공유하도록 했습니다.

### ES6의 클래스
- 클래스 문법을 도입하여 객체지향 프로그래밍을 사용할 수 있도록 했습니다.
- ES6의 클래스는 내부적으로는 ES5의 프로토타입 기반 패턴을 사용하지만, 명확한 구문(Class로 선언)과 구조를 제공합니다.
```js
Class Person(name, age) {
	constructor(name, age) {
	    this.name = name;
	    this.age = age;
    }
	greet() {
	    console.log("안녕하세요! 저는 " + this.name + "입니다. 저는 " + this.age + "살입니다.");
	}
}


const john = new Person("지혁준", 26);
john.greet();  // 안녕하세요! 저는 지혁준입니다. 저는 26살입니다.
```
- ES6의 클래스 문법은 `constructor`메서드로 객체의 초기화를 수행하고, 메서드를 내부에 직접 정의할 수 있게 합니다.
#### 결론
- ES6의 클래스는 자바스크립트에서 객체지향 프로그래밍을 위한 방법을 제공합니다.
- ES5의 프로토타입과 비교했을때 ES6는 클래스에 익숙한 개발자가 이해하기 좋습니다.
- ES6의 클래스를 활용하여 더욱 구조화되고 모듈화된 코드를 작성할 수 있습니다.
## **프로미스 (Promise)**
---
### ==ES5==
- ES5에서는 프로미스가 포함되어 있지않아서, 비동기 처리를 위해 주로 콜백 패턴을 사용했습니다.
- 콜백 패턴은 중첩되고 복잡한 비동기 코드에서 **콜백 지옥** 문제가 생길 수 있습니다.
### ES6의 프로미스
- ES6에서는 프로미스는 비동기 연산의 최종 성공 또는 실패를 나타내는 객체입니다.
- 프로미스를 사용하면 비동기 작업을 보다 선언적으로 표현할 수 있습니다.
- 콜백 지옥 문제를 효과적으로 해결할 수 있습니다.
## **aync/await**
---

## **모듈 (ES Module)**
---

