# Functional Component

- 리액트에서 컴포넌트를 정의할 때는 보통  ES6에 도입된 `class` 를 사용한다.
- 컴포넌트에서 라이프사이클 API를 사용해야 하거나, state를 사용하는 경우 `class`를 사용. <sub>React.createClass(...)는 요즘 잘사용되지 않는다.</sub>
  
  ```javascript
  import React, {Component} from "react";
  class App extends Component{
    render(){
        return(
            <p>Class Component</p>
        );
    }
  }
  export default App;
  ```

- props만 전달해서 뷰를 랜더링하는 간단한 컴포넌트의 경우, 함수형 컴포넌트를 정의할 수 있다.
- 해당 컴포넌트 자체의 기능은 따로 없고, props에 의해 컴포넌트가 표시되는것을 명시하기 위해 사용
- Redux 를 사용하여 컴포넌트들을 구성 할 때, Container 컴포넌트 (혹은 Smart, 컴포넌트) 는 클래스형 컴포넌트를 사용하고, Presentational 컴포넌트 (혹은, Dumb 컴포넌트) 는 함수형 컴포넌트를 사용
- 함수형 컴포넌트를 사용할 때, 첫 마운팅 속도는 클래스 컴포넌트보다 7~11% 빠름

- 형태
  - 일반적인 함수형 컴포넌트

  ```javascript
  import React, {Component} from "react";
  function App(props){
    return (
        <p>Functional Component : {props.name}</p>
    );
  }
  export default App;
  ```

  - 화살표 함수 사용
  
  ```javascript
  import React, {Component} from "react";
  function App = (props) => {
      return (
          <p>Arrow Functional Component : {props.name}</p>
      );
  }
  export default App;
  ```

  - 비구조화 할당 (Object Destructuring) 문법 사용
   ```javascript
  import React, {Component} from "react";
  function App = ({name}) => {
      return (
          <p>Object Destructuring Functional Component : {name}</p>
      );
  }
  export default App;
  ```