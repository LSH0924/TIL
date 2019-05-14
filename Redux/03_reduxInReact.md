# React 에서 Redux 사용하기 : 예제

아래 예제를 기반으로 실습. 액션을 위한 파일과 리듀서를 위한 파일을 하나로 작성하는 Ducks Pattern 을 사용한다.

[![Edit colorful-counter](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/2v8kko225n)

## 리액트에서 리덕스를 사용하기 위해 필요한 라이브러리

- redux : 리덕스 모듈
- react-redux : 리액트 컴포넌트에서 리덕스를 사용하기 위한 도구
- redux-actions : 액션 객체, 리듀서 함수 등을 깔끔하게 만들어주는 라이브러리?

## counter모듈 만들기

- 모듈 : 특정 기능을 구현하기 위해 필요한 액션, 액션 생성 함수, 초기값, 리듀서 함수가 들어있는 파일

- 코드샌드박스의 src/store/modules/ 에 counter.js 파일을 생성한 뒤, 액션타입을 정의한다.

  ```javascript : 액션 타입 정의하기
  const CHANGE_COLOR = "counter/CHANGE_COLOR";
  const INCREMENT = "counter/INCREMENT";
  const DECREMENT = "counter/DECREMENT";
  ```

  - Ducks 패턴을 사용할 땐 다른 모듈에서 작성하게 될 수도 있는 액션들과의 충돌을 방지하기 위해 모듈의 이름을 붙여 액션 이름을 설정한다.

- 액션 생성함수와 초기값, 리듀서를 정의한다. 각 함수들의 앞에 **export** 를 붙여주는데, 이 수식자는 나중에 각 컴포넌트에 리덕스를 연동하고 불러와서 사용할 때 필요하다.

  ```javascript : 액션함수와 초기값, 리듀서
  // 액션함수
  export const changeColor = color => ({ type: CHANGE_COLOR, color });
  export const increment = diff => ({ type: INCREMENT, diff });
  export const decrement = () => ({ type: DECREMENT });

  // 초기값 설정하기
  const initialState = {
    color: "red",
    number: 0
  };

  // 리듀서 작성
  // export default : 단일 값을 내보내거나 모듈의 반환값을 사용하고자 할 때 명시한다.
  export default function counter(state = initialState, action) {
    switch (action.type) {
      case CHANGE_COLOR:
        return { ...state, color: action.color };
      case INCREMENT:
        return { ...state, count: state.count + action.diff };
      case DECREMENT:
        const count = state.count > 0 ? state.count - 1 : state.count;
        return { ...state, count: count };
      default:
        return state;
    }
  }
  ```

- 리듀서를 합쳐주고 스토어를 만든다.

  - 여러 개의 리듀서를 만들 때에는 redux 의 내장함수인 `combineReducers` 을 사용해 각각의 리듀서를 합쳐주어야 한다.
  - 여러 개로 나뉜 각각의 리듀서를 **서브 리듀서**, 하나로 합쳐진 리듀서를 **루트 리듀서** 라고 한다.
  - src/store/modules/ 경로에 index.js 파일을 작성한다.

    ```javascript
    import { combineReducers } from "redux";
    import counter from "./counter";

    export default combineReducers({
      // 각각의 리듀서들을 이 안에 넣어서 합쳐준다.
      counter
    });
    ```

  - src/index.js에 스토어도 만들어준다.

    ```javascript
    import React from "react";
    import ReactDOM from "react-dom";

    // createStore 와 rootReducer 불러오기
    import { createStore } from "redux";
    import rootReducer from "./store/modules";

    // 스타일과 컴포넌트
    import "./index.css";
    import App from "./App";
    import registerServiceWorker from "./registerServiceWorker";

    // 스토어 만들고 값 확인하기
    const store = createStore(rootReducer);
    console.log(store.getState());

    ReactDOM.render(<App />, document.getElementById("root"));
    registerServiceWorker();
    ```

  - 합친 루트 리듀서의 초기값은 각 리듀서에 설정된 초기값들의 모음이다.

    > 결과
    > ▶Object {counter: Object}
    > ▶counter: Object
    > color: "red"
    > number: 0

    ```javascript
    {
      counter: {
        color: 'red',
        number: 0,
      },
      // ... 다른 리듀서에서 사용하는 초깃값들도 들어감
    }
    ```
- Connect 를 이용해 컴포넌트와 스토어 연동

- 컴포넌트의 분류

  - 리덕스의 창시자 Dan Abramov 가 제시.
  - 컨테이너 컴포넌트 : 리덕스와 연동된 컴포넌트. smart 컴포넌트라고도 함. 유저 인터렉션에 집중할 수 있다.
  - 프리젠테이셔널 컴포넌트 : props 를 전달해주면 그대로 보여주는 컴포넌트. dumb 컴포넌트라고도 함. UI에만 집중할 수 있다.

- 컨테이너 컴포넌트 만들기 : PaletteContainer.js

```javascript
import React, { Component } from "react";
import { connect } from "react-redux";
import Palette from "../components/Palette";
import { changeColor } from "../store/modules/counter";

// 클래스 형식의 팔레트 컨테이너를 만든다.
class PaletteContainer extends Component {
  // 팔레트 선택시 이벤트
  handleSelect = color => {
    const { changeColor } = this.props;
    console.log("what", color);
    changeColor(color);
  };

  render() {
    const { color } = this.props;
    return <Palette selected={color} onClick={this.handleSelect} />;
  }
}

//props 에 들어갈 스토어 상태값 : 색
const stateToProps = state => ({
  color: state.counter.color
});

//props 에 넣어줄 액션 함수
const dispatchToProps = dispatch => ({
  changeColor: color => dispatch(changeColor(color))
});

// connect 가 반환한 함수에 PaletteContainer 를 넣어 호출한다.
export default connect(
  stateToProps,
  dispatchToProps
)(PaletteContainer);
```

- 컨테이너 컴포넌트를 만들 때, react-redux의 connect를 불러와 사용한다. 첫번째 파라미터는 스토어 안에 들어는 값을, 두번째 파라미터는 액션 생성 함수들을 props 로 전달해준다.
- connect 함수를 호출하면 특정 컴포넌트에 설정된 props 를 전달해준다.
- dispatch 함수를 일일히 적용하지 않고 한번에 쓰는 방법도 있다. : CounterContainer.js

```javascript
import { bindActionCreators } from "redux";
...
...
// dispatch 만들기 방법2 : bindActionCreators를 이용해 한꺼번에 만들기
const dispatchToProps = dispatch =>
  bindActionCreators({ increment, decrement }, dispatch);
```

- 액션생성함수 자체로만 이루어진 객체를 바로 넣어주어도 redux 가 알아서 bindActionCreators 를 이용해 묶어준다.

```javascript
// 자동 bind 됨.
const mapDispatchToProps = { increment, decrement };
```

### redux-actions 라이브러리를 이용해 대기자 명단 구현하기

#### 액션타입, 액션생성함수 정의하기 : src/store/modules/waiting.js

- FSA(flux-standard-action)
  - 읽기 쉽고 유용하고 간단한 액션 객체를 만들기 위해 사용
  - 필수조건 : 순수 자바스크립트 객체이며 type 값을 가지고 있어야 한다.
  - 선택조건 : error, payload<sup>[1](#payload)</sup>, meta 값을 가지고있을 것

```javascript
const CHANGE_INPUT = "waiting/CHANGE_INPUT";
const CREATE = "waiting/CREATE";
const ENTER = "waiting/ENTER";
const LEAVE = "waiting/LEAVE";

//redux-actions 의 createAction 을 사용하지 않았을 때
const changeInput = text => ({ type: CHANGE_INPUT, payload: text });
const create = text => ({ type: CREATE, payload: text });
const enter = id => ({ type: CREATE, payload: id });
const leave = id => ({ type: CREATE, payload: id });

//redux-actions 의 createAction 을 사용할 때
const changeInput = createAction(CHANGE_INPUT, text => text);
const create = createAction(CREATE, text => text);
const enter = createAction(ENTER, id => id);
const leave = createAction(LEAVE, id => id);
```

- createAction 은 첫번째 파라미터로 액션의 타입을 받고, 두번째 파라미터로는 payloadCreator 를 받는다.
- reducer는 순수 함수여야 하기 때문에 액션객체가 dispatch 되기 전에 데이터가 완성되어있어야 한다. : 액션 함수 수정

```javascript
const changeInput = createAction(CHANGE_INPUT, text => text);
// 대기 명단을 하나 추가할 때마다 아이디도 하나씩 늘어남
const create = createAction(CREATE, text => ({ text, id: id++ }));
const enter = createAction(ENTER, id => id);
const leave = createAction(LEAVE, id => id);
```

#### 초기 상태 및 리듀서 정의하기

```javascript
// 초기상태 정의하기
const initialState = {
  list: [
    {
      id: 1,
      name: "대기자1",
      entered: false
    }
  ],
  input: ""
};

//redux-actions 에서 제공하는 리듀서용 함수
export default handleActions({
  [CHANGE_INPUT]: (state, action) => ({
    ...state,
    input: action.payload.text
  }),

  [CREATE]: (state, action) => ({
    ...state,
    list: state.list.concat({
      id: action.payload.id,
      name: action.payload.text,
      entered: false
    }),
    input: ""
  }),

  [ENTER]: (state, action) => ({
    ...state,
    list: state.list.map(item =>
      item.id === action.payload.id ? { ...item, entered: !item.entered } : item
    )
  }),

  [LEAVE]: (state, action) => ({
    ...state,
    list: state.list.filter(item => item.id !== action.payload.id)
  }),
  initialState
});
```

- handleAcrions 를 사용하면 리듀서에 switch/case 문을 사용할 필요 없이 각 액션 타입의 업데이터 함수만 구현하면 되기 때문에 편하다.

---

<strong id = "payload">payload</strong> 액션에서 사용할 파라미터의 필드명.


---

## 출처

velog - Redux (3) 리덕스를 리액트와 함께 사용하기 - 작성자 : @velopert [https://velog.io/@velopert/Redux-3-리덕스를-리액트와-함께-사용하기-nvjltahf5e](https://velog.io/@velopert/Redux-3-리덕스를-리액트와-함께-사용하기-nvjltahf5e)
