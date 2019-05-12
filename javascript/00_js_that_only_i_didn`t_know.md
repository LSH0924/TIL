# 나만 몰랐던 자바스크립트

- 자바스크립트의 원시타입은 booleans, null, undefined, number, string, symbol(ES6)으로, 오브젝트는 원시타입에 들어가지 않는다.
- 자바스크립트의 원시타입 데이터들은 불변성(immutable) 을 가지고있다.

## v8

- 구글이 제공하는 오픈소스 자바스크립트 엔진
- 자바스크립트 엔진 : 자바스크립트의 코드를 마이크로프로세서가 이해할 수 있는 더 낮은 수준의 언어나 기계어로 변환해준다. Rhino, JavascriptCore, SpiderMonkey 들은 ES 표준[1](#ES)을 따름.
- C++ 로 작성됐고, Chrome과 Nodejs에서 사용됨.
- ECMA-262에 기재된 ES를 구현했다.
- standalone 으로 동작하기 때문에 C++ 로 만든 프로그램을 내장시킬 수 있다. C++로 만들어진 코드를 자바스크립트에서 동작할 수 있게 할 수 있다는 소리.
  - Nodejs 에 C++ 의 출력구문인 `print("helloworld")` 를 넣어 실행시킬 수 있음.
  - 임의의 C++ 함수를 구현한 뒤 Nodejs에서 사용할 수 있음.

## 생성자 함수

- 리턴 값으로 생성하는 함수를 객체 그 자체로 반환. 같은 코드를 공유하는 여러 객체를 만들 때 사용. `new` 키워드를 붙인다.

  - 생성자 함수는 Object 를 리턴한다. 이 오브젝트에 새로운 프로퍼티들을 할당하고 싶다면, 함수 안에서 `tihs.프로퍼티` 키워드를 사용한다.

  ```javascript
  const Foo = function() {
    this.bar = "baz";
  };
  const bar = new Foo(); // 생성자 함수
  bar; // {bar : "baz"}
  bar instanceof Foo; // true
  bar instanceof Object; // true
  ```

  - `new` 키워드 없이 단순하게 `Foo()` 라는 함수를 실행한다면, 함수 내부의 this 키워드는 실행 컨텍스트(Execution Context) 와 응답을 주고받아 전역 컨텍스트 시점의 this 인 window 객체에 프로퍼티 bar 이 생긴다.
  - 일반 함수를 생성자 함수로 실행하면 함수 오브젝트 자체를 반환한다.

- 오토박싱 : 특정 원시타입에서 프로퍼티, 메소드를 호출하려고 할 때 자바스크립트는 이 원시타입을 임시 Wrapper 오브젝트로 바꾼 뒤 프로퍼티, 메소드에 접근한다.**원본에는 영향 없음**

## == 과 ===

- `===` : 엄격한 동등성을 비교하는 비교 연산자. 타입과 값이 둘 다 같아야 한다.

  ```javascript
  22 === "22"; // false : 같은 값이지만 다른 타입
  "cat" === "domestic short hair"; // false : 같은 타입이지만 값이 다름
  false === 0; // false : 다른 타입, 다른 값

  11 === 11; //true : 같은 값, 같은 타입
  ```

- `==` : 느슨한 동등 비교용. 동등 연산자로 비교하기 전, 피연산자들을 공통 타입으로 강제 형변환시킨다.

  ```javascript
  "cat" === "domestic short hair"; // false : 값이 다름

  22 === "22"; // true : 형변환 시킨 값이 같음
  false === 0; // true : 형변환 시킨 값이 같음
  ```

- `==` 과 `===` 로 참조 타입의 변수들을 비교할 때는 참조 타입의 변수들이 같은 오브젝트를 참조하는지 판단할 때 사용된다.

## 순수 함수(Pure Functions)

- 함수 중에서 함수 바깥의 스코프에 아무런 영향도 미치지 않는 함수. 원시타입만 파라미터로 받고, 주변 스코프에 있는 어떤 함수도 이용하지 않는다. 내부에서 만들어진 모든 변수들은 함수에서 return 이 실시되는 즉시 가비지콜렉션 처리됨.
- 순수 함수 안에서 오브젝트 객체 등 참조형 데이터를 이용하려면, JSON.parse(JSON.stringify(data)) 를 이용해 새로운 객체를 만드는 게 가장 간단

## valueOf

- valueOf 도 따로 정의할 수 있음.
- 객체에 toString, valueOf 가 둘 다 정의되어있을 때, 자바스크립트 엔진은 **valueOf** 메소드를 사용한다.

## **Falsy와 Truthy**

- `true` 로 형변환을 강제하는것을 `truthy`, `false` 로 형변환을 강제하는 것을 `falsy` 라고 한하.
- `falsy` 로 취급되는 값

  - false
  - 0
  - null
  - undefined
  - ""
  - NaN
  - -0

  ```javascript
  false == 0; // true
  0 == ""; // true
  "" == false; // true

  // null 과 undefined 를 비교할 때, 이 둘은 자기 자신과 같으며 서로 같다.
  null == null; // true
  undefined == undefined; // true
  null == undefined; // true
  ```

- `falsy` 로 취급되는 값 외의 다른 값들은 전부 `truthy` 로 취급한다.
- 하지만 역시 값이 참인 것을 명시적으로 표현해주는 게 더 좋은 작성법이다. 그냥 참고로만 알아두자.

## NaN

- NaN 은 자기 자신과도 같지 않은 특별한 숫자값이다.
- 전역 `isNaN` 함수는 파라미터가 실제로 NaN인지 체크하기 전, 파라미터의 값의 형변환을 강제해 비교한다. <sub>그다지 정확하지 않으니 웬만하면 사용하지 않는게 좋음.</sub>

## 함수 호이스팅(끌어올리기)

- `function` 키워드와 함께 선언된 함수들은 항상 자신이 속한 스코프의 가장 위로 호이스팅 된다.
- 변수에 넣은 함수. 함수 표현식으로 작성된 함수들은 자신이 속한 스코프의 가장 위로 호이스팅 되지 않는다.
- 함수는 항상 사용 전에 미리 선언해두자.

```javascript
hello();
sayHello();

function hello() {
  console.log("hello, javascript!");
}

const sayHello = function() {
  console.log("say hello!");
};

// 읽히는 순서는 아래와 같음

// function hello () {
//   console.log("hello, javascript!");
// }

// hello();
// sayHello();

// const sayHello = function () {
//   console.log("say hello!");
// }
```

## 그 외

- 자바스크립트 코드에 `debugger` 키워드를 추가하면 브라우저에서 자바스크립트 실행을 일시정지 하고 디버그 모드를 켠다.
- 이름이 붙은(Named) 함수표현식은 함수의 이름을 가지고 있기 때문에 함수 안에서 스스로를 호출하는 재귀적 용법을 사용할 수 있다.
- setTimeout(function, delayTime) 의 delay는 function이 실행된 뒤의 정확한 시간 딜레이가 아닌, function이 실행됐을 때의 어떤 지점 이후 최소 대기시간을 의미한다.<sub>최소 nn 초 정도는 있다가 뫄뫄하세요 정도?</sub>
- CSS에서 -으로 쓰여진 스타일 속성들은 javscript 에서는 카멜 케이스로 사용한다.


---
<strong id="ES">ECMAScript</strong> 스크립팅 언어가 어떻게 동작할지, 어떤 특성을 가지는지에 대한 표준을 정의. 자바스크립트는 ECMAScript 표준을 기반으로 한다.