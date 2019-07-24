# 자바스크립트의 클래스

- ECMAScript2015 에서 도입된 자바스크립트의 프로토 타입 기반 상속을 다루는 문법적 부가기능.
- 새로운 객체지향 상속 모델을 자바스크립트에 도입한 것은 아님.

## 클래스를 이해하기 위한 지식 : 생성자 함수

- 자바스크립트는 함수형 언어라, 자바스크립트 내부에서 클래스를 만들기 위해서는 생성자 함수가 필요하다.
- 생성자 함수의 이용 예시 : 애옹이들 소개

```javascript
function Cat(name, age, color) {
  this.name = name;
  this.age = age;
  this.color = color;
  this.getIntroduce = () =>
    this.name +
    " is " +
    this.age +
    " ages old, and have a(n)" +
    this.color +
    "hair.";
}

const kkong = new Cat("KKong", 9, "cheese and mackerel tabby");
const munji = new Cat("Munji", 2, "gray and white");
const harin = new Cat("Harin", 5, "ivory and darkbrown");

console.log(kkong.getIntroduce());
console.log(munji.getIntroduce());
console.log(harin.getIntroduce());
```

- 문제점
  - `new Cat()` 이라는 코드를 작성할 때, 실제로 자바스크립트 엔진이 하는 일은 각 오브젝트에 생성자 함수를 복사해버린다. **중복된 코드를 계속 생성해버리는것!**
  - 오브젝트를 만들 때 새롭게 추가하고싶은 요소(프로퍼티나 함수)를 추가할 수 없다. 생성자 함수를 고치는 수밖에 없음.

## 클래스를 이해하기 위한 지식 : 프로토타입

- 새 함수를 만들 때마다 자바스크립트 엔진은 기본으로 `prototype` 프로퍼티(프로토 타입 오브젝트)를 추가한다.
- 생성자 함수의 인스턴스가 만들어질 때에도 마찬가지로 다른 프로퍼티, 메소드와 함께 `__property__`<sup>[1](#dunder)</sup> 도 복사됨
- 프로토타입 오브젝트 안에는 만든 함수를 가리키는 생성자 프로퍼티, `__proto__` 오브젝트 프로퍼티가 있다.
- `__property__` 는 생성자 함수에 새로운 프로퍼티, 메소드를 추가할 수 있다.

```javascript : 꽁이의 종을 추가하자.
kkong.__proto__.type = "domestic short hair";
```

- 프로토타입의 프로퍼티와 메소드는 모든 생성자 함수의 인스턴스 사이에서 공유되지만, 생성자 함수로 만든 인스턴스 중 하나의 primitive property 를 변경했을 땐 해당 인스턴스만 반영된다.

```javascript : 애옹이들의 종
console.log(Cat.prototype.type); // domestic short hair
console.log(kkong.type); // domestic short hair
console.log((munji.type = "scottishi fold")); // scottishi fold
console.log((harin.type = "Siamese")); // Siamese
console.log(
  "꽁이는 " +
    kkong.type +
    "이고, 먼지는 " +
    munji.type +
    "이고, 하린이는 " +
    harin.type +
    " 이다."
);
// 꽁이는 domestic short hair이고, 먼지는 scottishi fold이고, 하린이는 Siamese 이다.
```

- 참조 타입 프로퍼티는 항상 모든 인스턴스 사이에서 공유된다.

```javascript : 애옹이들 간식주기
// 배열 등의 참조타입 프로퍼티는 모든 인스턴스 사이에서 공유된다.
kkong.__proto__.snack = ["각종트릿", "비타스틱", "쉬바"]; // 꽁이에게 간식을 세개 주었다.
console.log(kkong.snack); // ["각종트릿", "비타스틱", "쉬바"]
setTimeout(() => munji.snack.push("츄르")); // 먼지에게 추가로 츄르를 주었다.
console.log(munji.snack); // ["각종트릿", "비타스틱", "쉬바", "츄르"]
console.log(kkong.snack); // ["각종트릿", "비타스틱", "쉬바", "츄르"]
console.log(harin.snack); // ["각종트릿", "비타스틱", "쉬바", "츄르"]
// 간식창고 공유...
```

## 클래스

프로토 타입을 이용해 새롭게 생성자 함수를 작성하는 방법이라고 생각하면 된다.

```javascript : 아까 만들었던 생성자 함수 Cat 을 클래스로 만들어보자.
class Cat {
  // constructor 를 작동시키기 위해 new 를 사용해야 한다. 다시말해 new로만 생성자를 호출할 수 있음.
  constructor(name, age, color) {
    this.name = name;
    this.age = age;
    this.color = color;
  }

  getIntroduce() {
    return (
      this.name +
      " is " +
      this.age +
      " ages old, and have a(n)" +
      this.color +
      "hair."
    );
  }
}
```

- 클래스의 메소드는 enumerable<sup>[2](#enumerable)</sup> 하지 않다. 프로토타입에 정의된 모든 메소드에 대해 이 플래그를 false 로 설정함.
- constructor 를 클래스에 명시하지 않으면 기본값으로 빈 constructor 이 추가된다.
- 클래스 내부에서는 항상 `strict` 모드를 사용함으로써 오타, 문법적인 에러가 없는 코드를 작성하는 데 도움을 준다.
- 클래스의 선언은 `hoisted` 호이스팅 되지 않는다.

```javascript
const munji = new Cat("munji", 3, "gray and white");
// 'Cat' was used before it was defined. (no-use-before-define)

class Cat {
  ...
}
```

- 프로퍼티 값으로 오브젝트 리터럴, 생성자 함수를 받지 않는다. 함수나 getters/setters 만 가질 수 있음. property:value 할당은 직접 할 수 없다.

```javascript
class Cat {
  let kitty = 2; // Error
}
```

### 클래스의 특징

- 생성자

  - 클래스 자체를 표현하는 함수를 정의한다.
  - `new` 키워드로 호출.
  - 하나의 생성자만 가질 수 있다.
  - 생성자는 클래스를 확장해서 사용할 때 `super` 키워드를 사용할 수 있다.

    ```javascript
    class Domestic extends Cat {
      constructor(name, age, color, tail) {
        super(name, age, color);
        this.tail = tail;
      }
    }
    console.log(new Domestic("Yaong", "2", "cheese tabby", "question shape"));
    ```

- 정적(static) 메소드

  - 프로토 타입이 아닌 클레스 자체에 존재하는 메소드
  - `static` 키워드를 이용해 선언된다.
  - 대부분 공용 함수(utility functions)를 만들기 위해 선언됨.
  - 클래스의 인스턴스를 만들지 않고 호출된다. 클래스 자체에서 호출되기 때문에 클래스의 인스턴스에서는 호출알 수 없다.

- Getters/Setters

  - 프로퍼티의 값을 가져오거나 설정하기 위해 Getters/Setters 를 가질 수 있다.

  ```javascript
  class Domestic extends Cat {
    constructor(name, age, color, tail) {
      super(name, age, color);
      this.tail = tail;
    }
    // 사실 prototype 안쪽에서 기본적으로 정의되어있다.
    get name() {
      return this._name;
    }

    set name(val) {
      this._name = val;
    }
  }
  ```

- Subclassing

  - 자바의 클래스가 상속을 하는 것과 비슷.
  - 자식 클래스를 만들 때 `extends` 키워드를 사용한다.<sub>클래스 Domestic 을 만들 때 사용함.</sub>
    - extends : 프로토타입 체인에 깊이를 더함. 프로토타입안에 또다른 프로토타입을 추가한다는 말.

  ```javascript
  class Domestic extends Cat {
    constructor(name, age, color, tail) {
      super(name, age, color);
      this.tail = tail;
    }
    // 자식클래스의 메소드

    getIntroduce() {
      // super 키워드로 호출하는건 클래스 안에서만 가능한 것 같다.
      // dc.super.getIntroduce() 가 안되서 하는말임.
      return (
        super.getIntroduce() + "And tail is " + this.tail + " - Child class"
      );
    }
  }

  let dc = new Domestic("Yaong", "2", "cheese tabby", "question shape");
  console.log(dc.getIntroduce());
  // Yaong is 2 ages old, and have a(n)cheese tabbyhair.And tail is question shape - Child class
  ```

---

<strong id="dunder">dunder proto</strong> 변수 양 끝이 ** 으로 묶여있는 변수. 파이썬에서 유래. `**prototype\_\_` 은 생성자 함수의 프로퍼티를 가리킨다.
<strong id="enumerable">Enumerable</strong> 자바스크립트 오브젝트의 각 프로퍼티는 기본적으로 enumerable 플래그를 가진다. enumerable은 해당 프로퍼티에서 어떤 명령이 실행될 수 있는지의 유효성을 정의한다. 예를 들어 Object.keys() 를 실행했을 때 enumerable 플래그가 false 라면 해당 프로퍼티는 Object.keys()가 반환하는 값 안에 들어있지 않다.
