# create-react-app
  
  - 2016년 페이스북이 제안한 리액트 제로 구성 설치 스타터킷(Zero-Configuration Setup Starter Kit)
  - create-react-app을 통해 어플리케이션을 부트스트래핑 할 수 있다.
    + 부트스트래핑(bootstrapping) : 어플리케이션을 최초 생성 후 브라우저에서 실행하는 과정
  - 이미 리액트 개발도구와 환경설정이 다 되어 있음
  - 생성하는 어플리케이션의 이름은 대문자를 포함할 수 없다.

  - 전역 노드 패키지에 create-react-app 패키지 설치하기

    ```
    // create-react-app 설치하기
    npm install -g create-react-app

    // create-react-app 패키지 버전 확인
    create-react-app --version
    
    // 새로운 어플리케이션 부트스트래핑하기
    create-react-app
    
    // 위의 명령어를 입력하면 사용방법이 나옴
    Please specify the project directory:
    create-react-app <project-directory>

    For example:
    create-react-app my-react-app

    Run create-react-app --help to see all options.

    // 설명에 따라 어플리케이션 부트스트래핑
    create-react-app my_account_book
    ```
    
  - 부트스트래핑 후 생성된 폴더 안의 내용물
    ```
    D:\github\MyAccountBook\my_account_book 디렉터리

        2019-02-16  오후 08:38    <DIR>          .
        2019-02-16  오후 08:38    <DIR>          ..
        1985-10-26  오후 05:15               310 .gitignore
        2019-02-16  오후 08:37    <DIR>          node_modules
        2019-02-16  오후 08:38           647,215 package-lock.json
        2019-02-16  오후 08:38               489 package.json
        2019-02-16  오후 08:38    <DIR>          public
        1985-10-26  오후 05:15             2,881 README.md
        2019-02-16  오후 08:38    <DIR>          src
    ```
    + **.gitnore** : git에 업로드 할 때, 레파지토리에서 제외시킬 파일과 폴더 목록이 들어있다. 의존성 폴더 자체를 올리지 않고 `pakage.json` 파일만으로 설치할 수 있으므로 굳이 전부 올릴 필요 없다.
    + **node_modules** : `npm`으로 설치한 모든 노드 패키지를 가지고있다. 웬만하면 안 건들이는게 좋음.
    + **pakage.json** : 노드 패키지 의존성 및 기타 프로젝트의 구성목록. `version range` 를 사용해서 버전의 범위를 설정한다.
    + **pakage-lock.json** : `node_modules``pakage.json`을 참고해 `node_modules` 에 노드 패키지들을 설치하는데, 만약 배포된 패키지들이 업데이트 되거나 하면 새로운 버전을 설치하게 된다. 간혹 업데이트 버전이 오류를 발생시키기도 하는데, 이때 파일이 작성된 시점의 의존성 트리가 다시 생성될 수 있도록 보장한다.
