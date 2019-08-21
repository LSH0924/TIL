# koa-router
- koa 프레임워크에서 라우팅을 간단하게 사용할 수 있도록 돕는 라이브러리
- 라우트를 설정할 때 첫 번째 파라미터에는 라우트의 Path를, 두 번째 파라미터에는 라우트를 처리할 함수를 전달한다.
- router.routes() : 요청과 일치하는 패스를 전달하는 미들웨어를 반환
- router.allowedMethods() : 허용된 메서드을 포함하는 허용 헤더로 OPTIONS 요청에 응답하기 위한 별도의 미들웨어를 반환

## 파라미터, 쿼리스트링 사용
- 리액트 라우터에서 사용했던 설정과 비슷하다.

### 파라미터
- 콜론(:)을 사용해 파라미터를 지정한다. /about/:name 등.
- /about/:name? 처럼 물음표를 붙여 선택적 파라미터임을 나타낸다.
- 파라미터는 `ctx.params`에서 조회할 수 있다.

### 쿼리스트링
- `/post/?age=20` 형식으로 요청했을 때, `{age:'20'}` 형태의 객체를 얻을 수 있다.
- 쿼리스트링으로 보낸 객체는 `ctx.query`에서 조회할 수 있다.
- 문자열 형태의 쿼리스트링을 조회할 때는 `ctx.querystring`을 사용한다.

## 라우트 모듈화
- 각 라우트를 하나의 index.js파일 안에 두기엔 너무 길고, 유지보수 하기도 힘들기 때문에 모듈로 분리한다.

## 컨트롤러
- 라우트 처리 함수들만을 분리해둔 파일. 백엔드 기능을 구현한다.

### koa-bodyparser
- POST/PUT/PATCH 등, 메서드의 Request Body에 JSON 형식으로 데이터를 넣어주면 이 데이터를 파싱 후 서버에서 사용할 수 있게 해준다.
- 라우터를 설정하는 코드 윗부분에서 적용시켜야 한다.
    ```javascript
    const Koa = require("koa");
    const Router = require("koa-router");
    const bodyParser = require("koa-bodyParser");

    const api = require("./api");

    const app = new Koa();
    const router = new Router();

    // 라우터에 api라우트 설정
    router.use("/api", api.routes());

    // 라우터를 적용하기 전에 bodyParser부터 적용
    app.use(bodyParser());

    // app 인스턴스에 라우터 적용하기
    app.use(router.routes()).use(router.allowedMethods());
    ...
    ```
- REST API의 request body는 `ctx.request.body`에서 조회가능
- 아래의 형식으로 컨트롤러를 작성한다.
    ```javascript
    exports.write = (ctx) => {
        const {title, body} = ctx.request.body;
        postId++;

        // 등록할 포스트 객체 작성
        const post = {
            id: postId, title, body
        };

        post.push(post);
        ctx.body = post;
    };
    ...
    ```
- export 한 컨트롤러 함수는 아래와 같이 불러와 사용할 수 있다.
    ```javascript
    const postsCtrl = require("./posts.ctrl");
    ...
    posts.get("/", postsCtrl.list);
    ...
    ```