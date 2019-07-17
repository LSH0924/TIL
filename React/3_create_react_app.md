# create-react-app
  
  - 2016년 페이스북이 제안한 리액트 제로 구성 설치 스타터킷(Zero-Configuration Setup Starter Kit)
  - create-react-app을 통해 어플리케이션을 부트스트래핑 할 수 있다.
    + 부트스트래핑(bootstrapping) : 어플리케이션을 최초 생성 후 브라우저에서 실행하는 과정
  - 이미 리액트 개발도구와 환경설정이 다 되어 있음
  - 생성하는 어플리케이션의 이름은 대문자를 포함할 수 없다.

### npm start

- 브라우저에서 에러 메시지가 출력된 영역을 클릭하면 에러가 발생한 파일이 에디터에서 열린다.
- `https` 실행옵션을 제공한다.
  ```s
  # Mac os
  $ HTTPS= true npm start
  # windows
  $ set HTTPS= true && npm start
  ```
  - 자체 서명된 인증서와 함께 https사이트로 접속할 수 있다.

### npm build

- 배포 환경에서 사용할 파일을 만들어준다.
- 빌드 후 생성된 정적 `js`파일과 `css`파일은 사람이 읽기 힘든 형식으로 압축되어있다.

## npm test

- create-react-app 이 테스트파일로 인식하는 `js`파일
  - `__test__` 폴더 밑에 있는 모든 자바스크립트 파일
  - 파일 이름이 `.test.js` 나 `.spec.js` 로 끝나는 파일

## 전역 노드 패키지에 create-react-app 패키지 설치하기

    ```s
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
    
## 부트스트래핑 후 생성된 디렉토리 안의 내용물
    ```s
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
    - index.js 에 연결하는 모든 `js`, `css` 파일들은 `src` 폴더 밑에 있어야 한다. `src` 폴더 밖에 있는 파일들을 참조하려고 해도 실패함.
    - `js`, `css` 파일들은 빌드시 웹팩이 자동으로 압축시켜주기 때문에 `src` 폴더 아래 넣어두는게 좋다.
    - 이미지 파일이나 폰트 파일도 `src` 폴더 아래에 넣어서 `import` 키워드를 사용해 참조하는게 좋다. 웹팩에서 해시값을 이용해 url을 생성해주기 때문에 파일의 내용이 변경되지 않으면 브라우저 캐싱 효과를 볼 수 있기 때문.
  - **serviceWorker.js**
    - PWA(Progressive web app : 오프라인에서도 잘 동작하는 웹 애플리케이션을 만들기 위한 기술)와 관련된 코드가 들어있다.
    - PWA를 사용하고 싶다면 `index.js` 에 `serviceWorker.register();` 코드를 넣으면 된다.

## create-react-app v2.1.3 에서 지원하는 ES6기능

- 지수(제곱) 연산자(exponentiation operator) `**`
- async await 함수
- 나머지 연산자(rest operator), 전개 연산자(spred operator)
- 동적 임포트(dynamic import)
- 클래스 필드(class field)
- JSX
- type script(자바스크립트의 정적 타입 시스템), flow type system
- 템플릿 리터럴(tagged template literals) 등
- 단축 속성명(shorted property names)

  - 새로 만들고자 하는 객체의 속성값 일부가 이미 변수로 존재하고 있을때, 변수 이름만 적어주면 속성명, 속성값을 자동으로 설정해준다.
  - 속성값이 함수이면 function 키워드 없이 함수명만 적어도 된다. 속성명은 함수이름과 같아짐

  ```javascript
  const fruits = "복숭아";
  const obj = { fruits };
  console.log(obj);
  // Object {fruits: "복숭아"}

  // 디버깅을 위해 콘솔에서 로그를 출력할때도 매우 유용하다
  const box = 3;
  // 값을 확인하기위해 이렇게 입력했어야 했다..
  console.log("fruits=" + fruits + "\nbox=" + box);
  // fruits=복숭아
  // box=3

  // ES6문법을 사용하면 매우 간편하게 확인가능!
  console.log({ fruits, box });
  // Object {fruits: "복숭아", box: 3}
  ```

- 계산된 속성명(computed property names)

  - 객체의 속성명을 동적으로 결정한다.

  ```javascript
  function makeObj(key, value) {
    // 계산된 속성명을 이용해 더 간단하게 객체를 설정할 수 있음
    return { [key]: value };
  }
  ```

- 비구조화 할당으로 변수의 기본값을 정의할 때 배열의 속성값이 undefined라면 정의된 기본값이 할당되고, 아닐 경우 원래 있는 속성값이 할당된다.

```javascript
const arr = [0];
// 여기서 변수 a, b 생성
const [a = 100, b = 200] = arr;
console.log(a); // 0
console.log(b); // 200
```

- 두 변수의 값을 쉽게 교환할수도 있다.

```javascript
let a = 0;
let b = 1;
[a, b] = [b, a];
console.log(a); // 1
console.log(b); // 0
```

- 배열의 경우, 일부 속성값을 무시하고 진행하고 싶다면 건너뛰는 개수만큼 쉼표로 표시해준다.

- 객체의 비구조화에서는 속성명과 다른 이름으로 변수를 생성할 수 있다. 이는 중복된 변수명을 피하거나, 좀 더 구체적인 변수명을 만들 때 좋다.

```javascript
const obj = { animal: "cat", name: "kkong-nyang" };
const { animal: theAnimal, name } = obj;
// console.log(animal); // animal is not defined : 참조에러
console.log(theAnimal); // cat
```

- 비구조화로 변수에 할당할 때, 함수의 반환값을 기본값으로 설정할 수 있다.
- for 문에서 객체를 원소로 갖는 배열을 순회할 때 객체 비구조화를 사용하면 편하다.

## 알아두기 : 폴리필(polyfill)

- 개발자가 특정 기능이 지원되지 않는 브라우저를 위해 사용할 수 있는 코드 조각이나 플러그인
- 각기 다른 브라우저에서도 같은 기능을 할 수 있도록 한다.
- 실행 시점에서 주입하고자 하는 객체나 함수가 현재 환경에서 존재하는지 검사한 뒤, 그 기능이 없을때만 주입한다.
- create-react-app 에서는 아무런 폴리필도 포함되지 않는다. 사용하고싶으면 직접 주입해야함.