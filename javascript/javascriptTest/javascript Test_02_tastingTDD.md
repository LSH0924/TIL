# TDD 맛보기 : TDD(Test Driven Development : 테스트 주도 개발)
- 테스트가 개발을 이끌어 나가는 형태의 개발 이론
- 먼저 테스트 코드를 작성한 다음에 기능을 구현

## TDD 의 3가지 절차

### 1. 실패하는 테스트 케이스 만들기
- 프로젝트의 전체 기능에 대한 테스트가 아니라, 가장 먼저 구현할 기능별로 하나 씩 정상 작동하는지 확인하는 테스트 케이스를 작성한다.
- 아직 테스트 대상 코드가 작성되지 않았기 때문에 지금은 무조건 실패한다.

### 2. 성공하는 테스트 케이스 만들기
- 위에서 만든 테스트 케이스를 통과하기 위해 테스트가 실패하지 않을만한 코드(테스트 대상)를 작성한다.
- 코드가 구현되고 정상적으로 작동하면 테스트 또한 성공한다.

### 3. 리팩토링하기
- 구현한 코드에 중복되는 코드가 있거나, 코드를 개선시킬 방법이 있으면 리팩토링해준다.
- 리팩토링 후에도 1에서 작성한 테스트 케이스가 통과된다면 성공! 다시 1로 돌아가 새로운 기능을 위한 테스트를 작성한다.

## TDD 의 장점
- 테스트 케이스를 작성할 때 작은 단위로 만들기 때문에 코드가 너무 방대해지지 않고, 코드의 모듈화가 자연스럽게 이루어진다.
- 테스트를 먼저 작성한 뒤, 결과에 맞춰 구현하기 때문에 테스트 커버리지<sup><a href="#testCoverage">1</a></sup>가 높아진다. 테스트 커버리지가 높아지면 리팩토링, 유지보수, 협업 또한 쉬워져 프로젝트의 퀄리티를 높이기 좋은 환경이 된다.
- 버그에 낭비하는 시간도 최소한으로 줄일 수 있고, 구현한 기능이 요구사항을 충족하는지 쉽게 확인할 수 있다.
- 개발을 할 때 해결하고자 하는 문제에 집중할 수 있다.

## TDD 연습해보기 : 배열의 최댓값, 최솟값, 평균, 중앙값, 최대 빈도값 구하기

### 연습 1. 배열의 최댓값 구하기
  - 테스트 코드 구현하기 : operation.test.js 재활용
  ```javascript
  const max = require("./operation")

  describe("operate well?", () => {
    // 배열의 최댓값 구하기 테스트
    const arr = [1,2,3,4,5];
    it("5 is max of arr?" , () => {
      expect(max(arr)).toBe(5);
    })
  }); // 지금은 구현된 코드가 없기 때문에 통과되지 않음
  ```
  -  기능 구현하기 : operation.js
  ```javascript
  function max (arr){
    let result = 0;
    arr.map(n => result = result < n ? n : result);
    return result;
  }

  module.exports = max;
  ```
  
  - 실행결과
  ```s
   PASS  ./operation.test.js
    operate well?
      ✓ 5 is max of arr? (13ms)

  Test Suites: 1 passed, 1 total
  Tests:       1 passed, 1 total
  Snapshots:   0 total
  Time:        4.321s
  Ran all test suites.

  Watch Usage: Press w to show more.
  ```
  
  - 더 나은 방법이 있으면 리팩토링 후 다시 결과를 확인한다.
  ```javascript
  module.exports = arr => Math.max(...arr); // 성공!
  ```

### 연습 2. 배열의 최솟값 구하기
  - 테스트 코드 구현하기 : operation.test.js 재활용
  ```javascript
  const { max, min } = require("./operation")

  describe("operate well?", () => {
    // 배열의 최댓값 구하기 테스트
    const arr = [1,2,3,4,5];
    it("5 is max of arr?" , () => {
      expect(max(arr)).toBe(5);
    })
    // 배열의 최솟값 구하기 테스트
    it("1 is min of arr?", () => {
      expect(min(arr)).toBe(1);
    });
  });
  ```
  
  - 기능 구현하기 : operation.js
  ```javascript
  // 한 파일 안에서 여러 개의 함수 내보내기. module 생략가능
  module.exports.max = arr => Math.max(...arr);
  exports.min = arr => Math.min(...arr);
  ```
  
  - 실행결과
  ```s
   PASS  ./operation.test.js
  operate well?
    ✓ 5 is max of arr? (5ms)
    ✓ 1 is min of arr? (1ms)

  Test Suites: 1 passed, 1 total
  Tests:       2 passed, 2 total
  Snapshots:   0 total
  Time:        0.287s, estimated 1s
  Ran all test suites.

  Watch Usage: Press w to show more.
  ```
### 연습 3. 배열의 평균 구하기
  - 테스트 코드 작성하기 : operation.test.js 재활용
  ```javascript
  const { max, min, average } = require("./operation")

  describe("operate well?", () => {
    // 배열의 최댓값 구하기 테스트
    const arr = [1,2,3,4,5];
    it("5 is max of arr?" , () => {
      expect(max(arr)).toBe(5);
    })
    // 배열의 최솟값 구하기 테스트
    it("1 is min of arr?", () => {
      expect(min(arr)).toBe(1);
    });
    // 배열의 평균 구하기 테스트
    it("3 is average of arr?", () => {
      expect(average(arr)).toBe(3);
    });
  });
  ```
  
  - 기능 구현하기 : operation.js
  ```javascript
  // 한 파일 안에서 여러 개의 함수 내보내기. module 생략가능
  module.exports.max = arr => Math.max(...arr);
  exports.min = arr => Math.min(...arr);
  exports.average = arr => arr.reduce((a,b) => a+b, 0)/arr.length;
  // 구조 분해 문법으로 배열의 length 를 따로 분리해서 사용할 수도 있음.
  // exports.average = arr => arr.reduce((a,b, index, {length}) => a+b /length, 0);
  ```

  - 실행 결과
  ```s
   PASS  ./operation.test.js
  operate well?
    ✓ 5 is max of arr? (4ms)
    ✓ 1 is min of arr? (1ms)
    ✓ 3 is average of arr? (2ms)

  Test Suites: 1 passed, 1 total
  Tests:       3 passed, 3 total
  Snapshots:   0 total
  Time:        0.392s, estimated 1s
  Ran all test suites.

  Watch Usage: Press w to show more.
  ```

### 연습 4. 배열의 중앙값 구하기
- 중앙값 : 배열의 요소 중 가장 가운데 있는 값. 값이 짝수개일 경우 중앙에 있는 두 값의 평균을 낸다.
- 테스트 코드 작성하기 : operation.test.js
```javascript
const { max, min, average, sort, median } = require("./operation")

describe("operate well?", () => {
  // 배열의 최댓값 구하기 테스트
  const arr = [1,2,3,4,5];
  it("5 is max of arr?" , () => {
    expect(max(arr)).toBe(5);
  })
  // 배열의 최솟값 구하기 테스트
  it("1 is min of arr?", () => {
    expect(min(arr)).toBe(1);
  });
  // 배열의 평균 구하기 테스트
  it("3 is average of arr?", () => {
    expect(average(arr)).toBe(3);
  });
  // 배열 속 요소들의 중앙값 구하기 테스트
  describe("median", () => {
    // 1. 정렬하기 : 오름차순
    it("sorted desc?", ()=>{
      expect(sort([5,3,2,4,1])).toEqual([1,2,3,4,5]);
    });
    // 2. 중간값 구하기 : 배열의 길이가 짝수일 때
    it("array that even length", ()=>{
      expect(median([1,2,3,4,5,6,7,8])).toBe(4.5);
    });
    // 3. 중간값 구하기 : 배열의 길이가 홀수일 때
    it("array that odd length", () => {
      expect(median([1,2,3,4,5,6,7])).toBe(4);
    });
  });
});
```
- 기능 구현하기 : 정렬, 길이가 홀수, 짝수인 배열의 중앙값 : operation.js
```javascript
// 한 파일 안에서 여러 개의 함수 내보내기. module 생략가능
module.exports.max = arr => Math.max(...arr);
exports.min = arr => Math.min(...arr);
exports.average = arr => arr.reduce((a,b) => a+b, 0)/arr.length;
// exports.average = arr => arr.reduce((a,b, index, {length}) => a+b /length, 0);

exports.sort = (arr, sort) => arr.sort((a,b)=>a-b);
exports.median = arr => {
  const middle = Math.floor(arr.length/2);
  return (arr.length % 2)? arr[middle] : (arr[middle-1] + arr[middle])/2;
};
```

- 실행 결과
```s
 PASS  ./operation.test.js
  operate well?
    ✓ 5 is max of arr? (11ms)
    ✓ 1 is min of arr? (1ms)
    ✓ 3 is average of arr? (1ms)
    median
      ✓ sorted desc? (5ms)
      ✓ array that even length (1ms)
      ✓ array that odd length (1ms)

Test Suites: 1 passed, 1 total
Tests:       6 passed, 6 total
Snapshots:   0 total
Time:        7.386s
Ran all test suites.

Watch Usage: Press w to show more.
```

### 연습 5. 배열의 최대 빈도값 구하기

- 테스트 코드 작성하기 : operation.test.js
```javascript
const { max, min, average, sort, median, frequency } = require("./operation")

describe("operate well?", () => {
  // 배열의 최댓값 구하기 테스트
  const arr = [1,2,3,4,5];
  it("5 is max of arr?" , () => {
    expect(max(arr)).toBe(5);
  })
  // 배열의 최솟값 구하기 테스트
  it("1 is min of arr?", () => {
    expect(min(arr)).toBe(1);
  });
  // 배열의 평균 구하기 테스트
  it("3 is average of arr?", () => {
    expect(average(arr)).toBe(3);
  });
  // 배열 속 요소들의 중앙값 구하기 테스트
  describe("median", () => {
    // 1. 정렬하기 : 오름차순
    it("sorted desc?", ()=>{
      expect(sort([5,3,2,4,1])).toEqual([1,2,3,4,5]);
    });
    // 2. 중간값 구하기 : 배열의 길이가 짝수일 때
    it("array that even length", ()=>{
      expect(median([1,2,3,4,5,6,7,8])).toBe(4.5);
    });
    // 3. 중간값 구하기 : 배열의 길이가 홀수일 때
    it("array that odd length", () => {
      expect(median([1,2,3,4,5,6,7])).toBe(4);
    });
  });
  
  describe("frequency", ()=>{
    // 1. 배열 속 최대 빈도 값이 하나일 때
    it("one", () => {
      expect(frequency([1,2,3,3,3,3,4,5,5,5,5,5,5])).toBe(5);
    });
    // 2. 배열 속 요소들의 빈도가 모두 같을 때
    it("no one", () => {
      expect(frequency(arr)).toBe(null);
    });
    // 3. 배열 속 죄대 빈도 값이 같은 요소가 두 개 아상일 때
    it("someone", () => {
      expect(frequency([1,1,1,1,2,2,3,3,3,3,4,5])).toEqual([1,3]);
    });
  });
});
```
- 기능 구현하기 : operation.js
```javascript
// 한 파일 안에서 여러 개의 함수 내보내기. module 생략가능
module.exports.max = arr => Math.max(...arr);
exports.min = arr => Math.min(...arr);
exports.average = arr => arr.reduce((a,b) => a+b, 0)/arr.length;
// exports.average = arr => arr.reduce((a,b, index, {length}) => a+b /length, 0);

exports.sort = (arr, sort) => arr.sort((a,b)=>a-b);
exports.median = arr => {
  const middle = Math.floor(arr.length/2);
  return (arr.length % 2)? arr[middle] : (arr[middle-1] + arr[middle])/2;
};
// 최대 빈도 값 구하기
exports.frequency = arr => {
  const map = new Map();
  arr.forEach(num => {
    const count = map.get(num) || 0;
    map.set(num, count + 1);
  });
  // 맵에 들어가있는 카운트 개수 중 가장 큰 값 구하기
  const maxCount = Math.max(...map.values());
  // 가장 큰 카운트를 가진 값 찾아내기
  const keys = [...map.keys()].filter(num => map.get(num) === maxCount);
  // 배열 안 모든 요소의 빈도가 같을 때
  if (keys.length === arr.length) return null;
  // 최대 빈도 값이 여러개 이거나 하나일 때
  return (keys.length > 1) ? keys : keys[0];
};
```
- 실행 결과
```s
 PASS  ./operation.test.js
  operate well?
    ✓ 5 is max of arr? (12ms)
    ✓ 1 is min of arr? (13ms)
    ✓ 3 is average of arr? (5ms)
    median
      ✓ sorted desc? (1ms)
      ✓ array that even length (1ms)
      ✓ array that odd length (1ms)
    frequency
      ✓ one (1ms)
      ✓ no one (1ms)
      ✓ someone (1ms)

Test Suites: 1 passed, 1 total
Tests:       9 passed, 9 total
Snapshots:   0 total
Time:        0.9s
Ran all test suites.

Watch Usage: Press w to show more.
```

- 리팩토링 한 뒤 다시 테스트 해보기
```javascript
// 한 파일 안에서 여러 개의 함수 내보내기. module 생략가능
module.exports.max = arr => Math.max(...arr);
exports.min = arr => Math.min(...arr);
exports.average = arr => arr.reduce((a,b) => a+b, 0)/arr.length;
// exports.average = arr => arr.reduce((a,b, index, {length}) => a+b /length, 0);

exports.sort = (arr, sort) => arr.sort((a,b)=>a-b);
exports.median = arr => {
  const middle = Math.floor(arr.length/2);
  return (arr.length % 2)? arr[middle] : (arr[middle-1] + arr[middle])/2;
};
// 최대 빈도 값 구하기
exports.frequency = arr => {
  // acc : 누적값 : 여기서는 지금 map을 만들고 있기 때문에 빈 map 을 반환한다.
  const map = arr.reduce((acc, current) => 
    // 원래 있던 값에 1을 더하거나 1을 넣어주거나
    acc.set(current, acc.get(current) + 1 || 1)    
  , new Map());
  // 맵에 들어가있는 카운트 개수 중 가장 큰 값 구하기
  const maxCount = Math.max(...map.values());
  // 가장 큰 카운트를 가진 값 찾아내기
  const keys = [...map.keys()].filter(num => map.get(num) === maxCount);
  // 배열 안 모든 요소의 빈도가 같을 때
  if (keys.length === arr.length) return null;
  // 최대 빈도 값이 여러개 이거나 하나일 때
  return (keys.length > 1) ? keys : keys[0];
};
```

---

**Test Coverage** <p id = "testCoverage">소프트웨어의 테스트를 논할 때 얼마나 테스트가 충분한가를 나타내는 지표 중 하나.</p>