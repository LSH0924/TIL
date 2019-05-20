# React Hooks 01

- react v16.8부터 새롭게 도입됨
- 함수형 컴포넌트에서도 상태관리를 할 수 있는 useState, 랜더링 직후의 작업을 설정하는 useEffect 등의 기능 제공

## useState

가장 기본적인 Hook. 함수형 컴포넌트에서도 가변적인 상태를 유지할 수 있게 해준다. 함수형 컴포넌트에서 상태를 관리해야할 때 유용하다. 상태 관리를 위해 굳이 클래스 형태로 변환할 필요가 없어 매우 편리하다.

- 예제 : 카운터 구현해보기

```javascript : src/Counter.js
import React, { useState } from "react";

// 함수형 카운터 컴포넌트 생성
const Counter = () => {
  const [value, setValue] = useState(0);
  // counter 의 value 를 0으로 설정함.
  // useState 함수를 호출하면 배열을 반환한다.
  // 반환받은 배열의 첫번째 요소는 파라미터로 받은 value이고, 두번째 요소는 value의 상태를 설정하는 함수이다.

  return (
    <div>
      <p>
        Count : <strong>{value}</strong>
      </p>
      <button onClick={() => setValue(value - 1)}>-</button>
      <button onClick={() => setValue(value + 1)}>+</button>
    </div>
  );
};

export default Counter; // 만든 컴포넌트 내보내주기
```

- 예제 : useState 여러 번 사용하기

하나의 useState 함수는 하나의 상태값만 관리할 수 있기 때문에, 여러 개의 상태를 관리하려면 여러 개를 사용해야한다.

```javascript : src/Info.js
import React, { useState } from "react";

const ChangeInfo = () => {
  // 두 개의 상태는 각각 독립적으로 움직인다.
  const [name, setName] = useState("");
  const [phone, setPhone] = useState("");

  const onNameChange = e => {
    setName(e.target.value);
  };

  const onPhoneChange = e => {
    setPhone(e.target.value);
  };

  return (
    <div>
      <input value={name} onChange={onNameChange} />
      <input value={phone} onChange={onPhoneChange} />
      <p>이름 : {name}</p>
      <p>전화번호 : {phone}</p>
    </div>
  );
};
export default ChangeInfo;
```

## useEffect

리액트 컴포넌트가 랜더링 될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook. 클래스형 컴포넌트에서 쓰는 componentDidMount() 와 componentDidUpdate() 를 합친 형태. 두번째 파라미터 배열에 무엇을 넣느냐에 따라 실행되는 조건이 달라진다.

- 아까 만든 Info.js 에 useEffect 적용하기

  ```javascript
  // useEffect 함수에 콜백함수를 집어넣는다.
  useEffect(() => {
    console.log("랜더링 완료!");
    console.log({ name, phone });
  });
  ```

  - 재 랜더링 될 때마다 useEffect() 안에 넣은 함수가 실행된다.
  - useEffect()의 두번째 파라미터에 빈 배열을 넣으면 컴포넌트가 화면에 가장 **처음 랜더링 될 때만 실행**되고 업데이트 할때는 실해오디지 않는다.
  - **특정 값이 업데이트 될 때**만 useEffect() 안의 함수가 실행되게 만들고싶으면 두번째 파라미터의 배열에 체크하고싶은 값을 넣어준다. 이 때, 배열 안에 넣는 값은 useState 를 통해 관리하고 있는 값이어도 되고, props 로 받은 값이어도 된다.

  ```javascript : 전화번호가 바뀐것만 체크하고싶을 때
  useEffect(() => {
    console.log("랜더링 완료!");
    console.log({ name, phone });
  }, [phone]);
  ```

  - 컴포넌트가 언마운트 되기 전, 업데이트 되기 직전에 작업을 수행하고싶으면 useEffect() 에서 뒷정리(cleanup) 함수를 return 해주어야 한다.

  ```javascript : Info.js 의 useEffect()
  useEffect(() => {
    console.log("랜더링 완료!");
    console.log({ name, phone });
    return () => {
      console.log("Clean Up!");
      console.log({ name, phone });
    };
  });
  ```

  ```javascript : index.js
  import React, { useState } from "react";
  import ReactDOM from "react-dom";
  import Counter from "./Counter";
  import Info from "./Info";

  import "./styles.css";

  const rootElement = document.getElementById("root");

  const App = () => {
    const [visible, setVisible] = useState(true);
    const buttonClick = () => setVisible(!visible);
    return (
      <React.Fragment>
        <Counter />
        <hr />
        <button onClick={buttonClick}>
          {visible ? "Info 숨기기" : "Info 보이기"}
        </button>
        {visible && <Info />}
      </React.Fragment>
    );
  };

  ReactDOM.render(<App />, rootElement);
  ```

  - 랜더링되기 직전에 cleanup 함수가 실행된다. cleanup 함수가 호출되는 시점은 값을 업데이트 하지 않았을 때이다.
  - 언마운트 될 때만 뒷정리 함수를 호출하고싶다면 위와 마찬가지로 빈 배열을 넣어준다.

## useContext

함수형 컴포넌트에서 Context<sup>[1](#context)</sup> 를 쉽게 사용할 수 있는 Hook.

- 예제

  ```javascript : src/ContextSample.js
  import React, { createContext, useContext } from "react";

  // Context 로 만들고싶은 값을 인자로 넣어준다.
  const themeContext = createContext("Red");

  const ContextSample = () => {
    const theme = useContext(themeContext);
    const style = {
      width: "100px",
      height: "100px",
      background: theme
    };
    return <div style={style} />;
  };
  export default ContextSample;
  ```

---

<strong id = "context">Context</strong> 애플리케이션 전역적으로 데이터가 사용되어야 할 때 사용. 사용자 로그인 정보, 애플리케이션 설정, 테마 등등.

---

### 참고

https://velopert.com/3606
