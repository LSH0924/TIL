# Props와 State

- 리액트 컴포넌트에서 다루는 데이터

- props

  - 부모 컴포넌트가 자식 컴포넌트에게 전달하는 값
  - 자식 컴포넌트는 props를 받아와 그대로 사용만 할 수 있고, 직접 변경은 할 수 없다.

- defaultProps

  - props를 빠트리거나, 특정 상황에서 props를 일부러 비워야할 때 사용한다.
  - `컴포넌트명.defaultProps` 로 사용할 수 있다.

- state

  - 컴포넌트 내부에서 선언되는 값
  - 내부에서 state값을 변경할 수 있다.
  - `class fields` 문법을 사용해서 constructor를 사용한것보다 더 간단하게 state를 정의할 수 있다.

    ```javascript
      // 생성자를 사용할 때
      constructor(props) {
        super(props);
        this.state = {
          name: "NAME"
        };
      }

      // class fields 문법을 사용해 정의할 때
      state = {
        name: "NAME"
      }

    ```

  - `class fields`문법과 constructor 를 같이 사용한 경우, `class fields`이 먼저 실행-설정 되고 constructor가 나중에 실행-설정된다.

## this.setStatae()

- state의 값을 바꾸기 위해 무조건 거쳐가야 하는 함수
- 이 함수가 호출되면 컴포넌트가 리랜더링 된다.
- 객체(Object)로 전달되는 값만 업데이트 한다.
- 객체를 깊게 탐색할 수는 없어서 state 안에 Object(1)가 있는 경우, Object(1)과 맵핑된 키에 this.setState()의 매개변수로 받은 Object(2)를 맵핑시켜버린다.

  ```javascript
  import React, { Component } from "react";

  class Test extends Component {
    state = {
      info1: { a: "", b: "" }, // Object(1)
      info2: { a: "", b: "" } // Object(1)
    };

    handleChange = e => {
      const { name, value } = e.target;
      this.setState(
        name === "info1"
          ? { [name]: { a: value } }
          : { [name]: { a: value, ...this.info } } // Object(2)
      );

      console.log(this.state);
    };

    render() {
      return (
        <React.Fragment>
          <input type="text" name="name" onChange={this.handleChange} />
          <input type="text" name="info" onChange={this.handleChange} />
          // 값이 입력될때마다 새로운 객체가 this.state.info 안에 들어간다.
        </React.Fragment>
      );
    }
  }

  export default Test;
  ```

- 자바스크립트의 전개 연산자를 사용하면 값 덮어쓰기를 막을 수 있다.
  - 전개연산자(...) : 기존 객체 안에 있는 내용을 명시된 위치에 풀어준다.
- Immutable.js이나 immer.js 등 리액트의 불변성원칙을 위한 라이브러리를 사용할 수도 있다.
- this.setStatae()에 함수를 넣어 사용할 수도 있다.
- 비구조 할당 문법을 이용해 함수를 넣어 사용하는 this.setStatae()를 더욱 간단하게 표기할 수 있다.

  - 비구조 할당(Destructurning assignment) : 배열 객체의 속성을 해체해서 값을 개별 변수에 담는 javascript식 표현

  ```javascript
  handleRegist = data => {
    const { todoInfo } = this.state; // 비구조 할당
    this.setState({
      todoInfo: todoInfo.concat({
        key: ++this.key,
        ...data // 전개연산자 사용
      })
    });
  };
  ```

## 리액트 이벤트 함수 설정

- 사용방법
  > <button onClick={이벤트 함수명} />
- 규칙
  - 이벤트 속성의 이름을 정할 때, 이름은 카멜케이스(Camel case)로 해주어야 한다.<sub>onClick, onMouseDown 등</sub>
  - 이벤트에 전달되는 값은 **함수**여야한다.
    - 만약 함수가 아닌 메소드를 넣어버리면 랜더링 될 때마다 메소드를 실행하기 때문에, 서로를 무한으로 호출하게 되어버리는 상황이 발생할 수 있다.
