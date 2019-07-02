# Props와 State

- 리액트 컴포넌트에서 다루는 데이터

## props

  - 부모 컴포넌트가 자식 컴포넌트에게 전달하는 값
  - 부모 컴포넌트의 JSX 내부에서 랜더링하는 자식 컴포넌트에 props 를 직접 설정해줄 수 있다.
  - 문자열 외의 값을 props 에 설정할 때는 `{}` 를 사용한다.
    + MyComponent.js 를 랜더링하는 부모 컴포넌트 **App.js**
      ```javascript
      import React from 'react';
      import './App.css';
      import MyComponent from "./MyComponent";

      function App() {
        return (
          <MyComponent name="이수현"/>
        );
      }

      export default App;
      ```
    + MyComponent.js
      ```javascript
      import React, {Component} from "react";

      class MyComponent extends Component {
          render () {
              return (
                  <div>
                      실습용 새 컴포넌트 만들기 - new!
                      <h1>부모 컴포넌트에서 이름 받아오기</h1>
                      <p>안녕! 내 이름은 {this.props.name} 이라구 해!</p>
                  </div>
              );
          }
      }

      export default MyComponent;
      ```

  - 자식 컴포넌트는 props를 받아와 그대로 사용만 할 수 있고, 자식 컴포넌트 내부에서 props를 직접 변경 할 수 없다.
### defaultProps

  - props를 빠트리거나, 특정 상황에서 props를 일부러 비워야할 때 사용한다.

#### 사용법 1. 클래스 외부에서 선언할 때는 `컴포넌트명.defaultProps`

  + App.js
      ```javascript
      import React from 'react';
      import './App.css';
      import MyComponent from "./MyComponent";

      function App() {
        return (
          <MyComponent/>
        );
      }
      export default App;
      ```
  + MyComponent.js
      ```javascript
      import React, {Component} from "react";

      class MyComponent extends Component {
          render () {
              return (
                  <div>
                      실습용 새 컴포넌트 만들기 - new!
                      <h1>부모 컴포넌트에서 이름 받아오기</h1>
                      <p>안녕! 내 이름은 {this.props.name} 이라구 해!</p>
                  </div>
              );
          }
      }
      MyComponent.defaultProps = {
          name : "이수현"
      };
      export default MyComponent;
      ```

#### 컴포넌트 클래스 내부에서 사용하기

  - 일반적인 ES6 문법에서는 작동하지 않고, ES6 stage-2 서부터 나오는 transform-class-properties 문법으로 사용할 수 있다.<sub>create-react-app 이 지원하고있음</sub>

    + MyComponent.js
    ```javascript
    import React, {Component} from "react";

    class MyComponent extends Component {

        static defaultProps = {
            name:"이수현"
        }

        render () {
            return (
                <div>
                    실습용 새 컴포넌트 만들기 - new!
                    <h1>부모 컴포넌트에서 이름 받아오기</h1>
                    <p>안녕! 내 이름은 {this.props.name} 이라구 해!</p>
                </div>
            )
        }
    }

    export default MyComponent;

    ```
  - 빌드 작업에서 바벨을 통해 ES5 코드로 변환할 때 같은 결과가 나오므로, 어떤 방법을 사용하든 괜찮다.

### props 검증하기
  - 컴포넌트의 필수 props 를 지정하거나 props의 타입을 지정할 때 propTypes 를 사용한다.
  
    + MyComponent.js
    
      ```javascript
      /* eslint-disable react/no-typos */
      import React, {Component} from "react";
      import PropTypes from "prop-types";

      class MyComponent extends Component {
          render () {
              return (
                  <div>
                      실습용 새 컴포넌트 만들기 - new!
                      <h1>부모 컴포넌트에서 이름 받아오기</h1>
                      <p>안녕! 내 이름은 {this.props.name} 이라구 해!</p>
                  </div>
              )
          }
      }

      // props의 타입 정하기
      MyComponent.PropTypes={
          name: PropTypes.string
      }

      MyComponent.defaultProps={
          name: "이수현"
      }

      export default MyComponent;
     ```

- string 타입으로 설정한 props 에 숫자를 넣으면, 랜더링은 잘 되지만 콘솔에 경고가 발생한다.

    ```console
    index.js:1375 Warning: Failed prop type: Invalid prop `name` of type `number` supplied to `MyComponent`, expected `string`.
      in MyComponent (at App.js:7)
      in App (at src/index.js:7)
    ```

- props 를 필수 요소로 지정하려면 propTypes를 지정한 뒤 `.isRequired` 를 붙여준다.

    + MyComponent.js
    
      ```javascript
      /* eslint-disable react/no-typos */
      import React, {Component} from "react";
      import PropTypes from "prop-types";

      class MyComponent extends Component {

          static defaultProps={
              name: "이수현"
          }

          static propTypes={
              name:PropTypes.string
              , catAge: PropTypes.number.isRequired
          }

          render () {
              return (
                  <div>
                      실습용 새 컴포넌트 만들기 - new!
                      <h1>부모 컴포넌트에서 이름 받아오기</h1>
                      <p>안녕! 내 이름은 {this.props.name} 이라구 해!</p>
                      <p>내 고양이는 {this.props.catAge}살이야.</p>
                  </div>
              )
          }
      }

      export default MyComponent;
      ```
### PropTypes로 지정할 수 있는 props의 타입

- array : 배열
- bool : 참, 거짓 논리값
- func : 함수
- number : 숫자
- object : 객체
- string : 문자열
- symbol : ES6의 심벌객체
- node : 숫자, 문자열, element 또는 배열 등 랜더링 할 수 있는 모든 것
- element : 리액트 요소
- instanceOf(MyClass지정) : 특정 클래스의 인스턴스
- oneOf(["댕댕", "애옹"]) : 주어진 배열 요소값 중 하나가 와야함
- oneOfType([React.PropTypes.string, React.PropTypes.number]) : 주어진 배열 안의 종류 중 하나가 와야 함
- arrayOf(React.PropTypes.String) : 주어진 종류로 구성된 배열
- objectOf(React.PropTypes.String) : 주어진 종류의 값을 가진 객체
- shape({name: React.PropTypes.string, age: React.PropTypes.number}) : 주어진 스키마를 가진 객체
- any : 아무 종류나 다

## state

  - 컴포넌트 내부에서 선언되는 값
  - 내부에서 state값을 변경할 수 있다.
  - `class fields` 문법을 사용해서 constructor를 사용한것보다 더 간단하게 state를 정의할 수 있다.

### 생성자를 사용해서 state 설정하기

  ```javascript
    // 생성자를 사용할 때 : 컴포넌트를 새로 만들 때 실행
    /* eslint-disable react/no-typos */
    import React, {Component} from "react";
    import PropTypes from "prop-types";

    class MyComponent extends Component {

        static defaultProps={
            name: "이수현"
            , catAge:9
        }

        static propTypes={
            name:PropTypes.string
            , catAge: PropTypes.number.isRequired
        }

        constructor(props){
            super(props);
            this.state={
                number:0
            }
        }

        render () {
            return (
                <div>
                    실습용 새 컴포넌트 만들기 - new!
                    <h1>부모 컴포넌트에서 이름 받아오기</h1>
                    <p>안녕! 내 이름은 {this.props.name} 이라구 해!</p>
                    <p>내 고양이는 {this.props.catAge}살이야.</p>
                    <p>하지만 지금은 일본에 살기때문에 실질적으로는 {this.state.number}마리야... 나만 고양이 없어...</p>
                </div>
            )
        }
    }

    export default MyComponent;
  ```

  - `class fields`문법과 constructor 를 같이 사용한 경우, `class fields`이 먼저 실행-설정 되고 constructor가 나중에 실행-설정된다.

### constructor 밖에서 state 설정하기
- transform-class-properties 문법을 이용해 constructor 바깥에서 state 를 static으로 선언할 수 있다.
  ```javascript
  /* eslint-disable react/no-typos */
  import React, {Component} from "react";
  import PropTypes from "prop-types";

  class MyComponent extends Component {

      static defaultProps={
          name: "이수현"
          , catAge:9
      }

      static propTypes={
          name:PropTypes.string
          , catAge: PropTypes.number.isRequired
      }

      state={
          number:0,
          TMI:["한국에 가고싶어하지!", "흔들의자에 앉아서 전철 소리 듣는걸 좋아해.", "체력이 거지야..."],
          clickCount:0
      }

      render () {
          // state 에 있는 값 할당하기
          const {TMI, clickCount} = this.state;
          // 클릭한 횟수만큼 필터링하고 표시하기
          const tmiList = TMI
          .filter((element, index) => index < clickCount)
          .map((tmi, index) => 
              (<p key={index}>{tmi}</p>)
          );

          return (
              <div>
                  실습용 새 컴포넌트 만들기 - new!
                  <h1>부모 컴포넌트에서 이름 받아오기</h1>
                  <p>안녕! 내 이름은 {this.props.name} 이라구 해!</p>
                  <p>내 고양이는 {this.props.catAge}살이야.</p>
                  <p>하지만 지금은 일본에 살기때문에 실질적으로는 {this.state.number}마리야... 나만 고양이 없어...</p>
                  {/* button에서 이벤트 직접 집어넣기 */}
                  <button onClick={()=>{
                          const count = clickCount < 3 ? clickCount + 1 : clickCount;
                          this.setState({clickCount:count});
                      }
                  }>TMI생성기 : 남은 개수 {3 - this.state.clickCount}</button>
                  {/* JSX 밖에 있는 자바스크립트 변수 사용하기 */}
                  {tmiList}
              </div>
          )
      }
  }

  export default MyComponent;

  ```

## this.setStatae()

- state의 값을 바꾸기 위해 무조건 거쳐가야 하는 함수
- 컴포넌트를 리랜더링 시키기 위한 트리거 역할
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