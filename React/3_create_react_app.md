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
    
  - 부트스트래핑 후 생성된 디렉토리 안의 내용물
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
    - **.gitnore** : git에 업로드 할 때, 레파지토리에서 제외시킬 파일과 폴더 목록이 들어있다. 의존성 폴더 자체를 올리지 않고 `pakage.json` 파일만으로 설치할 수 있으므로 굳이 전부 올릴 필요 없다.
    - **node_modules** : `npm`으로 설치한 모든 노드 패키지를 가지고있다. 웬만하면 안 건들이는게 좋음.
    - **pakage.json** : 노드 패키지 의존성 및 기타 프로젝트의 구성목록. `version range` 를 사용해서 버전의 범위를 설정한다.
    - **pakage.json**
      - npm 으로 노드 패키지를 하나씩 설치하면, pakage.json에 이름과 버전정보가 추가된다.
      - 이 때 ^1.2.3 처럼 ^(caret)을 포함한 [시멘틱 버전](https://github.com/LSH0924/TIL/blob/master/React/3-1_sementic_versioning.md/)으로 표시된다.
        - Major 버전<sub>지금 예시에서는 1</sub>이 같을 경우에 한해 1.2.3 버전 이상의 모든 마이너패치를 지원한다는 의미이다.
    - **pakage-lock.json**
      - npm version 5.1.X 부터 제공
      - pakage.json 만으로는 다른 버전의 라이브러리를 마음대로 설치해버릴 가능성이 있는데, pakage-lock.json은 이런 경우를 막기 위해 존재한다.
      - 버전을 엄격하게 특정한 뒤, 통합 해시를 전체 모듈 혹은 각각의 의존처에 뿌림으로써 인스톨 할 때마다 완전히 같은 환경을 구축할 수 있다.
    - **public/**
      - 배포를 위한 프로젝트를 빌드할 때 필요한 모든 파일이 들어있다.
      - 프로젝트를 빌드할 때, **src/** 안의 모든 코드는 몇개의 파일로 묶여 **public/** 속에 배치된다.
    - **README.md**
      - 프로젝트 부트스트래핑 시 자동생성
      - 프로젝트에 관한 설명
    - **src/**
      - App.js : 리액트 컴포넌트를 관리하는 파일. 컴포넌트를 여러 개의 파일로 분리해서 관리할 수 있지만, 기본으로는 하나만 생성된다.
      - index.js : 리액트 진입점