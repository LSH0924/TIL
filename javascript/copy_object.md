# Object 복사하기

1. 원시적인(naive) 방법

- 반복문을 사용해 원본 오브젝트의 각 프로퍼티를 복사
- 상속 이슈
  - 원본 오브젝트와 완전히 똑같은 object.prototype 를 얻을 수 없다.
  - 프로퍼티 기술자(descriptors)<sup>[1](#propertyDescriptors)</sup> 는 복사되지 않는다.
  - 부수속성 `writable` 은 false 값으로 설정되어있었으나 복제한 오브젝트에서는 true 가 된다.
  - 부수속성 `enumerable` 프로퍼티만 복사된다.
  - 원본 오브젝트의 프로퍼티 중 하나가 object 타입(참조형) 이라면 복사된 오브젝트와 원본 오브젝트가 같은 프로퍼티 오브젝트를 참조하게되어버린다.

2. 얕은(shallow) 복사하기

- 소스의 최상위 레벨 프로퍼티들을 어떠한 참조도 없이 복사.
- 프로퍼티의 값이 참조형일 경우 타겟 오브젝트를 가리키는 레퍼런스 값만 복사한다.
- **Object.assign()** : 하나 이상의 원본 오브젝트로부터 복사한 오브젝트로 모든 enumerable 한 프로퍼티 값을 복사한다. 복사본 오브젝트를 반환함.

  - 오브젝트의 메소드를 복사할 수 있다.

  ```javascript
  var obj = {
    name: "anonymous",
    param: "목이마르다",
    date: { weather: "blue" }
  };

  var copy = Object.assign({}, obj);

  console.log(copy);
  // Object {name: "anonymous", param: "목이마르다", date: {weather: "blue"}}
  // 참조가 없는 프로퍼티 변경하기 : 원본유지
  setTimeout(() => {
    copy.param = "Lorem";
    console.log(copy);
    // Object {name: "anonymous", param: "Lorem", date: {weather: "blue"}}
    console.log(obj);
    // Object {name: "anonymous", param: "목이마르다", date: {weather: "blue"}
  });
  // 참조값만 복사한 프로퍼티 변경하기 : 원본변경
  setTimeout(() => {
    copy.date.weather = "rain";
    console.log(copy);
    // Object {name: "anonymous", param: "Lorem", date: {weather: "rain"}}
    console.log(obj);
    // Object {name: "anonymous", param: "목이마르다", date: {weather: "rain"}
  });
  ```

  - ECMAScript6 에서 배열을 복사할 때는 스프레드(...) 연산자를 사용한다. 얕은 복사에만 해당.

3. 깊은 복사하기

- 복사본과 원본 오브젝트가 어떤 프로퍼티도 공유하지 않도록 복사.
  - `JSON.parse(JSON.stringify(object))`
    - 사용자 정의 오브젝트 메소드를 복사하는데는 쓸 수 없지만, 원본과는 관계없는 완전히 새로운 복사본 오브젝트를 만들어낸다.

---

<strong id="propertyDescriptors"> 프로퍼티 기술자</strong> 속성 기술자. 객체의 부수 속성을 나타낸다. 속성 기술자는 네 가지 속성을 가지고 있음. `value` 는 속성에 어떤 값이 저장되어 있는지를 나타내고, `writable` 은 변경할 수 있는 속성인지, `enumerable`은 열거할 수 있는 속성인지, `configuable` 속성을 변경하거나 삭제할 수 있는지를 나타낸다.
