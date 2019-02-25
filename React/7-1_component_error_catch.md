# 컴포넌트의 render()에서 에러가 발생할 때 사용하는 API

### componentDidCatch(error, info)

```javascript
  componentDidCatch(error, info){
    this.setState({error: true});
  }
```

- 에러를 잡아내 어플리케이션의 크래쉬를 방지한다.
- 에러발생 시 componentDidCatch()가 실행되면, this.state.error가 true가 되고, render()에서 에러를 띄운다.
- 컴포넌트 자신의 render()에서 에러가 발생하는 것은 잡아낼 수 없지만, 자식 컴포넌트의 render()에서 발생하는 에러는 catch할 수 있다.

## render()에서 오류가 자주 나는 이유

- 존재하지 않는 함수를 호출하려고 할 때<br>
  ex) props로 받아온 줄 알았던 함수가 존재하지 않을 때
- 배열, 객체가 올 줄 알았는데, 해당 객체나 배열이 존재하지 않을 때
  ex) Array is undefined 등. <sub>조건이나 defaultProps를 이용해 방지할 수 있다.</sub>
