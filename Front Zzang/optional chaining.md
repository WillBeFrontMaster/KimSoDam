2022 / 06 / 13
Study
---------------

# 옵셔널 체이닝 (optional chaining)

`?.` 을 사용하여 프로퍼티가 없는 중첩 객체를 에러 없이 안전하게 접근하는 방법.

-- _사례1_ --
사용자가 여러 명 있는데, 그 중 몇 명은 주소 정보를 가지고 있지 않다고 가정.
이럴 때 `user.address.street`를 사용하여 주소 정보에 접근하면 에러가 발생한다.
```js
let user = {};
alert(user.address.street); // TypeError : Cannot read property 'street' of undefined
```

-- _사례2_ --
브라우저에서 동작하는 코드를 개발할 때, 자바스크립트를 사용해 페이지에 존재하지 않는 요소에 접근해 요소의 정보를 가져오려 하면 에러가 발생한다.
```js
// querySelector(...) 호출 결과가 null인 경우 에러 발생.
let html = document.querySelector('.my-element').innerHTML;
```
`?.`을 사용하기 전엔 이런 문제들을 해결하기 위해 `&&`연산자를 사용하곤 했음.

-- _예시1_ --
```js
let user = {};
alert(user && user.address && user.address.street); // undefined
```
중첩 객체의 특정 프로퍼티에 접근하기 위해 거쳐야 할 구성요소들을 
AND로 연결해 실제 해당 객체나 프로퍼티가 있는지 확인하는 방법. 

코드가 아주 길어져서 보기 싫음. 으

## 옵셔널 체이닝의 등장 !
`?.`은 앞의 평가 대상이 `undefined`나 `null`이면 평가를 멈추고 `undefined`를 반환.

-- _옵셔널 체이닝을 사용하여 `user.address.street`에 안전하게 접근_ --
```js
let user = {};
alert(user?.address?.street); // undefined
```
`user?.address`로 주소를 읽으면 에러가 발생하지 않는다. (user가 null이어도!)

 `?.`은  `?.`  ‘앞’ 평가 대상에만 동작되고, 확장은 되지 않는다.

참고로 위 예시에서 사용된  `user?.`는  `user`가  `null`이나  `undefined`인 경우만 처리할 수 있다.

`user`가  `null`이나  `undefined`가 아니고 실제 값이 존재하는 경우엔 
반드시  `user.address`  프로퍼티는 있어야 한다. 
그렇지 않으면  `user?.address.street`의 두 번째 점 연산자에서 에러가 발생.

**옵셔널 체이닝을 남용하면 안 됨.**
`?.`는 존재하지 않아도 괜찮은 대상에만 사용.

사용자 주소를 다루는 위 예시에서 논리상  `user`는 반드시 있어야 하는데  
`address`는 필수값이 아니다. 
그러니  `user.address?.street`를 사용하는 것이 바람직.

실수로 인해  `user`에 값을 할당하지 않았다면 바로 알아낼 수 있도록 해야 한다. 
그렇지 않으면 에러를 조기에 발견하지 못하고 디버깅이 어려워짐.


**`?.`앞의 변수는 꼭 선언.**
변수  `user`가 선언되어있지 않으면  `user?.anything`  평가시 에러가 발생.


```js
// ReferenceError: user is not defined
user?.address;
```
`user?.anything`을 사용하려면  `let`이나  `const`,  `var`를 사용해  `user`를 정의. 
옵셔널 체이닝은 선언이 완료된 변수를 대상으로만 동작.

## 단락 평가
`?.`는 왼쪽 평가대상에 값이 없으면 즉시 평가를 멈춘다.
이것을 단락 평가(short-circuit)라고 부르지요.
그래서 `?.` 오른쪽에 있는 부가 동작은 `?.`의 평가가 멈추면 더는 일어나지 않음.


2022 / 06 / 16
Study
---------------
## ?.()와 ?.[]
`?.`은 연산자가 아님.
함수나 대괄호와 함께 동작하는 문법 구조체(syntax construct).

존재 여부가 확실치 않은 함수를 호출할 때 사용.

-- 예시1 : 한 객체엔 메서드 `admin`이 있지만 다른 객체엔 없는 상황.
```js
let user1 = {
	admin() {
		alert("관리자 계정입니다.");
	}
}

let user2 = {};

user1.admin?.();	// 관리자 계정입니다.
user2.admin?.();
```

두 상황 모두 user 객체는 존재하기 때문에 `admin` 프로퍼티는 `.`만 사용하여 접근.

그리고 난 후 `?.()`를 사용해 `admin`의 존재 여부를 확인. `user1`엔 `admin`이 정의되어 있기 때문에 메서드가 제대로 호출 됨. 반면 `user2`에는 `admin`이 정의되어 있지 않았음에도 불구하고 메서드르 호출하면 에러 없이 그냥 평가가 멈추는 것 확인.

`.` 대신 대괄호 `[]`를 사용하여 객체 프로퍼티에 접근하는 경우엔 `?.[]`를 사용할 수 있음.
`?.[]`를 사용하면 객체 존재 여부가 확실치 않은 경우에도 안전하게 프로퍼티 읽기 가능.

```js
let user1 = {
	firstName : "Violet"
};

let user2 = null;	// user2는 권한이 없는 사용자라고 가정.

let key = "firstName";

alert( user1?.[key] );	// Violet
alert( user2?.[key] );	// undefined

alert( user1?.[key]?.something?.not?.existing);	// undefined
```

`?.`은 `delete`와 조합하여 사용 가능.

```js
delete user?.name;	// user가 존재하면 user.name을 삭제
```

`?.`은 읽기나 삭제하기에는 사용할 수 있지만 쓰기에는 사용할 수 없음.
