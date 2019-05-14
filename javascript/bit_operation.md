# 비트연산

- 바이너리 카운트는 **오른쪽에서 왼쪽으로** 가기 때문에 `1000` 은 4번째 값만 true 인 값이고, `0001` 은 첫번째 값이 true 인 값이다.
- 변수에 대한 n개의 조건을 손쉽게 체크하고 저장할 수 있다.

## <<

- 증가 연산자. 10진법 숫자를 2진법 규칙에 맞게 증가시킨다.
- 바이너리 문자열 오른쪽에 0을 넣어준다.

```javascript
console.log((12).toString(2)); // 1100
console.log((12 << 1).toString(2)); // 바이너리 문자열 오른쪽에 0을 하나 더 추가하면 11000
console.log(12 << 1); // 10진법으로 보면 24
```

## &(교집합)

- 비교하는 두 숫자가 모두 같아야 true(1) 을 반환

```javascript
console.log((10).toString(2));
console.log((5).toString(2));
console.log("-----");
console.log((10 & 5).toString(2));

// 결과 : 아무것도 일치하지 않으므로 false(0) 만 출력됨
// 1010
//  101
// -----
//    0

console.log((10).toString(2));
console.log((6).toString(2));
console.log("-----");
console.log((10 & 6).toString(2));
// 결과 : 끝의 10 이 일치하므로 10(true, false) 이 출력된다.
// 1010
//  110
// -----
//   10
```

## |(합집합)

- 비교하는 두 숫자 중 어느 한쪽만 true(1) 이어도 true(1)를 반환한다.

```javascript
console.log((10).toString(2));
console.log((5).toString(2));
console.log("-----");
console.log((10 | 5).toString(2));

// 결과 : 어느 한쪽의 숫자가 모두 1 이므로 결과는 전부 true가 된다.
// 1010
//  101
// -----
// 1111

console.log((10).toString(2));
console.log((8).toString(2));
console.log("-----");
console.log((10 | 8).toString(2));

// 결과 : 양쪽의 숫자가 모두 false(0) 인 곳은 false(0)가 출력된다.
// 1010
// 1000
// -----
// 1010
```
