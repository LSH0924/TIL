# App.js와 index.js 살펴보기

- 프로젝트를 부트스트래핑 하면 기본으로 생성되는 리액트 컴포넌트

## import

- JSX를 사용하기 위한 리액트 컴포넌트 불러오기

  ```javascript
  import React, { Component } from "react";
  ```

- import를 사용하면 나중에 프로젝트를 빌드할 때 webpack이 확장자에 따라 다른 작업을 해준다.
- .css : 프로젝트에서 사용한 모든 파일을 하나로 합쳐준다.
- .js : 모든 코드들이 제대로 로딩되게끔 순서를 설정하고 하나로 합쳐준다.
- 사전에 따로 설정되어있지 않은 확장자의 경우, 일반 파일로 불러온 다음 특정 경로에 사본을 생성하고 해당 사본의 경로를 텍스트로 받아온다.

## 컴포넌트를 만드는 두 가지 방법

### 1. **compoenet를 상속하는 클래스를 만든다.**

- 이 때, 이 클래스는 render() 함수를 반드시 가지고 있어야 하며, render()의 내부에서는 JSX를 리턴해주어야 한다.

### 2. **함수를 통해 컴포넌트를 만든다.**

```javascript
const myName = ({name}) => {
  return(
    //JSX
  );
}
```

- 클래스형 컴포넌트와의 차이
  - state, lifecycleAPI가 존재하지 않는다.
  - 컴포넌트의 초기 마운트가 클래스로 선언된 컴포넌트보다 조금 빠르다.
  - 메모리 자원 사용을 감소시킨다.

### Reactjs Code Snippet 을 이용해 초기 코드를 생성할 수 있다!
  - rcc 입력하고 자동완성!

## 만든 컴포넌트 내보내기

```javascript
export default App; // (<-클래스 이름)
```

- 작성한 컴포넌트를 다른 곳에서 불러와 사용할 수 있도록 export한다.
- export한 컴포넌트 사용하기

```javascript
import App from "./App"; // import 컴포넌트이름 from 컴포넌트 위치
```

## 가져온 컴포넌트 랜더링하기

```javascript
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
### ReactDOM.render()
- ReactDOM.render 의 첫번째 매개변수에는 JSX 형태로 작성한 컴포넌트를, 두번째 매개변수에는 컴포넌트를 랜더링할 Element를 지정한다.
- 컴포넌트를 페이지에 랜더링한다.

## 함수형 컴포넌트
- 클래스형 컴포넌트에서 라이프사이클, state 등 무거운것들을 제거했기 때문에 메모리 소모량이 적다.
- v16 이상에서는 함수형 컴포넌트의 성능이 클래스형 컴포넌트보다 빠르다.
- state를 사용하는 컴포넌트의 개수는 적으면 적을수록 적다. 때문에, 대부분 컴포넌트는 함수형으로 만들고, state나 라이프사이클 API 를 꼭 써야할대만 클래스형 컴포넌트를 사용하는게 좋다.

---
## 참고
리액트를 다루는 기술 - 김민준 저