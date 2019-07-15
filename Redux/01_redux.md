# Redux

- flux 아키텍쳐<sup>[1](#flux)</sup> 구조를 가진 상태관리 라이브러리
- 가장 사용률이 높은 상태 관리 라이브러리. 각 컴포넌트들의 상태 관련 로직들을 다른 파일들로 분리시켜 더욱 효율적으로 관리할 수 있다. 또한, 컴포넌트끼리 상태를 공유하게 될 때 여러 컴포넌트를 거치지 않고도 손쉽게 상태값을 전달할 수 있다.
- 리덕스의 미들웨어라는 기능은 비동기작업, 로깅 등의 확장적인 작업들을 더욱 쉽게 할 수 있게 해주기도 한다.
- 리덕스는 리액트에 종속되지 않는다.

## Redux 사용시 이점

- 글로벌 상태관리가 쉽다.
- 체계적이고 편안한 상태관리를 할 수 있다.
- 앱이 지니고 있는 상태와 상태 변화 로직이 들어있는 **스토어** 를 통해 원하는 컴포넌트에 원하는 상태값과 함수를 직접 주입할 수 있다.

## 기본 개념 정리

- **액션(Action)** : 상태에 어떠한 변화가 필요할 때 액션을 발생시킨다. 액션은 하나의 객체이며, `type` 필드를 반드시 가지고 있어야 한다. 그 외 값은 임의로 지정해 넣을 수 있다.

  - type : 액션에 대한 이름. 대문자로 되어있는 고유한 문자열이어야 한다. 중복금지.

  ```javascript : Action 객체
  {
    type: "ADD_EXPENDITURE",
    data:{
      id : "sane4290",
      date : "20190507143652",
      price : 3000,
      category : "snack"
    }
  }
  ```

- **액션 생성 함수(Action Creator)** : 액션을 만드는 함수. 파라미터를 받아 지정한 액션 객체 형태로 만들어준다. 화살표 함수 사용가능.

  ```javascript : Action Creator 예시
  // 일반적인 함수 형태로 만들수도 있고
  function addExpenditure(data) {
    return {
      type: "ADD_EXPENDITURE",
      data
    };
  }

  // 화살표 함수 형태로도 만들 수 있음
  const addExpenditure = data => ({
    type: "ADD_EXPENDITURE",
    data
  });
  ```

- **리듀서(Reducer)** : 파라미터로 전달받은 현재 상태와 액션을 참고해 새로운 상태를 만들어 반환한다. 불변성을 유지하면서 데이터에 변화를 일으켜주어야 하기 때문에 spread(...) 연산자를 사용하면 편리하다.

  ```javascript : Reducer 예시
  function reducer(state, action) {
    // 상태 업데이트 로직
    return alteredState;
  }

  // 마찬가지로 화살표 함수로도 작성할 수 있다.
  // state 가 undefined 일 때 initialState를 사용할 수 있도록 기본값을 설정할 수 있다.
  const reducer = (state = initialState, action) => (  
    // 상태 업데이트 로직은 여기에
    return alteredState;
  );
  ```

- **스토어(store)**
  - 리덕스의 핵심
  - 하나의 애플리케이션 당 하나의 스토어를 만든다.
  - 현재 애플리케이션의 상태(state)들과 리듀서, 내장 함수들이 들어있다.
  - createStore 함수를 이용해 스토어를 만든다. 파라미터로는 리듀서 함수를 전달.

  - **디스패치(dispatch)** : 스토어의 내장함수. 액션을 전달하는 과정. 액션 객체를 파라미터로 받아 리듀서를 실행시킨다.
  - **구독(subscribe)** : 스토어 값이 필요한 컴포넌트가 스토어를 구독하는 것. 함수 형태의 값을 파라미터로 받는다. subscribe 함수에 특정 함수를 전달해주면 액션이 디스패치됐을 때마다 전달해준 함수가 호출된다. subscribe 를 호출하면 반환값으로 오는 구독해제용 unsubscribe 함수는 나중에 필요할때 사용한다.<sup>[2](#subscribe)</sup>

```javascript : redux 사용해보기 실습
import { createStore } from "redux";

const lightDiv = document.getElementsByClassName("light")[0];
const switchBtn = document.getElementById("switch-btn");

const counterHeadings = document.getElementsByTagName("h1")[0];
const plusBtn = document.getElementById("plus-btn");
const minBtn = document.getElementById("minus-btn");

// Action Type 설정
const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREMENT = "INCREMENT";
const DECREMENT = "DECREMENT";

// Action Creator 설정
const toggleSwitch = () => ({ type: TOGGLE_SWITCH });
const increment = diff => ({ type: INCREMENT, diff });
const decrement = () => ({ type: DECREMENT });

// 초깃값 설정

const initialState = {
  light: false,
  counter: 0
};

// 리듀서
function reducer(state = initialState, action) {
  switch (action.type) {
    case TOGGLE_SWITCH:
      // Object.assign을 사용할 수도 있다.
      // 인자로 받은 객체를 왼쪽으로 덮어쓰면서 원하는 상태를 가진 새 객체를 만들 수 있음
      // return Object.assign({}, state, {light : !state.light});
      return { ...state, light: !state.light }; // light 값 반전

    case INCREMENT:
      return { ...state, counter: state.counter + action.diff };

    case DECREMENT:
      const count = state.counter > 0 ? state.counter - 1 : state.counter; // 숫자가 0이면 마이너스가 되지 않음
      return { ...state, counter: count };

    default:
      return state; // 지원하지 않는 액션일 때는 현재 상태를 유지한다.
  }
}

// 스토어 만들기
// 첫 번째 파라미터로는 리듀서 함수가, 두 번째 파라미터로는 스토어의 기본값을 설정한다(옵션). 
// 두 번째 파라미터가 생략될 경우 리듀서의 초깃값을 스토어의 기본 값으로 사용한다.
const store = createStore(reducer);
```

[![Edit Vanilla JS Redux Boilerplate](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/lx527xpq47?fontsize=14)

## Redux 사용하기

### Render 함수 구현

변경된 상태에 따라 화면을 랜더링 할 랜더 함수를 구현한다.

```javascript : render 함수 구현
const render = () => {
  const state = store.getState(); // 현재 상태 가져오기
  const { light, counter } = state; // 비구조화 할당하기

  if (light) {
    lightDiv.style.background = "green";
    switchBtn.innerText = "레드라이트";
  } else {
    lightDiv.style.background = "red";
    switchBtn.innerText = "그린라이트";
  }
  counterHeadings.innerText = counter;
};

render();
```

### 구독(subscribe) 하기

스토어의 상태가 바뀔 때마다 render 함수를 호출해 주어야 하기 때문에 스토어를 구독해서 반복작업을 시킨다.

```javascript : subscribe 사용예시
const listner = () => console.log("업데이트!");
// react-redux의 connect 함수가 대신해줌.
// 리덕스 스토어의 state 가 바뀔 때마다 특정 함수를 실행시키는것.
// 구독을 취소해야할 때는 unsubscribe() 를 호출해준다.
const unsubscribe = store.subscribe(listner);
```

예시를 참고해 이 화면에서 스토어 구독하기

```javascript : subscribing
store.subscribe(render);
```

### 이벤트 + 액션 발생시키기

각 버튼의 이벤트 함수를 만든 뒤, dispatch 함수를 이용해 액션 객체를 생성시켜 스토어로 보낸다.

```javascript : 이벤트 설정. 액션 발생시키기
switchBtn.onclick = () => {
  store.dispatch(toggleSwitch());
};

plusBtn.onclick = () => {
  store.dispatch(increment(5));
};

minBtn.onclick = () => {
  store.dispatch(decrement());
};
```

---

<strong id="flux">flux architecture</strong> FaceBook 에서 클라이언트 사이드 웹 애플리케이이션을 만들기 위해 사용하는 애플리케이션 아키텍쳐. **단방향** 데이터 흐름을 활용해 뷰 컴포넌트를 구성하는 React 를 보완한다. 프레임워크보다는 패턴처럼 사용. Dispatcher, Stores, Views(React 컴포넌트) 로 구성되어있고, 모든 변화는 action 객체를 통해 주고받는다.

<strong id="subscribe">subscribe</strong> 리액트 프로젝트에서는 리덕스를 쉽게 사용하기 위해 react-redux 라는 라이브러리를 사용하는데, 이 라이브러리가 자동으로 구독설정도 해주므로 리액트 프로젝트 외의 외부 라이브러리에 리덕스를 연동하는 등 특수한 경우 외에는 직접 구독시킬 일이 없다.

---

## 참고

velog - Redux (1) 소개 및 개념정리 - 작성자 : @velopert [https://velog.io/@velopert/redux-1-소개-및-개념정리-zxjlta8ywt](https://velog.io/@velopert/Redux-1-소개-및-개념정리-zxjlta8ywt)

velog - Redux (2) 리액트 없이 쓰는 리덕스 - 작성자 : @velopert [http://velog.io/@velopert/redux-2-리액트-없이-쓰는-리덕스-cijltabbd7](http://velog.io/@velopert/Redux-2-리액트-없이-쓰는-리덕스-cijltabbd7)

Flux 아키텍처 - Flux | 사용자 인터페이스를 만들기 위한 어플리케이션 아키텍쳐 [https://haruair.github.io/flux/docs/overview.html](https://haruair.github.io/flux/docs/overview.html)
