# JSX

> 0_React.md 참조
>
> - React를 위해 만들어진 새로운 자바스크립트 문법
> - XML 형식으로 쓰인 코드를 javascript 형식으로 바꾸기 위해 사용한다.
> - PHP의 개량판인 XHP에서 기원한다.
> - 한눈에 이해하기 쉬움(직관적)

- 컴포넌트 클래스 안의 render() 의 return부분에서 사용
  ```javascript
    class App extends Component{
      render(){
        // javascript code
        return (
          // JSX code
        );
      }
    }
  ```

## JSX 사용규칙

- 사용하는 태그는 꼭 닫아주어야 한다.(닫기 태그)
- 두 개 이상의 Element는 하나의 Element로 감싸주어야 한다.
  - Virtual DOM 에서 컴포넌트 변화를 감지해낼 때 효율적으로 비교할 수 있도록 컴포넌트 내부는 DOM 트리 구조 하나여야 한다.
  - 여러 개의 엘리먼트를 감싸기 위해서만 사용하는 컴포넌트, Fragment. (리액트 v16이상에서 제공)
    > <React.Fragment></React.Fragment>
    > <Fragment></Fragment> // 다른 사용자 속성은 전달할 수 없지만, key는 전달할 수 있다.
    > <></> // 축약형 : 다른 태그와 달리 키와 속성을 지원하지 않는다. 단순히 묶기 위한 태그. 아직 지원하는 툴이 많지 않음

## JSX 안에서 자바스크립트 값 사용하기

### 미리 알아둘 것 : const 와 let
  - const는 상수. 선언하고나서 바뀌지 않는 값이다. 배열, 객체일 경우는 변경할 수 있다.
  - let은 바뀔 수 있는 값임을 나타낸다.
  - var의 스코프가 function 단위라면, **const와 let의 스코프는 블록`{}` 단위**
  - const는 객체, 배열의 값이 변경된다고 하더라도 데이터 구조는 변하지 않는다.
  - 같은 블록 내부에서 중복으로 선언 할 수 없다.
  - react는 불변성을 중시하기 때문에 주로 const를 사용
- render() 안에서 const로 변수를 선언한 뒤, 변수명을 JSX 안의 `{}`<sub>(자바스크립트 표현식)</sub> 안에 넣어주면 사용할 수 있다.

## JSX 조건부 랜더링

- if문은 사용할 수 없다.
- 조건을 만족하는지 아닌지에 따라 다른 값을 보여주고싶을 때는 삼항 연산자를 사용한다.

  ```javascript
  class App extends Component{
    render(){
      const number = 10;
      return (
        {number > 0 ? <p>조건이 만족할 때</p>
           : <p>조건이 만족하지 않을 때</p>
        }
      );
    }
  }
  ```

- 조건을 만족하는 경우만 컴포넌트를 보여주고 싶을 때 AND(&&)연산자를 사용한다.

  ```javascript
  class App extends Component{
    render(){
      const number = 10;
      return (
        {number > 0 > &&
          (<div>
            <p>조건이 만족할 때</p>
          </div>)}
      );
    }
  }
  ```

- 복잡한 조건이 필요할 경우, IIFE(Immediately invoked function expression, 즉시 실행 함수표현) 를 사용

  - if, switch문을 사용할 수 있다.
  - 화살표함수로 축약해서 사용할 수 있다.

    ```
    // 화살표 함수 : this, argument, super 개념이 없는 함수
    function(){...}   →   ()=> {...}
    ```

## Style과 className

- Inline style을 설정할 때

  - javascript code부분에서 스타일 오브젝트를 생성한 후, JSX코드에서 사용한다.
  - 작성하는 스타일 오브젝트의 key 는 CamelCase로 작성한다.
  - javascript 객체의 key에서는 `-` 를 사용할 수 없기 때문에 다른 key 나 웹브라우저의 접두어 또한 머릿글자를 대문자로 변경해 사용한다. (-ms 만 예외로 소문자를 사용한다.)

    ```javascript
    class App extends Component {
      render() {
        const style = {
          border: "1px solid teal",
          background: skyblue
        };
        return <div style={style} />;
      }
    }
    ```

- 외부 css파일 등의 스타일을 적용시킬 때

  - 컴포넌트에 `className="클래스이름"` 을 입력한다.
  - 이미 javascript 에 class 키워드가 존재하기 때문에 구분하기 위해 className 을 사용함
  - 클래스 이름을 가져오는 .css 파일은 반드시 import해야 한다.

    ```javascript
    import "./style.css";

    class App extends Component {
      render() {
        return <div className="style" />;
      }
    }
    ```

## 주석

```javascript
var element = (
  <div>
    {
      /* 각 엘리먼트 바깥에서 사용하는 주석*/
    }
    <input type="text"
      // self-closed 태그에서만 사용할 수 있는 주석
      // 엘리먼트 내부에서 한줄 주석 사용하기
      /*마지막 닫는 태그가 꼭 새로운 줄로 존재해야 한다.*/
    />
  </div>
);
```
---
## 참고
리액트를 다루는 기술 - 김민준 저