# LifeCycleAPI

- 컴포넌트가 브라우저에 나타날 때, 사라질 때, 업데이트될 때 호출되는 메소드
- 각 컴포넌트 안에 선언된다.

## 컴포넌트 초기 생성시 호출되는 API

- constructor(props)
  - 컴포넌트의 생성자
  - 컴포넌트가 새로 하나 씩 만들어질 때마다 생성자가 호출된다.
  ```javascript
  constructor(props) {
    super(props);
  }
  ```

### componentWillMount()

- v16.3부터 비추장(deprecated) 되었다.
- v16.3부터는 `UNSAFE_componentWillMount()`로 사용된다.
- 컴포넌트가 화면에 출력되기 직전에 호출된다.
- 원래는 브라우저가 아닌 서버사이드 환경에서 호출하는 용도
- 기존의 componentWillMount()에서 하던 처리는 constructor과 componentDidMount()에서 처리할 수 있다.

### render()

- 라이프사이클 메서드 중 유일하게 필수
- 컴포넌트의 모양새를 결정
- 메서드 내부에서 this.props, this.state 에 접근할 수 있다.
- 리액트 요소를 반환
- 아무것도 보여주고싶지 않을 때는 `null` 이나 `false` 를 반환
- render() 안에서 state 를 변경하거나 웹브라우저에 접근해서는 안된다.

### componentDidMount()

- 컴포넌트가 화면에 나타나게 되었을 때 호출된다.
- DOM을 사용하는 외부 라이브러리와 연동작업을 한다. : D3, masonry 등
- 해당 컴포넌트에서 필요로 하는 데이터를 요청하기 위해 axios, fetch 등을 통해 ajax등을 요청한다.
- DOM의으 속성을 읽거나 변화시키는 처리를 한다. : 스크롤 설정, 크기 등

## 컴포넌트 업데이트 전후로 호출되는 API

- props, state의 변화에 따라 호출되는 메소드를 결정한다.

### componentWillReceiveProps(nextProps)

- v16.3부터 비추장(deprecated) 되었다.
- v16.3부터는 `UNSAFE_componentWillReceiveProps(nextprops)`로 사용된다.
- 컴포넌트가 새로운 props를 받게 될 때 호출된다.
- 주로 state가 props에 따라 어떻게 변하는지를 작성한다.
- 새로 받을 props는 매개변수
- 상황에 따라 `getDerivedStateFromProps()`로 대체될 수 있다.

### getDerivedStateFromProps(props, state)

- v16.3 이후로 추가됨
- props로부터 파생된 state를 얻는다.
- 초기 생성 혹은 업데이트 할 때, render()를 호출하기 전에 호출된다.
- 변경사항이 있을 때는 업데이트 된 state를, 변경이 되지 않은 경우는 null을 반환한다.
- props로 받아온 값을 state로 동기화 하는 작업을 해야 할 때 사옹된다.
- 특정 props가 바뀔 때, 설정하고 싶은 state 값을 return한다.
- 메소드의 반환값으로 컴포넌트가 업데이트 된다.

### shouldComponentUpdate(props, state)

- 컴포넌트 최적화에 사용.
- virtual DOM에 리랜더링 작업이 필요하지 않을 때, CPU의 처리량을 줄이기 위해 사용한다.
- 조건을 만족하지 않을 때는 virtual DOM에 리랜더링 하지 않는다.
- 반환형은 true/false.
- 기본적으로는 true를 반환한다.
- false가 반환되면 컴포넌트를 업데이트 하지 않는다.

### componentWillUpdate(nextProps, state)

- v16.3부터 비추장(deprecated) 되었다.
- v16.3부터는 `UNSAFE_componentWillUpdate(nextprops, state)`로 사용된다.
- shouldComponentUpdate가 true를 반환했을 때만 호출된다.
- 애니메이션 효과를 초기화시키거나, 이벤트 리스너를 없애는 작업을 한다.
- componentWillUpdate다음으로는 render() 이 호출된다.

### getSnapshotBeforeUpdate(prevProps, prevState)

- render() -> getSnapshotBeforeUpdate(prevProps, prevState) -> 실제 DOM에 변화 -> componentDidUpdate(prevProps, prevState, snapshot)
- DOM 변화가 일어나기 직전의 DOM 상태정보를 얻을 수 있다.
- 반환값은 componentDidUpdate 의 3번째 매개변수로 보내진다.
- 이전 DOM의 속성과 비교해 변화가 생긴 부분을 처리할 때 사용한다.

### componentDidUpdate(prevProps, prevState, snapshot)

- 컴포넌트에서 render() 호출 후, DOM 변화가 생긴 뒤<sub>(`this.props, this.state`는 이미 바뀌어있는 시점)</sub> 호출된다.
- 파라미터를 통해 변경 전의 props와 state를 받아올 수 있다.
- snapshot을 통해 변경 전의 원하는 DOM 속성값을 얻을 수도 있다.

## 컴포넌트 제거시 호출되는 API

### componentWillUnmount()

- 컴포넌트가 더 이상 필요하지 않을 때 호출된다.
- 등록했던 이벤트, setTimeOut을 제거한다.
- 사용한 외부 라이브러리를 dispose시킨다.
