# React에서 CodeSpliting하기

- 이벤트 등의 평소에는 사용하지 않는 함수의 코드를 나눠 사용할 때마다 불러온다.

- notify.js : 이벤트가 일어날 때만 쓰는 코드

## 예제 : 간단 코드 스플리팅

- notify.js

```javascript
function notify(name) {
  alert(name + "님 일어나실 시간이에여");
}

export default notify;
```

- index.js : 해당 코드를 사용할 때만 불러오기

```javascript
import React from "react";
import ReactDOM from "react-dom";

import "./styles.css";

function App() {
  const handleClick = name => {
    // 필요할 때 불러오기
    // notify를 import 한 뒤, default 로 export 한 함수를 notify 로 설정한다.... 인듯?
    import("./notify").then(({ default: notify }) => {
      notify(name);
    });
  };

  return (
    <div>
      <button onClick={() => handleClick("사네")}>Click me!</button>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

- import 키워드를 함수로 사용하면 Promise 객체를 반환한다.
- 표준은 아님.
- 웹팩에서 지원해주고 있는 함수라 따로 설정 없이 사용할 수 있다.
- 모듈을 비동기적 CommonJS 형태로 불러오기 때문에 따로 default 표시를 해 주어야 한다. 위 예제에서는 default 를 notify 로 부르겠다고 설정해주었다.

## 예제 : 컴포넌트를 코드스플리팅하기

- SplitMe.js : 코드 스플리팅 연습용 컴포넌트

```javascript
import React from "react";

const SplitMe = () => {
  return <div>필요할때만 나타나는 컴포넌트</div>;
};

export default SplitMe;
```

- index.js : 컴포넌트를 사용할 때만 가져오기

```javascript
// 클래스형 컴포넌트에서 불러올 때
class App extends Component {
  state = {
    SplitMe: null
  };

  handleClick = () => {
    import("./SplitMe").then(({ default: SplitMe }) => {
      this.setState({ SplitMe });
    });
  };

  render() {
    const { SplitMe } = this.state;
    return (
      <div>
        <button onClick={this.handleClick}>컴포넌트 부르기</button>
        {SplitMe && <SplitMe />}
      </div>
    );
  }
}

// 함수형 컴포넌트에서 불럴올 때

function App() {
  const [SplitMe, setSplitMe] = useState({ component: null });

  const handleClick = () => {
    console.log("handle");
    import("./SplitMe").then(({ default: SplitMe }) => {
      setSplitMe({ component: SplitMe }); //
    });
  };

  return (
    <div>
      <button onClick={handleClick}>컴포넌트 부르기</button>
      {SplitMe.component && <SplitMe.component />}
    </div>
  );
}
// 두 방법 중 하나를 쓰면 된다. 함수형 컴포넌트로 만드는 방법은 안 나와서 이것저것 해보느라 시간은 좀 걸렸지만 찾아냈으니 매우 뿌듯!
```

## HOC 를 사용해 더 쉽게 코드 스플리팅하기

- 스플리팅 해야 하는 컴포넌트들의 개수가 많아졌을 때, 같은 코드를 중복해서 작성해야하는 번거로움, 낭비를 줄이기 위해 HOC 를 사용할 수 있다.
- 보통 HOC 는 내보내기 직전에 사용하는데, 경우에 따라 불러온 다음 바로 사용할수도 있다.
- 불러올 컴포넌트의 state 에 스플리팅할 컴포넌트 자체를 담을 필요가 없다. 조건부로 랜더링 하기만 하면 자동으로 스플리팅 됨.
- HOC 함수 만들기 : src/withSplitting.js

```javascript
import React, { Component } from "react";

const withSplitting = getComponent => {
  // 인자로 받는 getComponent 는 () => import("./SplitMe") 의 형태인 함수가 전달되어야 한다.
  class WithSplitting extends Component {
    state = {
      SplitMe: null
    };

    constructor(props) {
      super(props);
      // 받아온 getComponent 를 constructor 에서 실행시킨다.
      getComponent().then(({ default: Splitted }) => {
        this.setState({
          Splitted
        });
      });
    }

    render() {
      const { Splitted } = this.state;
      if (!Splitted) return null;
      return <Splitted {...this.props} />;
    }
  }

  return WithSplitting;
};

export default withSplitting;
```

- HOC 적용하기 : index.js

```javascript
import React, { useState, Component } from "react";
import ReactDOM from "react-dom";
import withSpliting from "./withSplitting";

import "./styles.css";

// 코드 스플리팅 해야 할 컴포넌트는 미리 따로 만들어둔다. 만들어두고 랜더링만 하면 됨.

const SplitMe = withSpliting(() => import("./SplitMe"));

class App extends Component {
  state = {
    visible: false
  };

  handleClick = () => {
    const { visible } = this.state;
    this.setState({ visible: !visible });
  };

  render() {
    const { visible } = this.state;
    return (
      <div>
        <button onClick={this.handleClick}>컴포넌트 부르기</button>
        {visible && <SplitMe />}
      </div>
    );
  }
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

---
## 참고

https://velog.io/@velopert/react-code-splitting#3.-hoc-를-사용하여-더-쉽게-코드-스플리팅하기
