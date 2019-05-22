# Hoc(Higher-order Component)

- 함수형 컴포넌트? HOC를 이용해서 쓸데없는 코드를 늘리지 않고 여러 개의 컴포넌트에 특정 기능을 부여할 수 있다.
- 컴포넌트 기능상 자주 반복되는 코드들이 있을 때 HOC 를 만들어 해결해줄 수 있다.
- 파라미터로 컴포넌트를 받고, 함수 내부에서 지정한 기능을 가진 새로운 컴포넌트(파라미터로 받은 애랑은 아직 관계없음)를 만든 다음 만든 컴포넌트 안에서 파라미터로 받아온 컴포넌트를 랜더링한다.
- 받아온 컴포넌트의 props 는 그대로 받은 컴포넌트에게 주입해주고, 필요에 따라 추가 props 도 넣어준다.
- 라이프사이클 메소드를 추가, Redux 에서 특정 값을 받아와 주입, 다국어 지원 등 여러 방면에서 활용할 수 있다.

## HOC 사용하기

1. 반복되는 코드 찾아내기
2. HOC 작성하기

- HOC 의 이름은 `with~~` 형식으로 짓는다.

  ```javascript
  import React, { Component } from "react";
  import axios from "axios";

  // url => wrappedComponent 로 함수안에서 함수를 반환하는 이유는
  // 나중에 compose 등을 이용해 여러 개의 HOC 를 합쳐 사용하게 될 때를 위해서이다.
  //  HOC 굳이 함수를 리턴해주지 않고 바로 컴포넌트를 리턴할 수도 있다. 형태 매우 다양
  const withRequest = url => WrappedComponent => {
    // 기능을 추가해서 반환할 새로운 컴포넌트를 만들어준다.
    return class extends Component {
      state = {
        data: null
      };
      // 비동기적 초기화
      async initialize() {
        try {
          const response = await axios.get(url);
          this.setState({
            data: response.data
          });
        } catch (e) {
          console.error(e);
        }
      }
      // 컴포넌트가 마운트되고난 뒤, 이니셜라이징
      componentDidMount() {
        this.initialize();
      }
      render() {
        const { data } = this.state;
        // 새 컴포넌트 안에서 매개변수로 받은 컴포넌트를 랜더링
        return <WrappedComponent {...this.props} data={data} />;
      }
    };
  };

  export default withRequest;
  ```

3. HOC사용하기

- withRequest 의 파라미터로 넘겨준 url 로 데이터를 받아와 새로운 컴포넌트를 작성하는 함수를 만든 뒤 반환해주고, 반환한 함수에 다시 컴포넌트를 넘겨 원하는 기능이 부여된 새 컴포넌트를 받는다.
- Post.js

```javascript
import React, { Component } from "react";
// 만들어둔 HOC 함수를 import 한다.
import withRequest from "./withRequest";

class Post extends Component {
  render() {
    const postStyle = {
      width: "400px",
      minHeight: "150px"
    };

    const { data } = this.props;
    if (!data) return null;

    return (
      <div style={postStyle}>
        <h3>{data.title}</h3>
        <div>{data.userId}</div>
        <div>{data.body}</div>
      </div>
    );
  }
}
// withRequest 가 반환한 함수에 Post 컴포넌트를 매개변수로 넣어 원하는 기능이 부여된 새로운 컴포넌트를 받는다.
export default withRequest("https://jsonplaceholder.typicode.com/posts/1")(
  Post
);
```

- Comment.js

```javascript
import React, { Component } from "react";
// 만들어둔 HOC 함수를 import 한다.
import withRequest from "./withRequest";
class Comments extends Component {
  render() {
    const { data } = this.props;
    if (!data) return null;

    const commentStyle = {
      width: "400px",
      minHeight: "60px",
      border: "1px solid gray",
      borderRadius: "5px",
      marginBottom: "5px"
    };

    const commentList = data.map(item => (
      <div style={commentStyle} key={item.id}>
        <div>
          <label>{item.name}</label>
          <label>{item.email}</label>
        </div>
        {item.body}
      </div>
    ));

    return <div>{commentList}</div>;
  }
}

export default withRequest(
  "https://jsonplaceholder.typicode.com/comments?postId=1"
)(Comments);
```

---
## 참고

https://velopert.com/3537
