# new 연산자

## new 연산자가 하는 4가지

1. 비어있는 새 오브젝트를 생성한다.
2. 1에서 새로 만든 오브젝트에 `this` 를 바인드한다.
3. 1에서 새로 만든 오브젝트의 프로퍼티에 생성자함수의 프로포타입 오브젝트를 "**proto**" 라는 이름으로 추가한다.
4. 생성자 함수에서 완성된 오브젝트가 반환될 수 있도록 `return this` 를 함수의 맨 마지막 부분에 추가한다.

```javascript : 먼지 재활용
// 생성자 함수
function Cat(name, age, color) {
  this.name = name;
  this.age = age;
  this.color = color;
}

// 새 고양이 만들기
const munji = new Cat("Munji", 2, "gray and white");
// new 연산자를 사용하면 다음과 같이 동작한다.
// 빈 Cat 객체 생성 -> 빈 Cat 과 this 바인드 -> __proto__ 에 생성자 함수의 프로토 타입 오브젝트 추가 -> 생성자 함수에 return this 추가
console.log(munji.name); // Munji
console.log(munji.color); // gray and white
```

## 프로토타입(prototypes)

- 모든 자바스크립트의 오브젝트는 프로토타입을 가지고 있다.
- 모든 자바스크립트의 오브젝트는 프로토타입에서 메소드, 프로퍼티를 상속받는다.
- 생성자 함수와 `.prototype` 속의 constructor 는 서로를 가리킨다. <sub>루프</sub>

```javascript
function Cat(name, age, color) {
  this.name = name;
  this.age = age;
  this.color = color;
}

console.log(Cat.prototype); // [object Object]{}
const kkong = new Cat("KKong", 9, "three colors tabby");
console.log(kkong.__proto__.constructor);
// function Cat(name, age, color) {
//   this.name = name;
//   this.age = age;
//   this.color = color;
// }
```

- 프로토타입은 생성자 함수로 만들어진 모든 오브젝트 안에서 공유된다. prototype 에서 상속받기 때문.

```javascript : 생성자 함수 Cat 의 prototype 에 함수 추가
Cat.prototype.say = function() {
  console.log("Ya-Ong");
};

kkong.say(); // Ya-Ong
// 새로운 오브젝트 추가 후 다시 해보기
const munji = new Cat("Munji", 2, "gray and white");
munji.say(); // Ya-Ong
```

- 프로토타입의 메소드 오버라이딩하기

```javascript : munji.say 오버라이딩 하기
munji.say = function() {
  console.log("Me-Ong");
};
munji.say(); // Me-Ong
```

- 글로벌 메소드는 그냥 내버려두는게 좋다. 글로벌 메소드의 이름과 겹치는 메소드 작명은 피합시다.
