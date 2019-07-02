# IIFE : Immediately-invoked Function Expression

- 자바스크립트의 표현식 중 하나. 함수를 만들고 식으로 변환한 뒤 즉시 실행한다.
- 쓸데없는 전역변수를 만드는 것을 피함으로써 버그를 줄이는데 도움을 준다.

## 1진 연산자, void 를 사용하는 방식

- IIFE 에서 반환값을 필요로 하지 않을 때 사용한다.

### 1진 연산자 사용

```javascript
!(function() {
  alert("! 를 사용한 IIFE");
})();
// alert 로 "! 를 사용한 IIFE" 가 출력된다.
```

- 1진 연산자라면 아무거나 다 사용할 수 있다. **!나 +나 -나 ~ 등등**
- 1진 연산자를 사용하는 IIFE 는 일회용이다. 한번 실행되면 다시 실행시킬 수 없음.
- 1진 연산자를 사용하면 자바스크립트는 1진 연산자 뒤에 온게 무엇이든 **표현식**으로 다룬다.

### void 사용

```javascript
void (function() {
  alert("void 를 사용한 IIFE");
})();
// alert 로 "void 를 사용한 IIFE" 가 출력된다.
```

- `void` 키워드는 함수를 식으로 다루도록 강제한다.

## Classic IIFE

- IIFE 를 구성하기 위해서는 함수 표현식이 반드시 필요하다. 함수 statement 나 정의는 IIFE 를 만드는데 이용될 수 없다.

```javascript
(function() {
  console.log("아직 실행되지 않은 IIFE 후보");
});

//위의 함수를 IIFE 로 바꾸기 위한 두 가지 방법이 있다. 일단은 아무거나 써도 된다고 한다.

(function iife01() {
  console.log("아직 실행되지 않은 IIFE 후보 - 방법1");
  // 함수 식을 호출하기 위한 () 이 바깥쪽 괄호 안에 있음. 바깥 괄호가 바깥 함수를 함수식으로 만들기 위해 () 가 또 한번 필요하다.
  // 그리고 코드샌드박스도 방법2를 선호하는것같다...
})();

(function iife02() {
  console.log("아직 실행되지 않은 IIFE 후보 - 방법2");
  // 바깥쪽 괄호 뒤에 () 이 존재.
})();
```

## IIFE 와 private 변수

- IIFE 내부에 정의된 모든 변수와 함수는 밖에서 접근할 수 없는 private 요소들이다.
- 코드 바깥에서는 사용하지 않는(한번씩만 사용하는) 변수와 함수 등을 전역으로 만들 때마다 IIFE로 만들면 전역스코프는 오염되지 않고, 코드를 보호할 수 있다.

```javascript
(function IIFE_initGame() {
  // private 변수
  var life;
  var equipment;

  // private 함수. 하지만 init 함수 안에서는 바깥 변수에 접근할 수 있다.
  function init() {
    life = 100;
    equipment = "숏소드";
    console.log("들리나요...");
    setTimeout(() => {
      console.log("아, 들리는군요.... 에린이.. 에린이 위험합니다..");
    }, 1000);
  }
  init();
  console.log(life, equipment);
})();
```

## 값을 return하는 IIFE

- 모듈에서 자주 사용하는 패턴
- return 한 값과 함께 console.log() 를 실행한다.

```javascript
var result = (function() {
  return "값을 return하는 IIFE";
})();

console.log(result);
```

## 파라미터가 있는 IIFE

- 호출할 때 인수(arguments)를 넣어 사용할 수 있다.

```javascript
// 예제코드 1
(function IIFE(msg, times) {
  for (var i = 1; i <= times; i++) {
    console.log(msg);
  }
})("IIFE with parameters", 5);
// 예제코드 2 : jQuery, window, document 객체를 통째로 넘기기
(function($, global, doc) {
  // jQuery를 위해 $를 사용하고, window를 위해 global을 사용.
})(jQuery, window, document);
```

- 예제 코드 2처럼 사용했을 때의 이점
  - 자바스크립트는 항상 현재 함수의 스코프부터 식별자(ID) 를 찾을 때까지 계속 더 높은 레벨의 스코프로 올라가며 식별자를 찾는다. 하지만 인자로 document 등의 객체를 넘겨버리면 IIFE 안에서 이미 받은 document 를 doc.method() 나 doc.property 등으로 사용할 수 있다. <sub>만약 document 를 넘기지 않고 docuemnt.method() 나 document.property 등을 사용한다면 IIFE 내부에 존재하지 않는 document 라는 전역객체를 찾아 돌아다닐것...</sub>
- 자바스크립트의 minifiers 는 함수 안에 선언된 파라미터의 이름을 축소할 수 있다. 따라서 인수로 넘긴 객체들의 이름을 IIFE 안에서 임의로 축약해서 사용할 수 있음(\$, global, doc 등)

## Classical Javascript Module Pattern

- IIFE + closures 로 가장 기본적인 자바스크립트의 모듈 패턴 실습하기

```javascript : 간단 모듈 예제
var Sequence = (function sequenceIIFE() {
  // 현재 counter 값을 저장하기 위한 private 변수
  var current = 0;
  // IIFE 에서 반환되는 객체
  return {
    getCurrentValue: function() {
      return current;
    },
    getNextValue: function() {
      current++;
      return current;
    }
  };
})();

console.log(Sequence.getNextValue()); // 1
console.log(Sequence.getNextValue()); // 2
console.log(Sequence.getCurrentValue()); // 2

// 이 예제에서 반환하는 클로저 외에는 current 변수에 접근할 방법이 없음.
```

## 괄호 생략

- 명확한 함수의 표현식에서는 괄호를 생략할 수 있다.

```javascript
var result = (function() {
  console.log("Omit parentheses");
})();
```

- 하지만 IIFE 를 명확하게 구분하기 위해서는 괄호를 생략하지 않는 편이 낫다.

---

vlog - 자바스크립트 개발자라면 알아야 할 33가지 개념 #8 자바스크립트 필수요소 : IIFE 마스터하기 - 작성자 : @jakeseo_me [https://velog.io/@jakeseo_me/자바스크립트-개발자라면-알아야-할-33가지-개념-8-자바스크립트-필수요소-IIFE-마스터하기#iife와-private변수](https://velog.io/@jakeseo_me/자바스크립트-개발자라면-알아야-할-33가지-개념-8-자바스크립트-필수요소-IIFE-마스터하기#iife와-private변수)
