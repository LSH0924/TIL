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

