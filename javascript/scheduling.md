# 호출 스케줄링 하기 : setTimeout과 setInterval

setTimeout과 setInterval 은 자바스크립트 스펙에는 없지만, 대부분의 환경에서 이 메소드들을 제공하고 있다.

## setTimeout

```javascript
setTimeout(func|code, delay, [arg1, arg2, ....]);
```

- func|code : 실행을 위한 함수나 문자열. 주로 함수를 받고, 코드로 된 문자열을 넘기면 문자열로부터 새로운 함수를 만들지만 문자열을 사용하는것은 권장되지 않는다.
- delay : ms(1000ms = 1s) 단위로 이루어져 있다. default 는 0. 최소한 `delay` 만큼의 시간이 지난 뒤에 `func|code` 를 실행하라.
- [arg1, arg2, ....] : `func|code` 의 함수에 대한 인자. IE9 미만에서는 지원되지 않는다.

```javascript
setTimeout(
  (name, age) => {
    console.log("Hello javascript. My name is %s. I'm %d ages old", name, age);
  },
  2000,
  "Sane",
  26
);
```

- 함수 자체만 넘기고 실행`()`하지는 말것. 실행시켜버리면 함수의 결과값만 넘어감. 결과 값이 없는 함수의 경우 아무것도 스케쥴 되지 않는다.

## clearTimeout

- setTimeout 으로 설정한 스케쥴을 취소시킨다.
- setTimeout 을 호출하면 실행을 취소하기 위해 사용할 수 있는 timer identifier를 준다.

```javascript
const timerId = setTimeout(() => console.log("clearTimeoutTest"), 1000);
console.log(timerId); // timeIdentifier 이 출력. 그때그때 다름

clearTimeout(timerId); // setTimeout 으로 설정한 스케쥴이 취소된다.
```

## setInterval

- setTimeout 과 달리 부여된 delay 간격 이후 주기적으로 함수를 실행시킨다.
- setInterval 은 setTimeout 과 같은 문법을 사용한다.
- setInterval 로 설정한 스케쥴을 취소할 때는 `clearInterval(timerId)` 를 사용.

```javascript
// 2초마다 로그를 출력하는 스케쥴
const timer = setInterval(() => console.log("tick"), 2000);

//5초가 지난 다음 스케줄을 취소한다.
setTimeout(() => {
  clearInterval(timer);
  console.log("stop");
}, 5000);

// ---결과---
// tick
// tick
// stop
```

## 재귀적 setTimeout

- 어떤 작업을 정기적으로 실행시키기 위해서는 `setInterval` 이나 `setTimeout` 의 재귀적 용법을 사용한다.
- 재귀적인 `setTimeout` 은 `setInterval` 보다 유연하게 사용 가능하며, `setInterval`이 보장하지 못하는 실행간 딜레이를 보장할 수 있다.
- `setInterval`의 실행은 지정된 딜레이보다 짧거나 더 길수도 있다. 함수의 실행 또한 interval에 포함되기 때문임. 재귀적인 `setTimeout` 은 새로운 스케쥴이 이전 호출의 끝에 계획되므로 고정된 딜레이를 보장한다.
- `setInterval`의 경우 `clearInterval`이 호출될때까지 함수가 메모리에 남아있으므로, 사용이 끝나면 바로 cancel 시켜주는 게 좋다.

## setTimeout(func [, 0])

- func의 실행을 가능한 빠르게 스케쥴한다.
- 현재 실행중인 코드가 끝난 다음 실행되도록 스케줄함. **비동기적으로 실행**

```javascript
setTimeout(() =>
  console.log("얘는 바깥에 있는 console.log() 가 실행되고 나서 찍힌다.")
);
console.log("얘가 먼저 실행됨.");
console.log("두번째로 실행됨.");
// ---결과---
// 얘가 먼저 실행됨.
// 두번째로 실행됨.
// 얘는 바깥에 있는 console.log() 가 실행되고 나서 찍힌다.
```

## setTimeout 을 이용한 CPU 소비가 많은 작업 spliting

- 예제 : 1부터 1000000000 까지 숫자세기

  - `setTimeout` 을 사용하지 않았을 때

  ```javascript
  let i = 0;

  let start = Date.now();
  // 무거운 작업 하기
  function count() {
    for (let k = 0; k < 1e8; k++) {
      i++;
    }

    console.log("Done in " + (Date.now() - start) / 1000 + "second");
  }

  // Done in 11.11second
  // 실행할때까지 시간은 걸리지만 너무 크거나 하지 않기 때문에 경고메시지 등은 출력하지 않는다.
  ```

  - `setTimeout` 을 사용해서 작업분담

  ```javascript
  let i = 0;

  let start = Date.now();

  function count() {
    // 작업을 시작하기 전 미리 스케쥴을 걸어둔다.
    if (i < 1e9 - 1e6) {
      setTimeout(count);
    }

    do {
      i++;
    } while (i % 1e6 != 0); // 먼저 1e6 까지 세고
  }
  // 이렇게 하면 1e6 까지 세는 동작을 1e9에 도달할때까지 반복하게 된다.

  count();

  // Done in 60.844second
  // 처음에 1e9 를 셀 때는 1분 넘게 무반응이었는데 나눠서 세니 1분가량 걸렸다.
  ```

## 브라우저에서 중첩된 타이머의 딜레이

- 브라우저에는 중첩된 타이머가 얼마나 자주 동착할 수 있는지를 제한한다.
- HTML5 표준은 "5개의 중첩된 타이머 이후엔 간격이 적어도 4ms 만큼은 벌어진다." 라고 함.

```javascript
let start = Date.now();
let time = [];

// setTimeout 으로 설정하기
// [19, 46, 51, 55, 62, 67, 71, 76, 78, 83, 86, 90, 94, 99]
setTimeout(function timerCheck() {
  if (start + 100 < Date.now()) {
    console.log(time);
  } else {
    setTimeout(timerCheck);
  }
  time.push(Date.now() - start);
});

// function 으로 설정해서 호출하기
// [0, 21, 25, 25, 28, 32, 37, 41, 42, 45, 49, 53, 56, 61, 65, 73, 77, 84, 88, 93, 101]
function timerCheck() {
  if (start + 100 < Date.now()) {
    console.log(time);
  } else {
    setTimeout(timerCheck);
  }
  time.push(Date.now() - start);
}

timerCheck();

// 어느 방법으로 해도 크게 차이 나지는 않는다.
```

- 서버사이드 자바스크립트의 경우는 이런 제한이 없다.

## 프로그래스바 보여주기

- 브라우저는 주로 스크립트가 완료된 이후 모든 repainting 작업을 수행한다.
- spliting 할 때 썼던 예제처럼 처리용량이 큰 함수를 실행하면 그 함수가 완료될 때 까지는 리페인팅 되지 않는다.

```html
<body>
  <h1 id="progress"></h1>

  <script>
    let count = 0;
    const progress = document.getElementById("progress");
    console.log(progress);
    function counter() {
      for (let i = 0; i < 1e6; i++) {
        count++;
        progress.innerHTML = count;
      }
    }

    counter();
  </script>
  <!-- 1e6 까지 세기 전에는 console.log()의 출력도, innerHTML 의 리페인팅도
  수행하지 않음. -->
</body>
```

- setTimeout 을 사용하면 작업 중간중간 리페인팅이 실행된다.

```html
<body>
  <h1 id="progress"></h1>

  <script>
    let count = 0;
    const progress = document.getElementById("progress");
    console.log(progress);

    function counter2() {
      if (count < 1e6 - 1e3) setTimeout(counter2);
      do {
        count++;
        progress.innerHTML = count;
      } while (count % 1e3 != 0);
    }

    counter2();
  </script>
</body>

<!-- count 가 1e9가 될 때까지 1e3 단위로 리페인팅된다. -->
```

## 연습 : setTimeout 을 이용한 간단 시계

```html
<body>
  <h1 id="progress"></h1>

  <script>
    const progress = document.getElementById("progress");

    function calculateDatetime() {
      let now = new Date();
      let ampm = now.getHours() / 12 == 1 ? "PM" : "AM";
      let hours = ampm == "PM" ? now.getHours() - 12 : now.getHours();
      let min =
        now.getMinutes() < 10 ? "0" + now.getMinutes() : now.getMinutes();
      let sec =
        now.getSeconds() < 10 ? "0" + now.getSeconds() : now.getSeconds();

      return (
        ampm +
        " " +
        hours +
        " : " +
        min +
        " : " +
        sec +
        " : " +
        now.getMilliseconds() / 1000
      );
    }

    setTimeout(function timer() {
      setTimeout(timer, 100); // 100ms 마다 한번씩 시간을 잼
      progress.innerHTML = calculateDatetime();
    });
  </script>
</body>
```
