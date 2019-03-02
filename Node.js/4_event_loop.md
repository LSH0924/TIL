# Event-loop

- Node.js 기반 서버가 가동 될 때 변수들을 초기화시키고, 함수를 선언하고 나서 이벤트가 일어날 때까지 기다림.
- 이벤트 위주(event-driven) 어플리케이션에서는 이벤트를 대기하는 Event-loop가 존재
- Event-loop에서는 이벤트를 감지하면 콜백 함수(Callback function)을 호출한다.
- 이벤트 핸들링은 옵저버 패턴에 의해 작동된다.
  - 옵서버 패턴(observer pattern)
    - 옵저버 또는 리스너(Listner)라고 불리는 하나 이상의 객체를 관찰 대상이 되는 객체에 등록시킴.
    - 각각의 옵저버는 관찰 대상인 객체가 발생시키는 이벤트(Callback)를 받아 처리한다.
    - 각각의 파생 옵저버는 `notify` 함수를 구현함으로써 이벤트가 발생했을 때 처리할 각자의 동작을 정의한다.
    - 분산 이벤트 핸들링 시스템을 구현하는데 사용한다.
    - 발행/구독 모델이라고도 한다.
- EventListners 함수들이 옵저버 역할을 한다. 이벤트를 기다리다가 이벤트가 실행되면 이벤트를 처리하는 함수를 호출한다.
- Node.js에 내장되어있는 **event** 모듈과 **EventEmitter** 클래스로 이벤트와 이벤트 핸들러를 연동시킬 수 있다.

  ```javascript
  // events 모듈 가져오기
  const events = require("events");
  // EventEmitter 클래스의 객체 생성
  const eventEmitter = new events.EventEmitter();

  // 이벤트 이름(임의지정가능)과 이벤트 핸들러 연동시키기(바인딩하기
  eventEmitter.on("eventName", eventHandler);

  // 프로그램 안에서 이벤트 발생시키기
  eventEmitter.emit("eventName");
  ```

  - 이벤트 핸들러 예제

  ```javascript
  const e = require("events");
  const em = new e.EventEmitter();

  handlerFirst = () => {
    console.log("Application Start");
    em.emit("detonate");
  };
  em.on("start", handlerFirst);

  handlerDetonate = () => {
    console.log("Detect R...");
    let num = 5;
    countDown = setInterval(() => {
      if (num > 0) console.log(num-- + "...");
      else {
        clearInterval(countDown);
        console.log("Boomb!");
        console.log("R is destroyed.");
        console.log("Application has ended");
      }
    }, 500);
  };
  em.on("detonate", handlerDetonate);
  em.emit("start");
  ```

## Error Event

- 에러 발생시, EventEmitter에 `error` 키워드로 등록된 이벤트 리스너가 없으면 예외 스택을 출력 후 프로그램을 종료한다.
- EventEmitter에 `error` 키워드로 등록된 이벤트 리스너가 있을 경우는 등록한 이벤트 리스너를 호출한다.

  ```javascript
  const emitter = require("events");
  const myEmitter = new emitter.EventEmitter();

  myEmitter.once("error", err => {
    console.error("에러발생!");
  });
  myEmitter.emit("error", new Error("Error!"));
  myEmitter.emit("error", new Error("Error!"));
  ```

  - 실행결과

  ```console
  $ node app.js
  에러발생!  // once로 바인딩했던 이벤트 리스너 호출 -> 이제 없어짐
  events.js:167 // 다시 error 발생. 바인딩 된 이벤트 리스너가 없기 때문에 기본 에러 이벤트 발생 -> 프로그램 종료
        throw er; // Unhandled 'error' event
        ^

  Error: Error!
      at Object.<anonymous> (/home/scrapbook/tutorial/app.js:8:27)
      at Module._compile (internal/modules/cjs/loader.js:689:30)
      at Object.Module._extensions..js (internal/modules/cjs/loader.js:700:10)
      at Module.load (internal/modules/cjs/loader.js:599:32)
      at tryModuleLoad (internal/modules/cjs/loader.js:538:12)
      at Function.Module._load (internal/modules/cjs/loader.js:530:3)
      at Function.Module.runMain (internal/modules/cjs/loader.js:742:12)
      at startup (internal/bootstrap/node.js:266:19)
      at bootstrapNodeJSCore (internal/bootstrap/node.js:596:3)
  Emitted 'error' event at:
      at Object.<anonymous> (/home/scrapbook/tutorial/app.js:8:13)
      at Module._compile (internal/modules/cjs/loader.js:689:30)
      [... lines matching original stack trace ...]
      at bootstrapNodeJSCore (internal/bootstrap/node.js:596:3)
  ```

## EventEmitter

- 등록된 모든 이벤트 리스너를 등록 순서대로 동기적으로 호출한다.
  - 이벤트의 적절한 시퀀싱을 보장하고 경쟁 조건이나 논리오류를 피할 수 있음
- 비동기적으로 사용하고 싶으면 setImmediate () 또는 process.nextTick ()를 사용해 비동기 작업모드로 전환해야 한다.

### .on()

- 이벤트의 이름과 연결된 리스너 배열의 끝에 매개변수로 받은 이벤트 리스너를 추가한다.<sub>배열의 맨 앞에 추가하려면 `.prependListener()`</sub>
- .addListener() 도 같은 기능을 한다.
- .on()으로 등록한 이벤트 리스너는 해당 이벤트가 발생할 때마다 호출된다.
- 매개변수로 받은 이벤트 리스너(화살표 함수 외) 내부의 `this` 키워드는 리스너가 첨부된 EventEmitter 인스턴스를 참조한다.
- 매개변수로 받은 이벤트 리스너(화살표 함수) 내부의 `this` 키워드는 EventEmitter 인스턴스를 참조하지 않는다.
- 호출을 연결할 수 있도록 이벤트가 등록되어있는 EventEmitter에 대한 참조를 반환한다.

### .once()

- 한번만 호출되는 리스너를 등록하고싶을 때 사용
- 이벤트의 이름과 연결된 리스너의 배열 끝에 매개변수로 받은 이벤트 리스너를 추가한다.<sub>배열의 맨 앞에 추가하려면 `.prependOnceListener()`</sub>
- .once()으로 등록한 이벤트가 발생하면 해당 이벤트에 대한 리스너가 언바인딩 된 뒤 호출된다.
- 호출을 연결할 수 있도록 이벤트가 등록되어있는 EventEmitter에 대한 참조를 반환한다.
- 리스너가 언바인딩 된 이름의 이벤트는 리스너 배열 안에 다른 이벤트 리스너가 없을 경우 호출을 무시한다.

  ```javascript
  const emitter = require("events");
  const myEmitter = new emitter.EventEmitter();
  let m = 0;
  myEmitter.once("event", () => {
    console.log(++m);
  });
  myEmitter.emit("event");
  // 1
  myEmitter.emit("event");
  // 무시
  ```

### .prependListener(eventName, listener)

- 이벤트의 이름과 연결된 리스너의 배열 맨 앞에 매개변수로 받은 이벤트 리스너를 추가한다.
- 호출을 연결할 수 있도록 이벤트가 등록되어있는 EventEmitter에 대한 참조를 반환한다.

### .prependOnceListener(eventName, listener)

- 이벤트의 이름과 연결된 리스너의 배열 맨 앞에 매개변수로 받은 이벤트 리스너를 추가한다.
- 다음번에 이벤트네임으로 지정한 이벤트가 호출되면 이 리스너가 제거된 다음 호출된다.
- 호출을 연결할 수 있도록 이벤트가 등록되어있는 EventEmitter에 대한 참조를 반환한다.

### .emit()

- 매개변수로 받은 이벤트 이름과 바인딩되어있는 이벤트 리스너를 등록된 순서대로 호출한다.
- EventEmitter에 매개변수로 받은 이벤트 이름과 이벤트 리스너가 바인딩 되어있는 여부에 따라 true / false를 반환한다

### .eventNames()

- EventEmitter에 등록된 이벤트 이름의 배열을 반환
- 배열의 값은 문자열 또는 기호이다.

### .getMaxListeners()

- .setMaxListeners()로 설정한 값이나 기본 EventEmitter가 가질 수 있는 이벤트 리스너의 최대값을 반환한다.

### .listenerCount(eventName)

- 매개변수로 받은 이벤트 이름과 바인딩 된 이벤트 리스너의 개수를 반환한다.

### .listeners(eventName)

- 매개변수로 받은 이벤트 이름과 바인딩 된 이벤트 리스너의 배열을 반환한다.

### .removeListner()

- .off() 도 같은 기능을 한다.

---

- https://velopert.com/267
- https://ko.wikipedia.org/wiki/옵서버_패턴
- https://nodejs.org/api/events.html#events_event_newlistener
