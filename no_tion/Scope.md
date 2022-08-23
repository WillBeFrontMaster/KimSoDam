# Scope

```javascript
var x = 'global';  

function foo () {  
	var x = 'function scope'; 
	console.log(x); 
} 

foo(); // function scope 
console.log(x); // global
```

`foo()` 함수 바깥의 변수 `x`는 **전역 변수**, `foo()`함수 안의 x는 **지역 변수**.

**스코프는 참조 대상 식별자를 찾아내기 위한 규칙**.

식별자는 자신이 어디에서 선언되었는지에 의해 유효한(다른 코드가 자신을 참조할 수 있는) 범위를 가짐.

전역 변수 `x`는 어디에든 참조할 수 있으나, 함수 `foo()` 내에 선언된 지역 변수 `x`는 함수 `foo()` 내부에서만 참조할 수 있고 함수 외부에서는 참조할 수 없음. 이 규칙이 **스코프**.

## 스코프의 구분

**전역 스코프 (Global scope)**
: 코드 어디에서나 참조 가능.

**지역 스코프 (Local scope or Function-level scope)**
: 함수 코드 블록이 만든 스코프. 함수 자신과 하위 함수에서만 참조 가능.

_모든 변수는 스코프를 가짐. 변수의 관점에서 구분하는 스코프는_

**전역 변수 (Global variable)**
: 전역에서 선언된 변수이며 어디에든 참조 가능.

**지역 변수 (Local variable)**
: 지역(함수) 내에 선언된 변수, 그 지역과 그 지역의 하부 지역에서만 참조 가능.

## 스코프의 특징
자바스크립트는 **함수 레벨 스코프(function-level scope)**

(C-family language는 블록 레벨 스코프)

`let` 키워드를 사용하면 블록 레벨 스코프 사용 가능.

```js
var x = 0;
{
	var x = 1;
	console.log(x); // 1
}
console.log(x); // 1

let y = 0;
{
	let y = 1;
	console.log(y); // 1
}
console.log(y); // 0
```

## 전역 스코프(Global Scope)

`var` 키워드로 선언한 전역 변수는 **전역 객체 `window`** 의 프로퍼티
```js
var global = 'global';

function foo() {
	var local = 'local';
	console.log(global); // global
	console.log(local); // local
}
foo();

console.log(global); // global
console.log(local); // uncaught ReferenceError: local is not defined
```

자바스크립트는 다른 C-family language와는 달리 특별한 시작점이 없으며 코드가 나타나는 즉시 실행 됨.

그래서 전역에 변수 선언하기 쉬워져 전역 변수를 남발하는 문제 야기.

전역 변수 사용은 **변수 이름이 중복**될 수 있고 **의도치 않은 재할당에 의한 상태 변화로 코드를 예측하기 어렵게 만드므로** 사용 자제.

## 비 블록 레벨 스코프 (Non block-level scope)
```js
if (true) {
	var x = 5;
}
console.log(x);
```

자바스크립트는 블록 레벨 스코프를 사용하지 않아서 **함수 밖에서 선언된 변수는 코드 블록 내에서 선언되었다 하여도 모두 전역 스코프**, 변수 `x`는 전역 변수.

## 함수 레벨 스코프 (function-level scope)
```js
var a = 10;

(function () {
	var b = 20;
})();

console.log(a); // 10
console.log(b); // "b" is not defined
```
자바스크립트는 함수 레벨 스코프 사용. (몇 번째 얘기하는 것임)

즉, 함수 내에서 선언된 매개변수와 변수는 함수 외부에서 유효하지 않음.

고로, 변수 `b`는 지역 변수


```js
var x = 'global';

function foo() {
	var x = 'local';
	console.log(x);
}

foo(); // local
console.log(x); // global
```

전역변수 `x`와 지역변수 `x`가 중복 선언.

전역 영역에서는 전역 변수만이 참조 가능.
함수 내 지역 영역에서는 전역과 지역 변수 모두 참조 가능.

하지만 위와 같이 **변수명이 중복된 경우, 지역변수를 우선 참조.**



```js
var x = 'global';

function foo() {
	var x = 'local';
	console.log(x); // local

	function bar() { // 내부함수
		console.log(x); // local
	}

	bar();
}
foo();
console.log(x); // global
```

내부함수는 자신을 포함하고 있는 외부함수의 변수에 접근 가능.

클로저에서와 같이 내부함수가 더 오래 생존하는 경우, 타 언어와 다른 움직임 보여줌.(먼데요)

함수 `bar`에서 참조하는 변수 `x`는 함수 `foo()`에서 선언된 지역변수이다.

실행 컨텍스트의 스코프 체인에 의해 참조 순위에서 전역 변수 `x`가 뒤로 밀렸기 때문.

```js
var x = 10;

function foo() {
    x = 100;
    console.log(x); // 100
}
foo();
console.log(x); // 100
```

함수(지역) 영역에서 전역 변수를 참조할 수 있으므로 전역 변수의 값도 변경 가능.

```js
var x = 10;

function foo() {
    x = 100;
    console.log(x); // 100
    
    function bar() { // 내부 함수
        x = 1000;
        console.log(x); // 1000
    }
    bar();
}
foo();
console.log(x); // 1000
```

내부 함수의 경우에는 전역 변수는 물론 상위 함수에서 선언한 변수에 접근과 변경 가능.

```js
var foo = function () {

    var a = 3, b = 5;

    var bar = function () {
        var b = 7, c = 11;

        // 이 시점에서 a = 3, b = 7, c = 11
        console.log("bar 내부 a,b,c : " + a,b,c); // 3 7 11

        a += b + c;

        // 이 시점에서 a = 21, b = 7, c = 11
        console.log("더하기 후 : "+a,b,c); // 21 7 11

    };

    // 이 시점에서 a = 3, b = 5, c = not defined
    // 아직 bar()가 실행되지 않아서?
    console.log("bar() 선언 후 : " + a,b); // 3 5

    bar();

    // 이 시점에서 a = 21, b = 5
    // foo() 내의 b와 bar()의 b는 다른 변수라서?
    console.log("bar() 실행 후 : " + a,b); // 21 5
};
```

중첩 스코프는 가장 인접한 지역을 우선 참조

## 렉시컬 스코프 (Lexical scope)

```js
var x = 1;

function foo() {
    var x = 10;
    bar();
}

function bar() {
    console.log(x);
}

foo(); // 1
bar(); // 1
```

위 실행결과는 함수 `bar()`의 상위 스코픅가 무엇인지에 따라 결정.

상위 스코프를 결정할 수 있는 두 가지 패턴
1. 함수를 호출한 시점
2. 함수를 선언한 시점

1번의 방식이면 함수 `bar()`의 상위 스코프는 함수 `foo()`와 전역.

2번의 방식이면 함수 `bar()`의 스코프는 전역.

1번을 _**동적 스코프(Dynamic scope)**_

2번을 _**렉시컬 스코프(Lexical scope) or 정적 스코프(Static scope)**_

대부분의 언어는 렉시컬 스코프를 따름.

함수를 호출한 시점은 스코프 결정에 영향을 주지 않음.

위의 함수 `bar()`는 전역에 선언되었기 때문에 `bar()`의 상위 스코프는 전역 스코프이다.

## 암묵적 전역

```js
var x = 10; // 전역 변수

function foo() {
    // 선언하지 않은 식별자
    y = 20;
    console.log(x + y);
}

foo(); // 30
```

`y`는 선언하지 않은 식별자이지만 값을 할당하면 전역 객체의 프로퍼티가 된다.

1. `foo()`가 호출되면 자바스크립트 엔진은 변수 `y`에 값을 할당하기 위해 먼저 스코프 체인을 통해 선언된 변수인지 확인.
2. `foo()` 함수의 스코프와 전역 스코프 어디에도 변수 `y`의 선언을 찾을 수 없어서 참조 에러가 발생해야 하지만 자바스크립트 엔진에서 동적으로 `window.y = 20`으로 해석하여 프로퍼티를 생성.
3. `y`는 전역 변수처럼 동작

이러한 현상을 **암묵적 전역(implicit global)**

### 주의 :
```js
// 전역 변수 x는 호이스팅이 발생.
console.log(x); // undefined
// 전역 변수가 아닌 전역 프로퍼티 y는 호이스팅이 발생하지 않음.
console.log(y); // ReferrenceError: y is not defined

var x = 10; // 전역 변수

function foo() {
    // 선언하지 않은 변수
    y = 20;
    console.log(x + y);
}

foo(); // 30
```

**_`y`는 변수 선언 없이 전역 객체의 프로퍼티로 추가되었을 뿐이라 변수가 아님. 변수 호이스팅이 발생하지 않음._**




```js
var x = 10; // 전역 변수

function foo() {
    // 선언하지 않은 변수
    y = 20;
    console.log(x + y);
}

foo(); // 30

console.log(window.x); // 10
console.log(window.y); // 20

delete x; // 전역 변수는 삭제되지 않음.
delete y; // 프로퍼티는 삭제 됨.

console.log(window.x); // 10
console.log(window.y); // undefined
```

`y`는 전역 변수가 아닌 프로퍼티. `delete` 연산자로 삭제 가능.

전역 변수 `x`는 프로퍼티여도 `delete` 연산자로 삭제 불가능.


## 최소한의 전역 변수 사용 방법
```js
var MYAPP = {};

MYAPP.student = {
    name: 'Lee',
    gender: 'male'
};

console.log(MYAPP.student.name);
```
애플리케이션에서 전역변수 사용을 위해 전역변수 객체 하나를 만들어 사용.

## 즉시 실행 함수(IIFE, Immediately-Invoked Function Expression)를 이용한 전역 변수 사용 억제
즉시 실행 함수는 말 그대로 즉시 실행되고 그 후 전역에서 바로 사라짐.
```js
(function () {
    var MYAPP = {};
    
    MYAPP.student = {
        name: 'Lee',
        gender: 'male'
    };
    
    console.log(MYAPP.student.name); // Lee
}());

console.log(MYAPP.student.name); // Uncaught ReferenceError: MYAPP is not defined
```

