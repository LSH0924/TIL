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


# 리액트 설치하기

  - window cmd 창에서 애플리케이션이 있는 폴더로 이동 후 아래 명령어 입력
    ```
    npm install react
    ```
  - node_modules 디렉토리와 pakage.json 파일 생성
    - node_modules : 설치된 react 패키지 저장
    - pakage.json
      - 저장된 패키지의 리스트(의존성, Dependencies) 나열. 
      - npm 명령어를 통해 node_modules 디렉토리를 초기화 시킬 수 있다.
      - 노드 패키지 전체 파일을 공유하지 않고도 다른 개발자와 프로젝트를 공유할 수 있다. 
