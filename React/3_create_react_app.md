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
