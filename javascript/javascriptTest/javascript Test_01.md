# 테스트

작성한 코드가 정상으로 작동하는지 검증하는 작업.
특정 기능에 대한 수동 테스트를 일일히 할 수 없기 때문에 테스트를 자동화시킨다.
**테스트 자동화**란, 사람이 직접 기능을 확인하는 것이 아니라 테스트용 코드를 작성해서 테스트 시스템이 자동으로 확인해줄 수 있게 하는 작업이다.

## 테스트 자동화의 이점

- 프로젝트를 개발하는 과정에서 새롭게 추가하는 코드가 기존 기능들을 실수로 망가뜨릴 수 없게 미리 막을 수 있다.
- 실제 발생할 수 있게 되는 상황에 대해 미리 결과를 정해두고 그에 맞춰 코드를 작성할 수 있기 때문에 실수로 빠뜨릴 수 있는 사항들을 까먹지 않을 수 있다.
- 코드를 리팩토링할 때 유용하다. 코드를 리팩토링한 후에 해당 기능이 리팩토링 전과 같은 동작을 하는 지 세밀하게 확인해야 하는데, 테스트 코드가 있다면 기능들의 정상 작동 검증이 매우 쉬워지기 때문에 코드의 질 향상에 매우 도움이 된다.
- 버그가 발생했을 때 , 그 버그를 고치고나서 해당 버그가 발생하는 상황에 대한 테스트 코드를 작성해두면 똑같은 실수를 방지할 수 있다.

## 테스트 코드의 분류 : 유닛(Unit)테스트

- 매우 작은 단위로 작성되는 테스트.
- 보통 하나의 파일(한 가지 기능)만 불러와 테스트한다.
- 각 기능이 모두 잘 동작하는지 확인할 수 있으나, 전체적으로 잘 작동하는지는 확인할 수 없다.
- 예시
  - 컴포넌트가 잘 랜더링 될 것
  - 컴포넌트의 특정 함수를 실행하면 예상 결과대로 바뀌는 것
  - 리덕스의 액션 생성함수가 액션 객체를 잘 만들어낼 것
  - 리덕스의 리듀서에 state와 액션 객체를 넣어 호출하면 새로운 state 를 잘 만들어낼 것

## 테스트 코드의 분류 : 통합(Integrated)테스트

- 여러 기능들이 전체적으로 잘 작동하는지 확인하기 위한 테스트.
- 여러 요소들을 테스트하기 위해 여러 파일 혹은 한 파일 안의 여러 가지 기능을 불러와 테스트한다.
- 예시
  - 여러 컴포넌트들을 랜더링 하고 상호작용을 잘 하고 있을 것
  - DOM 이벤트를 발생시켰을 때 UI에 원하는 변화가 잘 발생할 것
  - 리덕스와 연동된 컨테이너 컴포넌트의 DOM 에 특정 이벤트를 발생시켰을 때, 원하는 액션이 잘 디스패치 될 것
  
---

## 자바스크립트 테스트의 기초 : 실습해보기

- 사용하는 테스트 도구 : Jest(페이스북 팀에서 테스트 도구인 Jasmine을 기반으로 만든 테스팅 프레임워크. create-react-app 으로 만든 프로젝트에는 자동으로 적용되어있다.)

### 1. 디렉트리를 만들고 그 안에서 `npm init -y` 를 실행한다.
### 2. jest 도 설치한다.

  ```s
  $ npm install --save jest

  # vsCode 사용자는 @types/jest 을 설치해 vsCode 에서 인텔리센스 지원을 받을 수 있다.
  # $ npm install --save @types/jest
  ```

### 3. 테스트 대상 파일 작성하기 : sum.js

  ```javascript
  function sum(a, b) {
    return a + b;
  }

  // 작성한 함수 내보내기
  module.exports = sum;
  ```

### 4. 테스트 파일 작성하기 : sum.test.js

  ```javascript
  const sum = require("./sum");

  test("1+2=3", () => {
    expect(sum(1, 2)).toBe(3);
  });
  ```

  - test()
    - 새로운 테스트 케이스를 만든다. 첫번째 매개변수로는 테스트의 이름을, 두 번째 매개변수로는 실행할 테스트를 넣는다.
    - `test()` 대신 `it()` 을 사용할 수 있는데, 사용하는 방식은 완전히 같지만 `it()` 은 테스트 케이스의 설명을 영어로 작성할 때 의미를 알아채기 더 쉽다. 영어문장이 되니까.
    - `test()` 안에 `test()`를 넣는 등, 중첩해서 사용할 수 없다. `it()`도 마찬가지.
  - expect() : 특정 값이 ~~일 것이다 라는 사전 정의. 통과하면 테스트를 성공시키고, 통과하지 못하면 실패시킨다.
  - toBe() : matchers. 특정 값이 어떤 조건을 만족하는지 혹은 어떤 함수가 실행됐는지, 에러가 났는지 등을 확인할 수 있게 해준다. 위 코드의 toBe 는 expect 안에서 실행되는 sum(1,2) 가 toBe 안의 값인 3과 같은 값인지를 확인한다.
    - 배열 등 객체를 확인하려면 `toEqual()` 을 사용한다.

### 5. package.json 에 스크립트 설정하기
  - jest를 이용한 테스트를 위해 단축 명령어를 설정한다.
  ```javascript
    // glitch 에서 만들어주는 기본적인 package.json
    // package.json 안의 scripts 항목에 작성한다. scripts 항목이 없으면 json 안에 추가하기.
  {
    "//1": "describes your app and its dependencies",
    "//2": "https://docs.npmjs.com/files/package.json",
    "//3": "updating this file will download and update your packages",
    "name": "hello-express",
    "version": "0.0.1",
    "description": "A simple Node app built on Express, instantly up and running.",
    "main": "server.js",
    // 이 scripts 항목에 삽입
    "scripts": {
      "start": "node server.js",
      "test" : "jest --watchAll --verbose"    
    },
    "dependencies": {
      "express": "^4.16.4"
    },
    "engines": {
      "node": "8.x"
    },
    "repository": {
      "url": "https://glitch.com/edit/#!/hello-express"
    },
    "license": "MIT",
    "keywords": [
      "node",
      "glitch",
      "express"
    ]
  }
  ```
  
### 6. 설정한 단축 명령어로 테스트 실행하기

```s
# yarn이 있으면 yarn test
# 난 지금 npm 을 사용하고 있으므로

$ npm test

# 명령어를 터미널에 입력한다.
```

#### 실행결과 : 테스트 성공
```s
 PASS  ./sum.test.js
  ✓ 1+2=3 (19ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        3.898s
Ran all test suites.

Watch Usage: Press w to show more.
```

#### 실행결과 : 테스트 실패
- 설정한 테스트 파일은 그대로 두고 테스트 대상인 sum.js 의 함수 sum 의 로직을 변경해 다른 결과가 나오게 한 뒤 테스트.
```s
 FAIL  ./sum.test.js
  ✕ 1+2=3 (33ms)

  ● 1+2=3

    expect(received).toBe(expected) // Object.is equality

    Expected: 3   # 반환될 예정이었던 값은 3인데
    Received: 2   # 반환된 값은 2이므로 테스트 실패!

      2 | 
      3 | test("1+2=3", () => {
    > 4 |   expect(sum(1,2)).toBe(3);
        |                    ^
      5 | });

      at Object.toBe (sum.test.js:4:20)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 total
Snapshots:   0 total
Time:        3.437s
Ran all test suites.

Watch Usage: Press w to show more.
```

## describe 를 사용해 여러 개의 테스트케이스 묶기
- 테스트 케이스를 작성할 때 `describe()` 를 사용하면 여러 개의 테스트 케이스를 묶어서 한번에 테스트 할 수 있다.
- `describe()` 안에 또 다른 `describe()` 를 쓸 수 있다.

### 실습 : describe 사용해보기
#### 1. 다른 테스트 대상 파일 만들기 : multi.js
  - 배열로 받은 숫자들을 서로 곱하는 함수를 만들어 내보낸다.

  ```javascript
  function multiply (numbers) {
    let result = 1;
    numbers.forEach(n => {
      result *= n;
    });
    return result;
  }

  module.exports = multiply;
  ```

#### 2. 여러 개의 파일을 불러와 테스트 하는 파일을 만든다. : operation.test.js
  - 테스트 대상 기능이 한 파일 안에서 export 될 경우는 비구조화 할당을 이용한다.
  - 지금은 다른 파일에 있는 기능들을 한데 모아 테스트하므로, 각각 불러와 묶어서 한번에 테스트 하는 코드를 작성한다.
  ```javascript
  const sum = require("./sum");
  const multiply = require("./multi");

  describe("operate well?", () => {
    it("is sum result 3?", () => {
      expect(sum(1,2)).toBe(3);
    });
    it("is multiply result 120?", () => {
      expect(multiply([1,2,3,4,5])).toBe(120);
    });
  });
  ```
#### 3. 테스트를 실행한다.
  ```s
  $ npm test
  ```
##### 실행결과
  ```s
   PASS  ./sum.test.js
    ✓ is sum result 3 (15ms)

   PASS  ./operation.test.js
    operate well?
      ✓ is sum result 3? (13ms)
      ✓ is multiply result 120 (7ms)

  Test Suites: 2 passed, 2 total
  Tests:       3 passed, 3 total
  Snapshots:   0 total
  Time:        6.426s
  Ran all test suites.

  Watch Usage: Press w to show more.
  ```
  - 각 파일별로 테스트를 진행한다.
  - descript 로 묶은 테스트 별 결과가 출력된다.
  
  
## 리팩토링 후에도 기능이 정상 작동하는지 확인할 수 있다.

- multi.js 의 `multiply()` 를 리팩토링 해보자.
```javascript
function multiply (numbers) {
 return numbers.reduce((a, b) => a * b , 1);
}

module.exports = multiply;
```
- 테스트 코드가 리팩토링 전과 같다면, 리팩토링 한 뒤의 기능이 이전과 같이 정상 작동하는지 확인하기 편한다.

### 테스트 실행결과
```s
 PASS  ./sum.test.js
  ✓ is sum result 3 (14ms)
  ✓ is sum result 5 (2ms)

 PASS  ./operation.test.js
  operate well?
    ✓ is sum result 3? (15ms)
    ✓ is multiply result 120? (1ms)

Test Suites: 2 passed, 2 total
Tests:       4 passed, 4 total
Snapshots:   0 total
Time:        5.125s
Ran all test suites.

Watch Usage: Press w to show more.
```

---
## 참고

https://velog.io/@velopert/자바스크립트-테스팅의-기초