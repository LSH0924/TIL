# 클로저(Closures)

- 자유 변수(free variable) : 자신을 포함하고 있는 외부 함수의 인자, 지역변수 등을 외부 함수가 종료된 후에도 사용할 수 있음. 단, 클로저를 통해서만 사용할 수 있다.
- 캡쳐(capture) : 클로저가 생성될 때 범위 안의 지역변수들을 자유 변수로 만드는 것.
- 반환용 함수. 보통 return 시키기 위해 함수 안에 반환용 함수를 만든다.
- 사이드 이펙트[1](#sideEffect)를 제어하거나 private 변수를 만들기 위해 사용한다.

```javascript
function outerFunc() {
  const outer = "이것은 바깥쪽 함수 내부의 변수입니다.";

  function innerFunc() {
    console.log(outer);
  }

  return innerFunc();

  // 아래 코드처럼 더 짧게 작성할 수도 있음
  // return function innerFunc() {
  //   console.log(outer);
  // }
}

outerFunc()(); // 이것은 바깥쪽 함수 내부의 변수입니다.
```

## 클로저로 사이드 이펙트 제어하기

- 함수에서 값을 반환하는 것과 별도의 작업이 더 있을 경우, 사이드 이펙트가 발생할 수 있다. Ajax, timeout, console.log 등.

```javascript
function cakeFlavor(flavor) {
  return function makeACake() {
    // 클로저를 사용함으로써 원하는 때에 timeout 을 일으킬 수 있다. : sideEffect 제어
    setTimeout(() => console.log("We make a %s cake!", flavor), 1000);
  };
}
const chocola = cakeFlavor("Choco");

chocola(); // We make a Choco cake!
```

## 클로저로 private 변수에 접근하기

```javascript
function cakeFlavor(flavor) {
  const decoration = "strawberry"; // 함수 내부의 변수
  return function makeACake() {
    setTimeout(
      // 유일하게 변수 decoration 을 노출시킬 수 있다. 이런 함수를 보통 privileged function 이라고 부른다.
      () => console.log("We make a %s %s cake!", flavor, decoration),
      1000
    );
  };
}
const chocola = cakeFlavor("choco");

chocola(); // We make a choco strawberry cake!
```

---

<strong id="sideEffect">사이드 이펙트</strong> 어떤 함수 내에서 자신의 스코프가 아닌 변수들을 제어하는 것.

---

https://offbyone.tistory.com/135

https://velog.io/@jakeseo_me/자바스크립트-개발자라면-알아야-할-33가지-개념-6-함수와-블록-스코프-번역-dijuhrub1x
