# NPM(Node Package Manager)

- NPMSearch 에엣에 탐색할 수 있는 Node.js의 패키지, 모듈 저장소
- Node.js 패키지 설치 및 버전, 호환성 관리를 할 수 있는 커맨드 라인 유틸리티

### 구버전 npm 업데이트 시키기

```terminal
sudo npm install npm -g
```

### 글로벌 모듈설치와 로컬 모듈 설치

- 일반적으로 npm모듈은 로컬모드로 설치한다.
- 로컬 모듈 설치

  - npm모듈 설치 명령을 실행한 디렉토리의 node_modules 안에 패키지를 설치하는 것
  - 어플리케이션 내부에서 바로 require()를 사용해 설치한 모듈을 불러올 수 있다.

- 글로벌 모듈 설치

  - npm모듈을 시스템 디랙토리(/usr/lib/node_modules)에 설치하는 것
  - 시스템 계정을 사용하지 않고 있다면, 설치 명령 앞에 `sudo` 를 붙여 주어야 한다.
  - 어플리케이션 내부에서 바로 require()로 불러올 수 없다.<br/>글로벌 모드로 설치한 모듈을 불러오기 위해서 `npm link` 명령어를 사용한다.

    ```terminal
    npm install -g express  // npm module 글로벌 설치
    cd [프로젝트 디렉토리 path]  // 프로젝트 디렉토리로 이동
    npm link express  // 글로벌 설치 한 module를 프로젝트와 연결
    ```

  - `npm link` 실행 후 어플리케이션 내부에서 require()을 사용해 모듈을 불러올 수 있음

### 모듈 제거하기

```terminal
npm uninstall 패키지이름
```

### 모듈 업데이트하기

```terminal
npm update 패키지이름
```

### 모듈 검색하기

```terminal
npm search 패키지이름
```
