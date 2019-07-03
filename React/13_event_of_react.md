# 리액트의 이벤트

## 리액트 이벤트 함수 설정

  ```html
  <button onClick={이벤트 함수명} />
  ```

### 규칙
  - 이벤트 속성의 이름을 정할 때, 이름은 카멜케이스(Camel case)로 해주어야 한다.<sub>onClick, onMouseDown 등</sub>
  - 이벤트에 전달되는 값은 **함수**여야한다.
    - 만약 함수가 아닌 메소드를 넣어버리면 랜더링 될 때마다 메소드를 실행하기 때문에, 서로를 무한으로 호출하게 되어버리는 상황이 발생할 수 있다.
  - DOM 요소에만 이벤트를 설정할 수 있다. 직접 만든 컴포넌트에는 자체적으로 이벤트를 설정할 수 없다. 컴포넌트들은 모든 요소를 props 로 받기 때문에 onClick 이벤트를 설정한다고 해도, 실제 클릭하면 onClick 안에 있는 함수가 props 로 넘어갈 뿐이다.<sub>전달받은 props를 이용해 컴포넌트 내부 DOM 이벤트로 설정할 수는 있다.</sub>

## 리액트에서 지원하는 이벤트 종류
- Clipboard
- Composition 
- Keyboard
- Focus
- Form
- Mouse
- Selection
- Touch
- UI
- Wheel
- Media
- Image
- Animation
- Transition
- 필요할 때 여기서 찾아보기 : https://reactjs.org/docs/events.html#clipboard-events

## 미리 만들어둔 이벤트 메서드를 적용하기

- 임의의 이벤트 메서드를 미리 만들어두고 DOM에 이벤트를 적용할 때는 반드시 생성자(constructor)에서 각각의 메서드를 this와 바인딩 해주어야 한다. 바인딩 작업을 해주지 않으면 호출한 `this.이벤트 메소드 이름` 은 `undefined` 를 반환한다.

```javascript
import React, { Component } from 'react';

class EventPractice extends Component {

    state={
        message: ""
    }

    constructor(props){
        super(props);
        // 임의로 만든 메서드들을 각각 this 에 바인딩
        this.handleInputChange = this.handleInputChange.bind(this);
        this.handleButtonClick = this.handleButtonClick.bind(this);
    }

    // input의 onChange 이벤트
    handleInputChange(event){
        this.setState({
            message:event.target.value
        });
    }

    // button의 onClick 이벤트
    handleButtonClick(event){
        alert(this.state.message);
        this.setState({
            message: ""
        });
    }

    render() {
        const {message} = this.state;
        return (
            <div>
                <h1>이벤트 연습</h1>
                <h2>입력한 값 : {message}</h2>
                <input
                type="text"
                name="message"
                value={message}
                placeholder="onChange 이벤트 연습"
                onChange={this.handleInputChange}/>
                <button onClick={this.handleButtonClick}>비우기</button>
            </div>
        );
    }
}

export default EventPractice;
```

- transform-class-properties 문법을 이용해 함수 형태로 메서드를 정의할 수 있다.
```javascript
import React, { Component } from 'react';

class EventPractice extends Component {

    state={
        message: ""
    }

    // input의 onChange 이벤트
    handleInputChange = (event)=>{
        this.setState({
            message:event.target.value
        });
    }

    // button의 onClick 이벤트
    handleButtonClick = ()=>{
        alert(this.state.message);
        this.setState({
            message: ""
        });
    }

    render() {
        const {message} = this.state;
        return (
            <div>
                <h1>이벤트 연습</h1>
                <h2>입력한 값 : {message}</h2>
                <input
                type="text"
                name="message"
                value={message}
                placeholder="onChange 이벤트 연습"
                onChange={this.handleInputChange}/>
                <button onClick={this.handleButtonClick}>비우기</button>
            </div>
        );
    }
}

export default EventPractice;
```

