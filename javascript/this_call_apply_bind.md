## this

- 함수가 동작하는 곳에 있는 오브젝트와 연결해주는 키워드.
- `this` 키워드의 값은 함수 자체와는 상관 없이, 불려지는 시점에 결정된다.
- 기본적으로 `this` 키워드는 전역 스코프의 root 를르 참조하는 window Object 이다.
- strict 모드에서는 기본적으로 undefined

### 묵시적 바인딩(implicit binding)

- 함수를 어디서 호출하느냐에 따라 `this` 가 참조하는 대상이 달라진다.

```javascript
var method01 = () => console.log(this);
var obj = {
  myMethod: method01
};

obj.myMethod(); // this == obj
method01(); // this == window object
```

### 명시적 바인딩(Explicit binding)

- 함수에 명시적으로 컨텍스트를 바인딩한다.
- `call()` 이나 `apply()` 를 이용한다.
- 명시적 바인딩은 묵시적 바인딩보다 우위를 가진다.

```javascript
var method01 = function() {
  console.log(this);
};
var obj1 = {
  name: "obj1",
  myMethod: method01
};
var obj2 = {
  name: "obj2",
  myMethod: method01
};

obj1.myMethod(); // obj1
obj2.myMethod(); // obj2

obj1.myMethod.call(obj2); // obj2
obj2.myMethod.apply(obj1); //obj1
```

#### `call()` 과 `apply()`

- `call()` 과 `apply()` 는 ECMAScript3 에서 소개되었다.
- `call()`, `Function.prototype.call()` 의 사용

  - `call()` 의 첫번째 파라미터는 함수가 호출되는 순간 `this` 가 된다. 아래 예제의 경우는 `sweetty` 오브젝트가 주어짐
  - 첫번째 파라미터 외의 다른 것들은 실행되는 함수의 인자들이다.

  ```javascript
  var sweetty = { name: "Kkong" };

  var introduce = function(age, coat, type) {
    return (
      "Her name is " +
      this.name + // sweetty.name 과 같음
      ". Her age is " +
      age +
      " ages old. And her is " +
      coat +
      " has a " +
      type +
      "."
    );
  };
  console.log(
    introduce.call(sweetty, 9, "domestic short hair cat", "three colors tabby")
  );
  // 결과
  // Her name is Kkong. Her age is 9 ages old. And her is domestic short hair cat has a three colors tabby.
  ```

- `apply()`, `Function.prototype.apply()` 의 사용

  - `call()` 과 마찬가지로 첫번째 파라미터 값을 `this` 에 세팅한다.
  - 두 번째 파라미터에서는 실행하는 함수의 인자를 배열로 받는다.

  ```javascript : 위의 예제에 이어서
  console.log(
    introduce.apply(sweetty, [
      9,
      "domestic short hair cat",
      "three colors tabby"
    ])
  );
  ```

### 하드 바인딩(Hard binding)

- ES5 에서 제공하는 `bind()` 메소드를 사용한다. `bind()` 는 사용자가 지정한 `this` 컨텍스트를 가진 기존 함수를 불러오기 위해 새 함수를 하드코딩 한 뒤 return한다.
- 하드 바인딩은 명시적 바인딩보다 우위를 가진다.

```javascript
var method01 = function() {
  console.log(this);
};
var obj1 = {
  name: "obj1"
};
var obj2 = {
  name: "obj2"
};

method01 = method01.bind(obj2); // 여기서 하드바인딩
method01.call(obj1); // 하드바인딩은 명시적 바인딩보다 우선시 된다.
```

#### `bind()`, `Function.prototype.bind()` 의 사용

- `bind()` 역시 첫번째 파라미터를 `this` 로 세팅한다.
- bound 함수가 `new` 연산자로 생성됐을 때, 처음에 바인딩한 `this` 값은 무시되고 새로운 인스턴스를 `this` 로 한다.

```javascript : call() 과 apply() 를 설명할 때 썼던 예제 재활용
var sweetty = { name: "Kkong" };

var introduce = function(age, coat, type) {
  return (
    "Her name is " +
    this.name +
    ". Her age is " +
    age +
    " ages old. And her is " +
    coat +
    " has a " +
    type +
    "."
  );
};

var newIntroduce = introduce.bind(sweetty);
console.log(newIntroduce); // sweetty 객체와 바인딩 된 함수 출력
console.log(newIntroduce); // bound introduce
console.log(newIntroduce(9, "domestic short hair cat", "three colors tabby"));
// Her name is Kkong. Her age is 9 ages old. And her is domestic short hair cat has a three colors tabby.
```

### 'New' 바인딩(New binding)

- 함수가 new 와 함께 호출되었을 때는 묵시적, 명시적, 하드 바인딩을 무시하고 새롭게 생긴 인스턴스의 컨텍스트를 만들어낸다.

```javascript
function something(parameter) {
  this.parameter = parameter;
}

var obj3 = {};

var bar = something.bind(obj3); // something 함수를 obj3 에 하드바인딩.
bar(2);
console.log(obj3.parameter); // bar의 this는 obj3을 가리킨다.
var foo = new bar(10); // new키워드로 bar 함수 호출. 새로운 bar의 인스턴스를 생성한다.
console.log(obj3.parameter); // obj3 과 하드바인딩 한 bar 은 그대로
console.log(foo.parameter); // new키워드로 새 인스턴스를 만든 bar은 자기 자신을 가리킨다.

// 결과
// 2
// 2
// 10
```

### API 호출

- ajax, eventHandling 등의 라이브러리, 헬퍼 오브젝트들의 콜백 사용과 this 의 동작에 주의해야 한다.
- this 가 참조하는 스코프를 확정하기 위해 함수의 파라미터로 원하는 스코프를 전달할 수 있다.

```javascript
var myObj = {
  method: function() {
    helperObj.doSomething("order", this.onSomething, this);
    // 함수를 호출하면서 세번째 파라미터에 this 도 넣어주었기 때문에 콜백으로 넘긴 this.onSomething 이 참조하는 스코프가 myObj 라는 것을 알 수 있음.
  },

  onSomething: function() {
    // 함수 동작
    console.log("얘가 실행됨");
  }
};
```

- 원하는 스코프를 하드바인드 해서 넣어줄 수도 있다.

```javascript
var myObj = {
  method: function() {
    helperObj.doSomething("order", this.onSomething.bind(this));
    // 하드바인드 하면 지정한 스코프에 바인딩 된 새로운 함수를 반환하기 때문에 myObj 에 스코프가 고정된 뉴함수가 넘어간다.
  },

  onSomething: function() {
    // 함수 동작
    console.log("얘가 실행됨");
  }
};
```

- 비추천 : 알아만 두기 : 클로저를 만들고 tihs 를 변수에 캐시하기

```javascript
var myObj = {
  method: function() {
    var me = this;
    helperObj.doSomething("order", function() {
      // 동작동작
      // this 가 누구인지는 모르겠고, 일단 me 에는 액세스 할 수 있음.
      // 진짜 스코프가 누군지는 모른 채 변수에만 의존하게 되기 때문에 비추천 1 드려요
      // 메모리 누수를 초래할 수 있다....?? 라는데 왜인지는 잘 모르겠음... 나중에 찾아본다.
      // 스코프가 엉망이 될 수 있다.
    });
  }
};
```
