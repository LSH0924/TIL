# Immutability(불변성)

- 리액트 컴포넌트의 state를 변경할 때는 무조건 setState() 를 통해 업데이트 해주어야 한다.
- 직접적으로 state를 변경하면 리랜더링이 되지 않는다. -> 컴포넌트 최적화 불가
- 불변성을 유지하면 shouldComponentUpdate를 통해 컴포넌트의 최적화를 쉽게 구현할 수 있다.

- 단, 불변성을 유지하면서 코드르 작성하면 state 등의 구조가 깊을 때는 코드가 복잡해진다.
  ```javascript
  //state가 복잡한 구조로 되어있을 때
  state={
    first:{
      second:{
        third:{
            changeState : "state",  // 이 state를 바꾸고싶을 때
            changeState2: "changeState2"
        },
        third2:"third2"
      },
      second2:"second2"
    }
  }

  // 바꾸려면
  const {changeState} = this.state;
  this.setState({
      first:{
        ...first,
        second: {
          ...first.second,
            third:{
              ...first.second.third,
              changeState : "state"  // 여기까지..
            }
        }
      }
  });
  ```

## Immutable.js

- state의 불변성을 유지하면서 코드를 간결하게 해주는 리액트 생태계의 라이브러리
- 설치하기
  ```terminal
  yarn add immutable
  ```

- **Immutable.js 의 규칙**
  - Map : 객체
  - List : 배열
  - `set`XXX : 설정할 때
  - `get`XXX : 값을 읽어올 때
  - `update`XXX : 값을 읽어온 뒤 새롭게 설정할 때
  - XXX`In` : 내부에 있는 값에 어떠한 처리를 하려고 할 때 사용
  - toJS : 일반적인 자바스크립트 객체로 변환할 때 사용
  - List 는 배열이 가지고 있는 내장함수와 비슷한 함수들을 가지고 있다.
    - push, slice, filter, sort, concat 등
  - 특정 Key를 지울 때, 혹은 List에서 원소를 지울 때 `delete`를 사용

  - 이외의 규칙은 Immutable.js 의 [메뉴얼](https://immutable-js.github.io/immutable-js/) 참조

- 