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


