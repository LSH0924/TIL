
# 리액트 설치하기

  ## CDN을 통해 설치하기
  
  - CDN(Content Delivery Network) : 콘텐츠 전송 네트워크
    + 콘텐츠를 효율적으로 전달하기 위해 여러 노드를 가진 네트워크에 데이터를 저장 후 제공하는 시스템
    + 번들링 된 파일은 react.js 뿐
    + CDN을 별도로 호스팅하거나 응용프로그램 안에 설치해서 사용할 수 있다.
  - 사용방법
    + HTML 파일에서 `<script>` 인라인태그로 CDN의 url을 작성한다.
    + react와 react-dom의 url을 추가

      ```
      <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
      <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
      ```

  ## npm을 통해 설치하기

  - window cmd 창에서 애플리케이션이 있는 폴더로 이동 후 아래 명령어 입력
    
    ```
    // 초기화시키기
    npm init -y

    // 리액트 설치하기
    npm install react react-dom
    ```
  - node_modules 디렉토리와 pakage.json 파일 생성
    - node_modules : 설치된 react 패키지 저장
    - pakage.json
      - 저장된 패키지의 리스트(의존성, Dependencies) 나열. 
      - npm 명령어를 통해 node_modules 디렉토리를 초기화 시킬 수 있다.
      - 노드 패키지 전체 파일을 공유하지 않고도 다른 개발자와 프로젝트를 공유할 수 있다. 
      - `npm install` 스크립트는 pakage.json 파일에 나열된 모든 의존성을 취득한 후 `node_modules/` 폴더에 설치한다.

-----

## 참고
- 리액트 도움닫기(The Road to learn React) : 로빈 위어크, 이수진
(https://github.com/the-road-to-learn-react/the-road-to-learn-react-korean/blob/master/manuscript/chapter1.md#12-%EC%A4%80%EB%B9%84-%EC%82%AC%ED%95%AD)
