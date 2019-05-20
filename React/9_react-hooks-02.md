# React Hooks 02

## useReducer

- 컴포넌트에서 useState 보다 더 다양한 상황에 따라 다양한 상태를 다른 값을 업데이트해주고싶을 때 사용하는 Hook.
- 리듀서는 현재 상태, 업데이트를 위한 Action 객체를 받아 새로운 상태를 반환한다.
- 리듀서를 사용할 때는 상태의 불변성을 꼭 지켜주어야 한다.
- Redux 에서 리듀서를 사용할 때 받는 Action 객체에는 무슨 액션인지 알려주는 `type` 이 있었지만, useReducer 에서는 `type` 을 사용할 필요가 없다. 객체가 아닌 문자열이나 숫자라도 괜찮음.
- **컴포넌트 업데이트 로직을 컴포넌트 바깥으로 빼낼 수 있다.**

- 예제 : 카운터 재구성

```javascript : src/Counter.js
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action) {
    case "INCREMENT":
      return { value: state.value + 1 };
    case "DECREMENT":
      return { value: state.value - 1 };
    default:
      return state;
  }
}

// 함수형 카운터 컴포넌트 생성
const Counter = () => {
  // 리듀서 사용. 실행할 리듀서 함수와 초기 state의 value 를 넣어준다.
  const [state, dispatch] = useReducer(reducer, { value: 0 });
  // useReducer 를 호출하면 state값과 dispatch(action) 함수를 받아온다.
  return (
    <div>
      <p>
        Count : <strong>{state.value}</strong>
      </p>
      <button onClick={() => dispatch("DECREMENT")}>-</button>
      <button onClick={() => dispatch("INCREMENT")}>+</button>
    </div>
  );
};

export default Counter;
```

- 예제2 : input 상태 관리하기

  - 아무리 input이 많아져도 코드를 짧고 깔끔하게 유지할 수 있다.

  ```javascript : src/Info.js
  import React, { useReducer } from "react";

  function reducer(state, action) {
    return { ...state, [action.name]: action.value };
  }

  const ChangeInfo = () => {
    const [state, dispatch] = useReducer(reducer, { name: "", phone: "" });

    const onChange = e => {
      dispatch(e.target);
    };

    return (
      <div>
        <input name="name" value={state.name} onChange={onChange} />
        <input name="phone" value={state.phone} onChange={onChange} />
        <p>이름 : {state.name}</p>
        <p>전화번호 : {state.phone}</p>
      </div>
    );
  };
  export default ChangeInfo;
  ```

## useMemo

함수형 컴포넌트 내부에서 발생하는 연산을 최적화 할 수 있는 Hook. 랜더링 하는 과정에서 **특정 값이 바뀌었을 때**만 연산을 실행하고, 원하는 값이 바뀐게 아니라면 이전에 연산했던 결과를 다시 사용한다.

- 예제 : 평균값 구하기 : useMemo 를 사용하지 않았을 때

```javascript : src/Average.js
import React, { useState } from "react";

const getAverage = numbers => {
  if (numbers.legth === 0) return 0;
  let sum = 0,
    length = 0;
  numbers
    // num.check가 true인 요소들을 추려낸다.
    .filter(num => num.check)
    // 추려낸 요소들을 서로 더한다.
    .forEach(item => {
      length++;
      sum += item.number;
    });
  return sum === 0 ? 0 : sum / length;
};

// 함수형 컴포넌트 만들기
const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");
  const style = {
    marginLeft: "10px",
    marginRight: "10px"
  };

  const onChange = e => {
    setNumber(e.target.value);
  };

  const addList = () => {
    // 10진수 숫자로 만들기
    const num = parseInt(number, 10);

    // 간단 validate
    if (!Number.isInteger(num)) {
      alert("숫자만 넣어주세요!");
      return;
    }

    const numList = list.concat({
      number: num,
      check: true
    });
    setList(numList);
    // 불변성을 위해 기존 리스트에 concat 작업을 통해 새로운 리스트를 작성한 뒤 업데이트 했다.
    setNumber(""); // input초기화
  };

  // 체크를 풀면 평균값 구하는 대상에서 제외. 체크하면 대상에 포함
  const excludeNumber = key => {
    const numlist = list.map((item, index) =>
      index === key
        ? {
            ...item,
            check: !item.check
          }
        : item
    );
    setList(numlist);
  };

  const deleteNum = key => {
    const numlist = list.filter((item, index) => index !== key);
    setList(numlist);
  };

  return (
    <React.Fragment>
      <div>
        <input value={number} onChange={onChange} />
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
      <h1>평균값 : </h1> {getAverage(list)}
    </React.Fragment>
  );
};

export default Average;
```

- 위 코드는 최적화되어있지 않다. input을 입력할때도, 새 값으로 바뀔때도 계속해서 랜더링 되기 때문에 자원의 낭비가 일어난다.

- 예제 : 위의 예제를 useMemo 로 최적화시키기

```javascript
// useMemo 를 import 한다.
import React, { useState, useMemo } from "react";

// 컴포넌트 안에 새로운 변수 선언
...
const avg = useMemo(() => getAverage(list), [list]);

// 선언한 변수를 평균값을 나타내는 태그 안에 넣어준다.
...
<h1>평균값 : </h1> {avg}
...
```

- list 의 내용이 바뀔때만 getAverage 함수가 호출된다.

## useCallback

useMemo 와 상당히 비슷. 랜더링 성능을 최적화 해야 할 때 사용한다. useCallback 을 사용하면 이벤트 핸들러 함수를 필요할 때만 생성할 수 있다. 컴포넌트의 랜더링이 자주 발생하거나, 랜더링 해야 할 컴포넌트의 개수가 많아질 때 유용하다.

- 예제 : Average.js : 이벤트 함수 최적화하기

```javascript
// useCallback을 import 한다.
import React, { useState, useMemo, useCallback } from "react";

...
// useCallback을 이용해 함수 생성 제어하기
// 체크를 풀면 평균값 구하는 대상에서 제외. 체크하면 대상에 포함
  const excludeNumber = useCallback(
    key => {
      const numlist = list.map((item, index) =>
        index === key
          ? {
              ...item,
              check: !item.check
            }
          : item
      );
      setList(numlist);
    },
    [list]
  );
...
```

- useCallback() 의 첫번째 파라미터로는 생성하고 싶은 함수를 넣어주고, 두번째 파라미터에는 어떤 값이 바뀌었을 때 함수를 새로 생성해 주어야 하는지 명시하는 배열을 넣어준다.
- 비어있는 배열을 넣으면 화면이 처음 랜더링 될 때만 함수가 생성된다.
- 함수 내부에서 기존의 상태값을 의존(조회 등의 작업)해야 할 때는 반드시 두번째 파라미터 안에 포함시켜주어야 한다.
- useCallback 은 useMemo 에서 함수를 반환하는 경우 더 편하게 사용할 수 있다.
- 숫자, 문자열, 객체처럼 **일반 값을 재사용**하기 위해서는 **useMemo**, 함수를 재사용하기 위해서는 **useCallback** 을 사용한다.
