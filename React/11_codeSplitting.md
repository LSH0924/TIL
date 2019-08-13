# React에서 CodeSpliting하기

## 코드 스플리팅이란?
- 코드 분할
- webpack 에서 프로젝트를 번들링 할 때 파일 하나가 아니라 파일 여러 개로 분산시켜 결과물을 만들 수 있다.
- 이벤트 등의 평소에는 사용하지 않는 함수의 코드를 나눠 사용할 때마다 불러온다.
- 나중에 프로젝트를 업데이트 할 때, 업데이트 하는 파일의 크기를 최소화 할 수 있다.
- 업데이트 시킬 파일들만 업데이트 하기 때문에 브라우저 캐싱 효과를 오래 누릴 수 있고, 트래픽 절감 및 로딩 속도를 개선할 수 있다.
- webpack 설정 파일을 커스터마이징 해야 하기 때문에 프로젝트의 환경 설정 파일을 밖으로 꺼낸다.
  ```s
  $ yarn eject
  ```
- **webpack 설정에서 코드 스플리팅 : vendor 설정**
  - 작성한 코드가 아닌 서드파티 라이브러리를 따로 분리시킨다. 
  ```javascript
  entry: [
      isEnvDevelopment &&
        require.resolve('react-dev-utils/webpackHotDevClient'),
      paths.appIndexJs,
    ].filter(Boolean),

  // ▽객체였던 entry 항목을 변경
  entry: {
      app: [
        // 핫리로드
        isEnvDevelopment &&
          require.resolve('react-dev-utils/webpackHotDevClient'),
        // 프로젝트 엔트리. index.js
        paths.appIndexJs,
      ],
      vendor: [
        // 구형 웹브라우저에서도 ES6 전용 코드를 제대로 작동할 수 있게 해 주는 라이브러리
        require.resolve("./polyfils"),
        "react",
        "react-dom",
        "react-router-dom"
      ]
    }.filter(Boolean),
  ```
  - ~~CommonsChunkPlugin : vendor로 분리된 곳에 들어간 내용들이 app쪽에서 중복되지 않도록 해주는 라이브러리~~ webpack4부터 **splitChunks**로 대체됨.

## 청크를 이용해 비동기적으로 코드 불러오기
- chunk : 데이터의 덩어리
- vendor처리는 단순히 캐싱을 원활히 할 수 있게 하는 작업일 뿐, 페이지를 로딩할 때 모든 코드를 불러오는것은 같다.
- 청크를 이용하면 페이지를 로딩할 때 필요한 파일만 불러올 수 있고, 아직 불러오지 않은 청크 파일들은 나중에 필요할 때 비동기적으로 불러와 사용할 수 있다.
- 비동기적으로 파일을 불러오기 위해 `import`를 LifeCycleMethod 나 이벤트 핸들러 등 특정 함수 내부에서 작성한다. 
  - 청크 생성 컴포넌트 : Split.js
    ```javascript
    import React from "react";
    const Split = () => {
        return <div>청크 생성 컴포넌트</div>;
    }
    export default Split;
    ```
  - 청크 임포트 컴포넌트 : AsyncSplit.js
    ```javascript
    import React, {Component} from "react";

    class AsyncSplit extends Component {
        state = {
            Split : null
        }
        loadSplit = () => {
            // import() 는 Promise를 반환한다.
            // import() 를 사용하면 webpack은 청크를 생성해 저장한다.
            import("./Split").then(({default: Split}) => {
                this.setState({ Split });
            });
        };
        render(){
            const {Split} = this.state;
            // Split 이 존재하면 컴포넌트 Split을 랜더링, 아닐시 버튼을 표시한다.
            return Split ? <Split/> : <button onClick={this.loadSplit}>Split Loading</button>;
        }
    }
    export default AsyncSplit;
    ```

## 라우트에 코드 스플리팅 하기
- 비동기적으로 불러올 코드의 양이 많을 경우 청크를 생성할 때마다 코드의 중복이 일어나므로, 공통 함수를 만드는게 좋다.

- **프로덕션 서버에서의 라우트 코드스플리팅은 나중에 보충하기로 한다.**

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
