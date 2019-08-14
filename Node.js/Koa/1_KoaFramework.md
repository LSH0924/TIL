# Koa
- Express를 리팩토링 한 결과물. 기존 Express에 비해 아키텍쳐가 많이 바뀌었기 때문에 새로운 이름을 사용하게 되었다.
- Express보다 가벼우며, Node v7.6부터 정식 지원하는 async/await 문법을 편하게 사용할 수 있다.
    - 비동기 작업 시 매우 편함
- 미들웨어의 배열로 구성되어 있다.

## Koa.use()
- 미들웨어를 애플리케이션에 등록하기 위한 함수
- 파라미터로 받는 함수를 하나의 미들웨어로 취급한다.
- 등록하는 순서대로 미들웨어가 처리된다.
- 미들웨어 함수의 파라미터
    - ctx : 웹 요청과 응답 정보
    - next : 처리중인 미들웨어의 다음 순번 미들웨어를 호출. next를 명시하지 않으면 그 다음 미들웨어는 호출하지 않는다.
    - index.js
    ```javascript
    const Koa = require("koa");
    const app = new Koa();

    // 사용할 미들웨어 등록하기
    // 콘솔창에 1, 2까지만 출력되고 웹 페이지에는 Not Found가 나타남
    app.use((ctx, next) => {
        console.log(1);
        // 다음 순번의 미들웨어를 호출
        next();
    });

    app.use(() => {
        console.log(2);
    });

    app.use((ctx) => {
        ctx.body = "hello world!";
    });

    // 40000번 포트 열기. 연결되면 console.log를 출력
    app.listen(4000, () => {
        console.log("4000번 포트에 연결되었습니다.");
    });

    const hello = 'hello';
    ```

## Nodemon
- 개발용 의존모듈로써 사용
- 서버 코드를 변경할 때마다 자동으로 서버를 재시작 해준다.
- package.json에 script 블럭 추가
```javascript
{
    ...
    ,
    "scripts": {
        "start" : "node src",
        // src 디렉토리를 주시하면서 src 안의 내용이 바뀌면 src/index.js를 재시작한다.
        "start:dev" : "nodemon --watch src/ src/index.js"
    }
}
```