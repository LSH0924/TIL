# NODE_PATH 설정

- NODE_PATH를 설정하면 컴포넌트나 모듈을 import 할 때 절대 경로로 사용할 수 있다. (../ 등 생략 가능)
- webpack 의 resolve 기능을 사용해야 하지만, create-react-app으로 만든 프로젝트에서는 NODE_PATH 를 자동으로 resolve 해주므로 따로 webpack 설정을 변경하지 않아도 된다.

## cross-env
- Windows 환경에서는 일반적인 NODE_PATH가 정상 작동하지 않으므로 cross-env를 설치한다.

- package.json의 script 항목
```javascript
...
  "scripts": {
    "start": "cross-env NODE_PATH=src react-scripts start",
    "build": "cross-env NODE_PATH=src react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
...
```