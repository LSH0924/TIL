# node.js 와 npm 설치

  - https://nodejs.org/en/ 으로 가서 node.js 다운받기
  
    ```
    node -v  // node 버전 확인
    npm -v   // npm(Node Packaged Manager) 버전 확인
    ```

  - 노드 패키지 관리자의 명령어**npm**으로 노드 패키지를 로컬에 설치한다.
  - 명령어 : **-g** : npm에게 지정한 패키지를 전역적으로 설치하도록 명령
  
    ```
    npm install -g 패키지이름
    npm install 패키지이름
    // 개발중인 애플리케이션 안에서만 사용할 패키지를 설치
    ```

  - 명령어 : **-y** : pakage.json 파일을 초기화시킬 수 있다.
  
    ```
    npm init -y
    ```

  - 프로젝트를 초기화시킨 후 새 패키지를 설치하는게 좋음
  - 명령어 : **--save-dev**
    + 노드 패키지가 개발환경에서만 사용되게 한다. 
    + 실제 제품이 운영되는 환경에서 이 명령어를 통해 설치된 노드 패키지는 사용되지 않는다.
    + 테스트를 위한 노드 패키지 등을 설치할 때 사용

    ```
    npm install --save-dev 패키지이름
    ```

-----

## 참고
- 리액트 도움닫기(The Road to learn React) : 로빈 위어크, 이수진
(https://github.com/the-road-to-learn-react/the-road-to-learn-react-korean/blob/master/manuscript/chapter1.md#12-%EC%A4%80%EB%B9%84-%EC%82%AC%ED%95%AD)
