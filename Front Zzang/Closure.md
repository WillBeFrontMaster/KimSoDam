2022 / 06 / 20
Study
---------------
# **클로저(Closure)**

## **정의**
### **함수와 함수가 선언된 어휘적(lexical) 환경의 조합**
-   클로저는 함수를 지칭하고 또 그 함수가 선언된 환경과의 관계라는 개념이 합쳐짐.
### **클로저의 핵심** 👉 스코프를 이용하여 변수의 접근 범위를 닫는 것. (폐쇄성)
- 외부함수 스코프 👉  내부함수 스코프로 접근 불가능
- 내부 함수 👉 외부 함수에 선언된 변수 접근 가능
### **함수 호출 환경과 별개로, 기존 선언 환경(어휘적 환경)을 기준으로 변수 조회**
- 외부함수 실행이 종료된 후에도 클로저 함수는 외부 함수의 스코프(함수가 선언된 어휘적 환경) 접근 가능
- 회부 함수 스코프가 내부 함수에 의해 언제든 참조 가능
- 클로저 남발할 경우 퍼포먼스 저하
### **상위 스코프의 식별자를 포함하여 쓰여있는 내부 함수 코드 자체를 어휘적 환경(lexical environment)라고 부를 수 있음**

```js
function outerFunc() {
	var x = 10;
	var innerFunc = function() { console.log(x)l };
	innerFunc();
}

outerFunc(); // 10
```
함수 `outerFunc` 내에서 내부함수 `innerFunc`가 선언되고 호출
이 때, 내부함수 `innerFunc`는 자신을 포함하고 있는 외부함수 `outerFunc`의 변수 `x`에 접근할 수 있음.
함수 `innerFunc`가 함수 `outerFunc`의 외부에 선언되었기 때문

---
스코프는 함수를 호출할 때가 아니라 함수를 어디에 선언하였는지에 따라 결정. 
이를 **렉시컬 스코핑(Lexical scoping)** 이라 함.
위 예제의 함수 `innerFunc`는 함수 `outerFunc`의 내부에서 선언되었기 때문에 함수 `innerFunc`의 상위 스코프는 함수 `outerFunc`. 함수 `innerFunc`가 전역에 선언되었다면 함수 `innerFunc`의 상위 스코프는 전역 스코프가 된다.

---

## **클로저 함수의 장점**

### **1. 데이터를 보존할 수 있다.**

클로저 함수는 외부 함수의 실행이 끝나더라도 외부 함수 내 변수를 사용할 수 있음.
특정 데이터를 스코프 안에 가두어 둔 채로 사용할 수 있게 하는 폐쇄성 가짐.

### **2. 정보의 접근 제한 (캡슐화)**

**클로저 모듈 패턴**을 사용하여 객체에 담아 여러 개의 함수를 리턴하도록 만들 수 있음.
이렇게 정보의 접근을 제한하는 것을 **캡슐화**라고 함.
### **3. 모듈화**
클로저 함수를 각각의 변수에 할당하면 각자 독립적으로 값을 사용하고 보존이 가능.
함수의 재사용성을 극대화하고 함수 하나를 독립적인 부품의 형태로 분리하는 것을 **모듈화**라고 함.
클로저를 이용하면 데이터와 메소드를 묶어다닐 수 있어 모듈화에 유리함.


## **클로저 활용**
- 상태유지
	```html
	<!DOCTYPE html>  
	<html>  
	<body>  
		<button class="toggle">toggle</button>  
		<div class="box" style="width: 100px; height: 100px; background: red;"></div>  
		
		<script>  
			var box = document.querySelector('.box');
			var toggleBtn = document.querySelector('.toggle'); 
			var toggle = (function () {
				var  isShow  =  false;
				
				// ① 클로저를 반환 
				return function () {  
					box.style.display = isShow ? 'block' : 'none';  
					// ③ 상태 변경  
					isShow = !isShow; 
				}; 
			})();  
			
			// ② 이벤트 프로퍼티에 클로저를 할당 
			toggleBtn.onclick = toggle;  
		</script>  
	</body>  
	</html>
	```

	① 즉시실행함수는 함수를 반환하고 즉시 소멸. 
		즉시실행함수가 반환한 함수는 자신이 생성됐을 때의 
		렉시컬 환경(Lexical environment)에 속한 변수 `isShow`를 기억하는 클로저. 
		클로저가 기억하는 변수 `isShow`는 `box` 요소의 표시 상태를 나타냄

	② 클로저를 이벤트 핸들러로서 이벤트 프로퍼티에 할당. 
		이벤트 프로퍼티에서 이벤트 핸들러인 클로저를 제거하지 않는 한 
		클로저가 기억하는 렉시컬 환경의 변수 `isShow`는 소멸하지 않음. 
		다시 말해 **현재 상태를 기억**

	③ 버튼을 클릭하면 이벤트 프로퍼티에 할당한 이벤트 핸들러인 클로저가 호출. 
		이때 `.box` 요소의 표시 상태를 나타내는 변수 `isShow`의 값이 변경. 
		변수 `isShow`는 클로저에 의해 참조되고 있기 때문에 유효하며 자신의 변경된 최신 상태를 게속해서 유지.

이처럼 클로저는 현재 상태(위 예제의 경우 `.box` 요소의 표시 상태를 나타내는 `isShow` 변수)를 기억하고 
이 **상태가 변경되어도 최신 상태를 유지해야 하는 상황에 매우 유용.** 

만약 **클로저가 없다면 상태를 유지하기 위해 전역 변수를 사용할 수 밖에 없음.**
전역 변수는 언제든지 **누구나 접근**할 수 있고 **변경**할 수 있기 때문에 많은 **부작용을 유발**해 
**오류의 원인이 되므로 사용을 억제.**


2022 / 06 / 24
Study
---------------
## 2.2 전역 변수의 사용 억제

버튼이 클릭될 때마다 클릭한 횟수가 누적되어 화면에 표시되는 카운터. 
예제의 클릭된 횟수가 바로 유지해야할 상태.

```html
<!DOCTYPE html>  
<html>  
<body>  
	<p>전역 변수를 사용한 Counting</p>  
	<button id="inclease">+</button>  
	<p id="count">0</p>  
	<script>  
		var incleaseBtn = document.getElementById('inclease');  
		var count = document.getElementById('count');  

		// 카운트 상태를 유지하기 위한 전역 변수  
		var counter = 0;  

		function increase() {  
			return ++counter; 
		}  

		incleaseBtn.onclick = function () { 
			count.innerHTML = increase();
		};  
	</script> 
</body> 
</html>
```

위 코드는 잘 동작하지만 오류를 발생시킬 가능성을 내포하고 있는 좋지 않은 코드. 
`increase` 함수는 호출되기 직전에 전역변수 `counter`의 값이 반드시 `0`이여야 제대로 동작. 
하지만 변수 `counter`는 전역 변수이기 때문에 언제든지 누구나 접근할 수 있고 변경 가능. 
이는 의도치 않게 값이 변경될 수 있다는 것을 의미. 

만약 누군가에 의해 의도치 않게 전역 변수 `counter`의 값이 변경됐다면 오류로 이어짐. 
변수 `counter`는 카운터를 관리하는 `increase` 함수가 관리하는 것이 바람직. 

전역 변수 `counter`를 `increase`함수의 지역 변수로 바꾸어 의도치 않은 상태 변경을 방지 가능.

```html
<!DOCTYPE html>  
<html>  
<body>  
	<p>지역 변수를 사용한 Counting</p>  
	<button id="inclease">+</button>  
	<p id="count">0</p>  
	<script>  
		var incleaseBtn = document.getElementById('inclease');  
		var count = document.getElementById('count');  

		function increase() {  
			// 카운트 상태를 유지하기 위한 지역 변수  
			var counter = 0;  
			return ++counter; 
		}  

		incleaseBtn.onclick = function () { 
			count.innerHTML = increase();
		};  
	</script> 
</body> 
</html>
```

전역변수를 지역변수로 변경하여 의도치 않은 상태 변경은 방지. 
하지만 `increase` 함수가 호출될 때마다 지역변수 `counter`를 `0`으로 초기화하기 때문에 언제나 `1`이 표시. 
다시 말해 **변경된 이전 상태를 기억하지 못함.**

이전 상태를 기억하도록 클로저를 사용하여 해결 가능.


```html
<!DOCTYPE html>  
<html>  
<body>  
	<p>클로저를 사용한 Counting</p>  
	<button id="inclease">+</button>  
	<p id="count">0</p>  
	<script>  
		var incleaseBtn = document.getElementById('inclease');  
		var count = document.getElementById('count');  

		var increase(function () {  
			// 카운트 상태를 유지하기 위한 자유 변수  
			var counter = 0;  
			// 클로저를 반환
			return funtion() {
				return ++counter; 
			};
		}());

		incleaseBtn.onclick = function () { 
			count.innerHTML = increase();
		};  
	</script> 
</body> 
</html>
```

스크립트가 실행되면 즉시실행함수(immediately-invoked function expression)가 호출되고 
변수 `increase`에는 함수  `function () { return ++counter; }`가 할당. 

이 함수는 자신이 생성됐을 때의 렉시컬 환경(Lexical environment)을 기억하는 클로저. 
즉시실행함수는 호출된 이후 소멸되지만 즉시실행함수가 반환한 함수는 변수 `increase`에 할당되어 
`inclease` 버튼을 클릭하면 클릭 이벤트 핸들러 내부에서 호출. 

이때 클로저인 이 함수는 자신이 선언됐을 때의 렉시컬 환경인 즉시실행함수의 스코프에 속한 지역변수 `counter`를 기억. 
따라서 즉시실행함수의 변수 `counter`에 접근할 수 있고 변수 `counter`는 자신을 참조하는 함수가 소멸될 때가지 유지.

즉시실행함수는 한번만 실행되므로 `increase`가 호출될 때마다 변수 `counter`가 재차 초기화될 일은 없을 것. 
변수 `counter`는 외부에서 직접 접근할 수 없는  `private`  변수이므로 전역 변수를 사용했을 때와 같이  
**의도되지 않은 변경**을 걱정할 필요도 없음. 
보다 안정적인 프로그래밍이 가능.

변수의 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적 원인이 될 수 있음. 
상태 변경이나 가변(mutable) 데이터를 피하고  **불변성(Immutability)을 지향**하는 함수형 프로그래밍에서  
**부수 효과(Side effect)를 최대한 억제**하여 오류를 피하고 프로그램의 안정성을 높일 수 있는 목적으로 클로저 사용.



함수형 프로그래밍에서 클로저를 활용하는 예제.
```js
// 함수를 인자로 전달받고 함수를 반환하는 고차 함수  
// 이 함수가 반환하는 함수는 클로저로서 카운트 상태를 유지하기 위한 자유 변수 counter을 기억.  
function makeCounter(predicate) {  
	// 카운트 상태를 유지하기 위한 자유 변수  
	var counter = 0;  
	// 클로저를 반환  
	return function () {  
		counter = predicate(counter);  
		return  counter; 
	};  
}  

// 보조 함수  
function increase(n) {  
	return ++n;
}  

// 보조 함수  
function decrease(n) { 
	return --n; 
} 

// 함수로 함수를 생성.  
// makeCounter 함수는 보조 함수를 인자로 전달받아 함수를 반환 
const increaser = makeCounter(increase);  
console.log(increaser()); // 1  
console.log(increaser()); // 2  

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않음.
const decreaser = makeCounter(decrease); 
console.log(decreaser()); // -1  
console.log(decreaser()); // -2
```

함수 `makeCounter`는 보조 함수를 인자로 전달받고 함수를 반환하는 고차 함수. 
함수 `makeCounter`가 반환하는 함수는 자신이 생성됐을 때의 
렉시컬 환경인 함수 `makeCounter`의 스코프에 속한 변수 `counter`을 기억하는 클로저. 

함수 `makeCounter`는 인자로 전달받은 보조 함수를 합성하여 자신이 반환하는 함수의 동작을 변경 가능. 

주의해야 할 것은 함수 `makeCounter`를 호출해 함수를 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 가짐.
함수를 호출하면 그때마다 새로운 렉시컬 환경이 생성되기 때문. 

위 예제에서 변수 `increaser`와 변수 `decreaser`에 할당된 함수는 각각 자신만의 독립된 렉시컬 환경을 갖기 때문에 
카운트를 유지하기 위한 자유 변수 `counter`를 공유하지 않아 카운터의 증감이 연동하지 않음. 

따라서 독립된 카운터가 아니라 연동하여 증감이 가능한 카운터를 만들려면 
렉시컬 환경을 공유하는 클로저를 만들어야 함.
