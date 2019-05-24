# React Hooks 03

## useRef

DOM에 액세스하기 편함. 함수형 컴포넌트에서 ref 를 쉽게 사용할 수 있게 해준다.

- 예제 : Average.js의 input에 포커스하기ㅣ

```javascript
// 1. useRef 를 import 한다.
import React, { useState, useMemo, useCallback, useRef } from "react";

// ...컴포넌트 안쪽
const addList = useCallback(() => {
    const num = parseInt(number, 10);

    if (!Number.isInteger(num)) {
      alert("숫자만 넣어주세요!");
      return;
    }

    const numList = list.concat({
      number: num,
      check: true
    });
    setList(numList);
    setNumber("");
    input.current.focus();
    // 3. ref를 넣은 element 가 바뀔때마다 업데이트 되는 .current 를 이용해 포커싱
  }, [number, list]);

...
return (
    <React.Fragment>
      <div>
        <input value={number} onChange={onChange} ref={input} />
        {/* 2. 참조할 엘리먼트에 ref 를 적용한다. */}
        <button onClick={addList}>숫자추가</button>
        <ul>
          {list.map((value, index) => (
            <li key={index}>
              <input
                type="checkbox"
                onClick={() => excludeNumber(index)}
                style={style}
                checked={value.check && "checked"}
              />
              {value.number}
              <button style={style} onClick={() => deleteNum(index)}>
                Delete
              </button>
            </li>
          ))}
        </ul>
      </div>
      <h1>평균값 : </h1> {avg}
    </React.Fragment>
  );
...
```

- ref 객체를 노드 안에 넣어 리액트에 전달하면 리액트는 노드가 변경될 때마다 .current 속성을 해당 DOM 노드로 변경된다.
- ref 안의 값이 바뀌어도 컴포넌트가 재 랜더링 되지 않기 때문에, 컴포넌트로 로컬 변수<sub>랜더링과 관계없이 바뀔 수 있는 값</sub>를 사용해야 할 때도 uesRef 를 사용할 수 있다.

## 커스텀 Hooks 만들기

- 예제 : 여러 개의 인풋을 관리하는 useInput 만들기(useReducer 이용)

  - useInput.js 작성하기

  ```javascript
  import { useReducer } from "react";

  function reducer(state, action) {
    return { ...state, [action.name]: action.value };
  }

  export default function useInputs(initialForm) {
    // 초기값을 받아와 리듀서에 묶는다.
    const [state, dispatch] = useReducer(reducer, initialForm);
    const onChange = e => {
      dispatch(e.target);
    };
    // state 와 상태를 바꾸는 함수를 가진 배열을 반환해준다. (use~~ 들의 반환값)
    return [state, onChange];
  }
  ```

  - Info.js 에 적용하기

  ```javascript
  import React from "react";
  import useInput from "./useInputs";

  const ChangeInfo = () => {
    const [state, onChange] = useInput({ name: "", phone: "" });
    // useReducer()로 구현했던 작업들을 useInput.js 안에 미리 만들어두었기 때문에 깔끔한 코드를 구현할 수 있음
    const { name, phone } = state;

    return (
      <div>
        <input name="name" value={name} onChange={onChange} />
        <input name="phone" value={phone} onChange={onChange} />
        <p>이름 : {name}</p>
        <p>전화번호 : {phone}</p>
      </div>
    );
  };
  export default ChangeInfo;
  ```

- 예제 : 간단한 로딩 만들기 : usePromise(useState, useEffect 이용)

  - usePromise.js 만들기

  ```javascript
  import { useState, useEffect } from "react";

  /**
   * 프로미스 객체 반환
   * @param {*} promiseCreator : 프로미스 생성용 함수
   * @param {*} deps : 언제 프로미스를 새로 만들지에 대한 조건 배열. 기본값은 빈 배열이다.
   */

  export default function usePromise(promiseCreator, deps) {
    // 참조형 state를 관리?
    const [resolved, setResolved] = useState(null);
    const [loading, setLoading] = useState(false);
    const [error, setError] = useState(null);

    const process = async () => {
      // 로딩 시작
      setLoading(true);
      try {
        // promise 결과가 나올때까지 기다림.
        const result = await promiseCreator();
        setResolved(result);
      } catch (e) {
        setError(e);
      }
      // 로딩끝
      setLoading(false);
    };

    // 조건이 업데이트 될 때마다 process 를 실행시킬 useEffect
    // useEffect 를 사용할 때 파라미터로 전달해주는 함수로 async 를 사용하면 함수가 아닌 프로미스 객체를 반환하기 때문에 오류가 발생한다.
    useEffect(() => {
      process();
    }, deps);

    return [loading, resolved, error];
  }
  ```

  - UsePromiseSample.js 에서 사용해보기

  ```javascript
  import React from "react";
  import usePromise from "./usePromise";

  const wait = () => {
    // 3초 후에 resolve를 부르기로 하는 promise 객체 만들기
    return new Promise(resolve => setTimeout(() => resolve("Hooks?"), 3000));
  };

  const usePromiseSample = () => {
    const [loading, resolved, error] = usePromise(wait, []);

    if (loading) return <div>로딩중...</div>;
    if (error) return <div>Error!</div>;
    if (!resolved) return null;

    return <div>{resolved}</div>;
  };

  export default usePromiseSample;
  ```
