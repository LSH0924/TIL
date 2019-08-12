# Router
- 라우팅: 주소에 따라 다른 뷰를 보여줌
- 리액트에는 라우팅 기능이 내장되어있지는 않다.

## SPA(Single Page Application)

- 서버에서 제공하는 페이지가 한 개인 애플리케이션
- 서버 사이드 랜더링 애플리케이션과의 비교
    - 서버 사이드 랜더링 애플리케이션은 유저의 요청이 있을 때바다 페이지를 새로고침하고, 페이지를 로딩할 때마다 서버에서 리소스를 받아 해석한 후 랜더링한다.
    - 서버에 부담이 많이 가고, 웹에서 제공하는 정보가 많아지면 속도가 느려진다.
    - 단점을 극복하기 위해 캐싱이나 압축 등을 제공하지만, 불필요하게 트래픽이 낭비되기 때문에 사용자와의 상호작용이 많은 모던 웹 어플리케이션에는 적합하지 않다. 
- 싱글 페이지 애플리케이션은 뷰 랜더링을 브라우저가 담당
- 애플리케이션을 웹 브라우저에 로드시킨 후 필요한 데이터만 받아 보여준다.
- 단점
    - 페이지를 로딩할 때 사용하지 않을 수도 있는 페이지와 관련된 컴포넌트 코드도 모두 불러오기 때문에, 애플리케이션 규모가 커지면 자바스크립트 파일의 크기가 너무 커진다.
- 코드 스플리팅을 사용하면 라우트별로 파일을 나눠 트래픽과 로딩 속도를 개선할 수 있다.

## react-router
- 클라이언트 사이드에서 진행하는 라우팅 과정을 간략하게 해준다.
- 페이지 주소를 변경했을 때 주소에 따라 다른 컴포넌트를 랜더링 해 주고, 주소 정보를 컴포넌트의 props로 전달해 컴포넌트 단에서 주소 상태에 따라 다른 작업을 하도록 설정할 수 있다.
- react-router, react-router-dom 사용

### react-router-dom
- BrowserRouter: HTML5의 history API를 사용해 새로고침 하지 않고 페이지 주소를 교체해주는 컴포넌트
- Route : 경로와 보여줄 컴포넌트를 설정하는 컴포넌트
    - path : 경로
    - component : 보여줄 대상 컴포넌트
    - exact : 현재 주소가 path에서 설정한 값과 완벽하게 일치할 때만 컴포넌트를 보여줌
- Link : 새로고침을 사용하지 않고 주소창 상태를 변경, 원하는 라우트로 화면을 전환하는 컴포넌트
    - to : 이동할 주소
- NavLink : Link 컴포넌트와 비슷하나, 현재 주소와 해당 컴포넌트의 목적지 주소가 일치하면 특정 스타일 혹은 클래스를 지정할 수 있는 추가 기능이 있다.
    - activeStyle : to로 지정한 링크를 활성화 했을 때 지정할 스타일
    - activeClassName : to로 지정한 링크를 활성화 했을 때 지정할 CSS 클래스

## route params & Query String
- params를 사용하거나 Query String 을 사용해 라우트의 경로에 특정 값을 집어넣을 수 있다.

### params
- URL의 params 를 지정할 때는 `:key` 형식으로 지정. 슬래시(/)로 구분하고 key 는 param의 이름으로 사용한다.
- 설정한 params 객체는 컴포넌트를 라우트로 설정했을 때, props로 전달받는 match 객체 내부에 있다.
- App.js

    ```javascript
    import React from "react";

    import {Route} from 'react-router-dom';
    import {Home, About, TodoList} from '../pages'

    const App = () => {
        return (
        <div>
            <Route exact path="/" component={Home} />
            // params 의 이름은 name으로 설정한다. 파라미터가 여러 개면 /:key를 여러 개 붙이면 된다.
            // 라우트에서 :name 값을 선택적으로 입력받을 수 있게 `?`를 입력한다.
            <Route exact path="/about/:name?" component={About} />
            <Route exact path="/todo" component={TodoList} />
        </div>
        );
    }
    export default App;
    ```
- params 를 받는 About.js
    ```javascript
    import React from "react";
    import queryString from "query-string";

    const About = ({location, match}) => {
        const query = queryString.parse(location.search);
        console.log(query);
        
        const color = {
            color: query.color
        }
        return (
            <div>
                <h2 style={color}>About</h2>
                <p>리액트 라우터 사용하기 실습</p>
                <p>안녕하세요, {match.params.name}의 연습장입니다.</p>
            </div>
        );
    };

    export default About;
    ```
- match.params에 지정한 파라미터를 사용

#### 라우트로 사용된 컴포넌트가 전달받는 props

##### match
- <Route> 컴포넌트에서 설정한 params를 조회하거나, 서브 라우트를 만들 때 현재 path를 참조할 때 사용
- 나타나는 URL이 같아도 다른 라우트에서 사용된 match는 다른 정보를 가지고 있다.
```javascript

// Post의 match
{
    isExact: true,
    params: {},
    path: "/post",
    url: "/post",
    __proto__: Object
}

{
    isExact: true,
    params: {id: "2"},
    path: "/post/:id",
    url: "/post/2",
    __proto__: Object
}
```
- **match.url** : 현재 라우트에 설정된 경로를 알려준다. 직접 지정하는것보다 match.url을 사용하는 게, 컴포넌트의 라우트 주소를 변경할 때 일일이 변경하지 않아도 된다는 점에서 편하고 정확하다.

##### location
- 현재 페이지의 주소 상태를 알려준다.
- ~~모든 라우트에서 값이 같다.~~ 정확히는 뭐가 같은지 모르겠다. 해시가 없다는거..?
```javascript
// Posts의 location
{
    hash: "",
    key: "0x8bgz",
    pathname: "/post",
    search: "",
    state: undefined,
    __proto__: Object
}

// Post의 location
{
    hash: "",
    key: "gxkfab",
    pathname: "/post/2",
    search: "",
    state: undefined,
    __proto__: Object
}
```
- search 값에서 URL Query를 읽는데 사용하거나, 주소 변경을 감지하는데 사용한다.

    ```javascript
    // componentDidUpdate 라이프사이클 메서드 사용
    componentDidUpdate(prevProps, prevState){
        if(prevProps.loaction !== this.props.location){
            // 주소가 변경될 시 수행할 작업 등
        }
    }
    ```

##### history
- 뒤로 가기, 앞으로 가기, 새로운 주소 이동 등 현재 라우터를 조작할 때 사용
- push("이동하고싶은 패스") : 페이지 방문 기록이 남는다. 페이지 이동.
- replace("이동하고싶은 패스") : 페이지 방문기록을 남기지 않아 페이지 이동 후 뒤로가기 버튼을 눌렀을 때 이동 전의 전 페이지가 나타난다. 
- action : 현재 history의 상태를 알려준다. 페이지를 처음 방문했을 때는 POP, 링크를 통한 라우팅 혹은 push를 통한 라우팅을 했을 경우 PUSH, replace를 사용했을 경우 REPLACE를 가진다.
- block("이 페이지를 떠나시겠습니까?") : 페이지에서 벗어나려고 할 때 사용자에게 정말 페이지를 떠날 것인지 묻는 창을 띄운다.
- go(), goBack(), goForward() : 이전 페이지 혹은 다음 페이지로 이동. go에 음수를 넣으면 이전 페이지로, 양수를 넣으면 다음 페이지로 이동한다.

### Query String
- URL 뒤에 아래 형식으로 지정한다.

> /about/aa?key=value&anotherKey=value

- Query String을 파싱하기 위해서는 `query-string` 라이브러리를 설치해야 한다.
- 라우트 내부에서 정의 : 라우트로 설정된 대상 컴포넌트의 props 중 하나인 `location` 객체의 `search` 값을 조회한다.
- 파라미터의 값은 모두 문자열로 들어오기 때문에 사용하기 전에 형변환 시켜주어야 한다.

## 자바스크립트에서 라우팅
- 조건에 맞춰 자바스크립트로 페이지를 이동시키는 로직을 작성해야할 때 사용
- 라우트로 사용된 컴포넌트의 props 중 하나인 history 객체의 push 함수를 활용
    ```javascript
    import React from "react";
    const Home = ({history}) => {
        return (
            <div>
                <h2>HOME</h2>
                <button
                onClick = {()=>{
                    history.push('/about/Javascript');
                }}>
                    자바스크립트를 사용해 이동 - about
                </button>
            </div>
        );
    };
    export default Home;
    ```