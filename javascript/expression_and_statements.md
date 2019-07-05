# 표현식(Experssions) 과 문장(Statements)

- 표현식(Experssions) : 값 하나로 귀결되는 자바스크립트의 코드 조각. 자바스크립트 코드 중 값이 들어가는 곳이면 어디에나 넣을 수 있다.

  - 모든 표현식은 왼쪽에서 오른쪽으로 계신되며, 마지막 값을 리턴한다.

    ```javascript
    console.log(true && 2 * 9); // 18
    ```

  - 표현식은 반드시 상태(state)를 바꿀 필요는 없다.
  - 함수 호출 또한 표현식이다.
  - 네임드 함수와 익명함수는 표현식이다.
    ```javascript
    setTimeout(function() {});
    setTimeout(function foo() {});
    ```

- 문장(Statements) : 무언가 작업을 수행하는 구문. 반환하는 값이 없기 때문에 값이 들어와야할 곳에는 들어갈 수 없다. 때문에 함수의 파라미터, 대입연산의 값, 연산자의 피연산자로 사용될 수 없다.
  - 함수의 선언은 문장이다.
    ```javascript
    function foo(func) {
      return func.name;
    }
    ```
- 표현식을 문장으로 바꾸기 : 표현식 문장(Expression Statements) : 표현식에 세미콜론(;)을 붙이면 표현식이 있는 문장으로 인식.

## IIFEs (Immediately Invoked Function Expression : 즉시 호출되는 함수 표현식)

- 값갑가 들어갈 곳에 괄호를 쓸 수 있다면, 익명함수를 값으로 넘길 수 있다.
- 익명함수를 괄호속에 넣으면 곧바로 같은 익명함수를 return 한다.

```javascript
(function() {
  //동작
})(); // 익명 함수를 선언하고 바로 사용할 수 있음.
```

## 오브젝트 리터럴과 블록문장

- label : breaking loop 를 구성할 때 유용함. 어떠한 표현식이나 표현식 문장에도 붙일 수 있다. 변수를 만드는건 아님.

```javascript
loop: {
  for (let i = 0; i < 5; i++) {
    for (let j = 0; j < 5; j++) {
      console.log(i, j);
      if (j > 1) break loop;
    }
  }
}

// 결과 : 조건에 맞는 순간 라벨 loop 바깥으로 빠져나가 반복문이 종료된다.
// 0 0
// 0 1
// 0 2
```

## 블록 문장

- `{}` 는 문장과 표현식 문장들을 그룹화 시킨다.

  ```javascript
  {
    var a = "b";
    func();
    2 + 2;
  } // 4
  console.log(b);
  ```

  - 블록 문장을 값, 표현식으로는 사용할 수 없다.

- 문장과 표현식을 더할 경우, 문장은 아무런 값도 반환하지 않는다. 하지만 자바스크립트는 error 를 내보내는 대신 `+` 연산자의 피연산자를 숫자나 문자열로 바꾼다. 블록 문장의 경우 암묵적 `0` 으로 강제 형변환 되어 피연산자로 사용된다.

  ```
  {} + 1 // 1

  {2} + 2 // 2

  {2+2} + 3 // 3
  ```

---

## 출처

velog - 자바스크립트 개발자라면 알아야 할 33가지 개념 #7 표현식(Expression)과 문장(Statement) (번역) - 작성자 : @jakeseo_me [https://velog.io/@jakeseo_me/자바스크립트-개발자라면-알아야-할-33가지-개념-7-표현식과-문Statement-번역-2xjuhvbal7#들어가기-전에](https://velog.io/@jakeseo_me/자바스크립트-개발자라면-알아야-할-33가지-개념-7-표현식과-문Statement-번역-2xjuhvbal7#들어가기-전에)
