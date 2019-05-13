# React 에서 Redux 사용하기 : 예제

아래 예제를 기반으로 실습. 액션을 위한 파일과 리듀서를 위한 파일을 하나로 작성하는 Ducks Pattern 을 사용한다.

[![Edit colorful-counter](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/03vw6nq910)

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

- 리덕스 개발자 도구를 적용한다.... 부터는 집에서.

---

## 출처

velog - Redux (3) 리덕스를 리액트와 함께 사용하기 - 작성자 : @velopert [https://velog.io/@velopert/Redux-3-리덕스를-리액트와-함께-사용하기-nvjltahf5e](https://velog.io/@velopert/Redux-3-리덕스를-리액트와-함께-사용하기-nvjltahf5e)
