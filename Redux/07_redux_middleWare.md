# 미들웨어(Middleware)
- 액션과 리듀서 사이의 중간장치
- 액션을 디스패치 했을 때 리듀서에서 처리하기 전에 사전에 지정된 작업들(전달받은 액션을 콘솔에 기록하거나, 액션 정보를 기반으로 액션을 취소하거나 다른 액션을 실행하는 등의)을 실행한다.

## 미들웨어 직접 만들고 적용
- 미들웨어 내부에서 store.dispatch를 사용할 때, 특정 조건을 만족하면 같은 액션이 아니라 다른 액션으로 실행하게끔 만든다.
- /lib/loggerMiddleware.js
    ```javascript
    const loggerMiddleware = store => next => action => {
        console.log("현재 store의 state", store.getState());
        console.log("현재 store의 todos", store.getState().todos.toJS()); // state안의 todos 출력
        console.log("액션", action);
        
        // 액션을 다음 미들웨어 또는 리듀서로 넘긴다.
        const result = next(action);
        
        console.log("다음 작업으로 넘긴 뒤 store의 state", store.getState());
        console.log("현재 store의 todos", store.getState().todos.toJS());
        console.log("\n"); // 로그 구분용 newLine

        // store.dispatch(ACTION_TYPE) 했을 때(리듀서로 갔을 때)의 결과
        return result;
    };

    export default loggerMiddleware;
    ```
- 적용하기 : store.js
    ```javascript
    import { createStore, applyMiddleware, compose } from "redux";
    import modules from "./modules";
    import loggerMiddleware from "./lib/loggerMiddleware";

    const store = createStore(
        modules,
        compose(
            applyMiddleware(
                loggerMiddleware
            ),
            window.devToolsExtension && window.devToolsExtension()
        )
    );

    export default store;
    ```
    - `compose()`
        - redux 에서 제공하는 함수형 프로그래밍 유틸리티
        - 여러 스토어 인핸서들을 순차적으로(오른쪽에서 왼쪽으로) 적용시키는 함수를 반환한다.
    - `applyMiddleware()`
        - 여러 개의 미들웨어를 묶어서 전달해주는 역할
        - 미들웨어 순서는 applyMiddleware에 전달한 파라미터 순서대로 지정된다.
- 적용 후
![로거 미들웨어 적용](./image/15.1.2.3.png)