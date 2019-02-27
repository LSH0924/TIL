# Node.js Application 만들기

1. 필요한 모듈 import

```javascript
// require() 는 필요한 모듈을 불러올 때 사용한다.
// HTTP모듈을 불러와 http변수에 저장하기
const http = require("http");
```

- require()

  - 자바스크립트로 만든 모듈을 가져올 수 있다.
  - 노드는 파일 단위로 하나의 모듈을 만들 수 있음.

    ```javascript
    //module.js
    function sum(a, b) {
      return a + b;
    }
    module.export = sum; // sum 함수를 export할것

    // 다른 파일.js
    const sum = require("./module.js");
    console.log(sum(10, 100)); // 콘솔출력 110
    ```

2. 서버 생성

```javascript
const host = "127.0.0.1";
const port = "3000";

const server = http.createServer(function(request, response) {
  /* 
        HTTP 헤더 전송하기
        첫번째 매개변수에는 포트번호를, 두번째 매개변수에는 {HTTP Status:Content Type} 을 넣음
        HTTP Status: 200 : OK
        Content Type: text/plain
    */
  response.writeHead(200, { "Content-Type": "text/plain" });
  /*
        Response Body 를 "Hello World" 로 설정
    */
  response.end("Hello World\n");
});
server.listen(port, host, () => {
  console.log("서버가 실행되었습니다.  http://${hostname}:${port}/");
});
```

## Node.js 서버 실행

```
$ node main.js
```
