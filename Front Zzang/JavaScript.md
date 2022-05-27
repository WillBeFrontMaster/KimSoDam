# JavaScript

### 객체 기반의 스크립트 언어

HTML = 웹의 내용 작성

CSS = 웹을 디자인

JavaScript = 웹의 동작 구현 (작성된 HTML의 내용이나 속성, 스타일을 동적으로 변경할 수 있다.)

Node.js 등 프레임워크를 사용하여 서버 프로그래밍도 가능 (Express)

## 자바스크립트 출력

결과물을 HTML 페이지에 출력하는 방법들

1. window.alert() 메소드
    
    window.alert() 메소드는 브라우저와는 별도의 대화 상자를 띄워 사용자에게 데이터를 전달
    
    ```jsx
    <script>
        function alertDialogBox() {
            alert("확인을 누를 때까지 다른 작업을 할 수 없어요!");
        }
    </script>
    ```
    
2. HTML DOM 요소를 이용한 innerHTML 프로퍼티 (가장 많이 사용되는 방법)
    
    ```jsx
    <script>
        var str = document.getElementById("text");
        str.innerHTML = "이 문장으로 바뀌었습니다!";
    </script>
    ```
    
    document 객체의 getElementByID()나 getElementsByTagName() 등의 메소드를 사용하여 
    
    HTML 요소를 선택하고, innerHTML 프로퍼티를 사용하여 요소의 내용이나 속성값을 변경
    
3. document.write() 메소드
    
    ```jsx
    <script>
        document.write(4 * 5);
    </script>
    ```
    
    페이지가 로딩될 때 실행되면 가장 먼저 데이터를 출력한다.
    
    로딩된 후 실행되면 페이지에 먼저 출력된 데이터를 지우고 해당 데이터만 출력.
    
    따라서 대부분 테스트나 디버깅용으로 사용
    
4. console.log() 메소드
    
    ```jsx
    <script>
        console.log(4 * 5);
    </script>
    ```
    
    브라우저의 콘솔을 통해 데이터를 출력해 줍니다.
    

## 자바스크립트 타입 (data type)

### 기본 타입

- 원시 타입 (primitive type)
    - 숫자 (number)
        
        정수와 실수를 따로 구분하지 않고, 모든 수를 실수 하나로만 표현
        
        매우 큰 수나 매우 작은 수 표현에는 e 표기법 사용 가능
        
        ```jsx
        var firstNum = 10;     // 소수점을 사용하지 않은 표현
        var secondNum = 10.00; // 소수점을 사용한 표현
        var thirdNum = 10e6;   // 10000000
        var fourthNum = 10e-6; // 0.00001
        ```
        
    - 문자열 (string)
        
        
    - 불리언 (boolean)
        
        
    - 심볼(symbol)
        
        ECMAScript 6부터 새롭게 추가된 타입
        
        유일하고 변경할 수 없는 타입으로, 객체의 프로퍼티를 위한 식별자로 사용
        
        ```jsx
        var sym = Symbol("javascript");  // symbol 타입
        var symObj = Object(sym);        // object 타입
        ```
        
        *심볼 타입은 익스플로러에서 지원하지 않음.*
        
    - undefined
        
        **null은 아직 값이 정해지지 않은 것.** (object type)
        
        **undefined는 타입이 정해지지 않은 것.**
        
        초기화되지 않은 변수나 존재하지 않는 값에 접근할 때 반환된다.
        
        ```jsx
        var num;          // 초기화하지 않았으므로 undefined 값을 반환함.
        var str = null;   // object 타입의 null 값
        typeof secondNum; // 정의되지 않은 변수에 접근하면 undefined 값을 반환함.
        ```
        
        null과 undefined는 동등 연산자(==)와 일치 연산자(===)로 비교할 때 그 결괏값이 다르므로 주의
        
        타입을 제외하면 같은 의미 (== true) 이지만, 타입이 다르므로 일치하지 않는다. (=== false)
        
- 객체 타입 (object type)
    
    ```jsx
    var num = 10;          // 숫자
    var myName = "김소담";  // 문자열
    var str;              // undefined
    ```
    
    여러 프로퍼티(property)나 메소드(method)를 같은 이름으로 묶어놓은 집합체
    
    ```jsx
    var dog = { name: "해피", age: 3 }; // 객체의 생성
    document.getElementById("result").innerHTML =
        "강아지의 이름은 " + dog.name + "이고, 나이는 " + dog.age + "살 입니다.";
    ```
    

### 타입 변환

```jsx
var num = 20; // Number 타입의 20
num = "이십"; // String 타입의 "이십"
var num;      // 한 변수에 여러 번 대입할 수는 있지만, 변수의 재선언은 불가능. 재선언문은 무시.
```

자바스크립트는 변수의 타입이 정해져 있지 않아 같은 변수에 다른 타입의 값을 다시 대입하는 것이 가능하다.

- 묵시적 타입 변환 (implicit type conversion)
    
    ```jsx
    10 + "문자열"; // 문자열 연결을 위해 숫자 10이 문자열로 변환됨.
    "3" * "5";     // 곱셈 연산을 위해 두 문자열이 모두 숫자로 변환됨.
    1 - "문자열";  // NaN
    ```
    
    의미에 맞게 자동으로 타입을 변환할 수 없는 것은 NaN 값을 반환
    
    NaN : Not a Number의 축약형. 정의되지 않은 값이나 표현할 수 없는 값이라는 의미
    
- 명시적 타입 변환(explicit type conversion)
    
    자바스크립트는 주로 묵시적 타입 변환을 사용하지만 명시적으로 타입을 변환할 방법도 있음.
    
    - 명시적 타입 변환 전역 함수
        1. Number()
        2. String()
        3. Boolean()
        4. Object()
        5. parseInt()
        6. parseFloat()
    
    ```jsx
    Number("10"); // 숫자 10
    String(true); // 문자열 "true"
    Boolean(0);   // 불리언 false
    Object(3);    // new Number(3)와 동일한 결과로 숫자 3
    ```
    

- 숫자를 문자열로 변환
    
    String() 함수 사용하거나 toString() 메소드 사용 (null, undefined 제외)
    
    혹은 다음과 같은 메소드도 사용이 가능
    
    | toExponential() | 정수 부분은 1자리, 소수 부분은 입력받은 수만큼 e 표기법을 사용하여 숫자를 문자열로 변환 |
    | --- | --- |
    | toFixed() | 소수 부분을 입력받은 수만큼 사용하여 숫자를 문자열로 변환 |
    | toPrecision() | 입력받은 수만큼 유효 자릿수를 사용하여 숫자를 문자열로 변환 |
    
- 불리언 값을 문자열로 변환
    
    ```jsx
    String(true);     // 문자열 "true"
    false.toString(); // 문자열 "false"
    ```
    
- 날짜를 문자열이나 숫자로 변환
    - 문자열 변환은 String() 함수와 toString() 메소드로 변환 가능
    
    - 날짜(Date) 객체를 숫자로 변환하는 메소드
        
        
        | getDate() | 날짜 중 일자를 숫자로 반환. (1 ~ 31) |
        | --- | --- |
        | getDay() | 날짜 중 요일을 숫자로 반환. (일요일 : 0 ~ 토요일 : 6) |
        | getFullYear() | 날짜 중 연도를 4자리 숫자로 반환. (yyyy년) |
        | getMonth() | 날짜 중 월을 숫자로 반환. (1월 : 0 ~ 12월 : 11) |
        | getTime() | 1970년 1월 1일부터 현재까지의 시간을 밀리초(millisecond) 단위의 숫자로 반환. |
        | getHours() | 시간 중 시를 숫자로 반환. (0 ~ 23) |
        | getMinutes() | 시간 중 분을 숫자로 반환. (0 ~ 59) |
        | getSeconds() | 시간 중 초를 숫자로 반환. (0 ~ 59) |
        | getMilliseconds() | 시간 중 초를 밀리초(millisecond) 단위의 숫자로 반환. (0 ~ 999) |
        
        ```jsx
        String(Date());        // Mon May 16 2016 19:35:25 GMT+0900
        Date().toString();     // Mon May 16 2016 19:35:25 GMT+0900
        var date = new Date(); // Date 객체 생성
        date.getFullYear();    // 2016
        date.getTime();        
        // 1463394925632 -> 1970년 1월 1일부터 현재까지의 시간을 밀리초 단위의 숫자로 반환.
        ```
        
- 문자열을 숫자로 변환
    
    Number() 함수를 사용하는 것이 가장 간단하다.
    
    그 외 전역 함수
    
    | parseInt() | 자열을 파싱하여 특정 진법의 정수를 반환 |
    | --- | --- |
    | parseFloat() | 문자열을 파싱하여 부동 소수점 수를 반환 |
    
- 불리언 값을 숫자로 변환
    
    Number() 함수 사용
    
    ```jsx
    Number(true);  // 숫자 1
    Number(false); // 숫자 0
    ```