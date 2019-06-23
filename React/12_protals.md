# Portals 로 부모 컴포넌트의 외부 DOM 에 컴포넌트 렌더링하기

- Portals는 리액트 프로젝트에서 컴포넌트를 랜더링할 때 UI를 랜더링시킬 DOM 을 미리 선택해 부모컴포넌트의 바깥에서 랜더링할 수 있게 해준다.<sub>(DOM 의 계층구조 시스템에 종속되지 않는 컴포넌트 랜더링 가능)</sub>

- 굳이 부모 요소에 랜더링 하는게 아니어도 됨. 원하는 DOM 을 지정하고 해당 DOM의 위치에서 랜더링 시킬 수 있다.

- 불필요한 스타일을 상속받는 문제를 해결할 수 있고, 엘리먼트의 레이어 위치를 관리하는 `z-index` 스타일을 관리할 때 유용하게 쑬 수 있다.

- 리액트 v16에서 도입. 이전까지는 컴포넌트를 랜더링할 때 자식컴포넌트는 무조건 부모컴포넌트 안쪽에 있어야 했다.

* 실습 : 모달 만들기

1. 리액트 프로젝트를 새로 생성해 그 안의 index.html 에 모달용 DOM 을 하나 만든다.

```html
...
  <body>
    <div id="root"></div>
    <div id="modal"></div>
  </body>
</html>
```

2. 모달 랜더링용 컴포넌트 만들기 : src/ModalPortals.js

```javascript
import ReactDOM from "react-dom";

/**
 * 자식객체를 받아 지정한 DOM 에 랜더링한다
 * @param {Object} param0 자식객체
 */
const ModalPortal = ({ children }) => {
  // 자식객체를 랜더링할 DOM
  const element = document.getElementById("modal");
  return ReactDOM.createPortal(children, element);
};

export default ModalPortal;
```

3. 새 모달 만들기 : src/MyModal.js

```javascript
import React from "react";
import "./MyModal.css";

const MyModal = () => {
  return (
    <div className="MyModal">
      <div className="content">
        <h2>모달입니다.</h2>
        <p>리액트에서 모달만들기</p>
        <button>닫기</button>
      </div>
    </div>
  );
};

export default MyModal;
```

```css
.MyModal {
  background: rgba(0, 0, 0, 0.25);
  position: fixed;
  left: 0;
  top: 0;
  height: 100%;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.MyModal .content {
  background: white;
  padding: 1rem;
  width: 400px;
  height: auto;
}
```

4. ModalPortal 을 이용해 MyModal 을 한번 감싸주기

- ModalPortal 에서 지정한 DOM 안에 컴포넌트가 랜더링됨

```javascript
import React, { Component } from "react";
import MyModal from "./MyModal";
import ModalPortals from "./ModalPortals";

import "./App.css";

class App extends Component {
  render() {
    return (
      <div className="App">
        <h1>안녕하세요 리액트!</h1>
        <ModalPortals>
          {/*ModalPortals 컴포넌트로 모달 컴포넌트 한번 감싸주기*/}
          <MyModal />
        </ModalPortals>
      </div>
    );
  }
}

export default App;
```

5. 모달 닫기기능 구현하기 : App.js

```javascript
import React, { Component } from "react";
import MyModal from "./MyModal";
import ModalPortals from "./ModalPortals";

import "./App.css";

class App extends Component {
  // 모달을 표시할지 말지를 관리하는 상태를 하나 만들어준다.
  state = { modal: false };

  // 버튼을 누를때마다 끄고 켜는 동작 설정
  handleModalOpenClose = () => {
    const { modal } = this.state;
    this.setState({
      modal: !modal
    });
  };

  render() {
    const { modal } = this.state;
    return (
      <div className="App">
        <h1>안녕하세요 리액트!</h1>
        {/*모달 열기용 버튼을 새로 만들고 이벤트 적용시켜주기*/}
        <button onClick={this.handleModalOpenClose}>OpenModal</button>
        {/*state의 modal이 true일 때만 컴포넌트 표시*/}
        {modal && (
          <ModalPortals>
            {/*표시할 모달 컴포넌트에 이벤트 전달해주기*/}
            <MyModal onClick={this.handleModalOpenClose} />
          </ModalPortals>
        )}
      </div>
    );
  }
}

export default App;
```

- MyModal.js 의 닫기 버튼에 이벤트 넣어주기

```javascript
import React from "react";
import "./MyModal.css";

// 이벤트 받아오기
const MyModal = ({ onClick }) => {
  return (
    <div className="MyModal">
      <div className="content">
        <h2>모달입니다.</h2>
        <p>리액트에서 모달만들기</p>
        {/*받아온 이벤트를 닫기 버튼에 적용해준다.*/}
        <button onClick={onClick}>닫기</button>
      </div>
    </div>
  );
};

export default MyModal;
```

---

## 참고

https://velog.io/@velopert/react-portals
