# 리덕스 개발자 도구

- 액션을 디스패치할 때마다 기록을 확인하고, 이전의 상태로 돌아갈 수 있게 할 수 있는 개발용 툴
- 현재 리덕스 상태, 방금 디스패치한 액션, 액션으로 어떤 값을 바꿨는지 등을 확인할 수 있다. 
- 크롬 확장프로그램으로 제공
    - [https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd/related?hl=ko](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd/related?hl=ko)

- 확장프로그램에 추가한 뒤 개발자 도구를 활성화 하는 작업이 필요하다.
    - src/index.js
        ```javascript
        // 스토어 생성 코드를 수정한다.
        // const store = createStore(reducers);
        const store = createStore(reducers, window.devToolsExtension && window.devToolsExtension());
        ```