#mongoose
- Node.js 환경에서 사용하는 MongoDB 기반 ODM(Object Ddata Modelling) 라이브러리
- 데이터베이스의 Document들을 자바스크립트 객체처럼 사용할 수 있게 한다.
- dotenv : 환경변수들을 파일에 넣고 사용할 수 있게 하는 개발도구
    - 환경변수는 process.env 파일로 조회할 수 있다.
- 설치
    ```s
    $ yarn add mongoose dotenv
    ```
- mongoose에서 데이터베이스를 요청할 때, Promise 기반으로 처리할 수 있다. 이때 어떤 형식의 Promise를 사용할 지 정해야 한다.(Node v7 이후부터는 내장 Promise 를 사용)

## 스키마와 모델
- 스키마 : 컬렉션에 들어가는 문서 내부의 각 필드가 어떤 형식으로 되어 있는지 정의하는 객체
- 모델 : 스키마를 사용해 만드는 인스턴스. 데이터베이스에서 실제 작업을 처리할 수 있는 함수들을 지니고 있는 객체

### 스키마 생성
- mongoose 모듈의 `Schema` 사용
- 문서 내에 들어갈 필드의 이름, 데이터 타입 정보가 들어 있는 객체를 만든다.
- 필드의 기본 값을 설정해주고 싶을 때는 default 를 설정한다.
- 스키마의 기본 지원 타입

    |타입|설명|
    |-----|-----|
    |String|문자열|
    |Number|숫자|
    |Date|날짜|
    |Buffer|파일을 담을 수 있는 버퍼|
    |Boolean|true, false|
    |Mixed(Schema.Types.Mixed)|어떤 데이터도 넣을 수 있는 형식|
    |ObjectId(Schema.Types.ObjectId)|객체 아이디, 주로 다른 객체를 참조할 때 넣는다|
    |Array|배열 형태의 값. []로 감싸서 사용|

### 모델 생성
- mongoose 의 `model(스키마 이름, 스키마 객체)` 함수를 사용
- 스키마 이름을 지정해주면 데이터베이스가 스키마 이름의 복수형태로 컬렉션을 생성
- 세 번째 파라미터로 컬렉션의 이름을 지정해줄 수도 있다.
- 첫 번째 파라미터는 나중에 다른 스키마에서 현재 스키마를 참조해야 하는 상황에서 사용한다.

#### 모델의 메소드
- 모델.save() : 모델의 인스턴스 안에 들어있는 데이터 객체(인스턴스를 만들 때 ctx.request.body를 생성자에 집어넣는다.)를 데이터베이스에 등록한다. 등록이 완료되면 등록된 Document를 반환한다.
- 모델.find() : 데이터베이스 컬렉션 안에 들어있는 모든 데이터를 찾아 반환한다. find~() 종류의 함수 뒤에는 exec() 를 붙여주어야 검색 쿼리를 실행한다. 옵션을 붙여서 검색에 옵션을 설정할 수 있다.
- 모델.findById(id) : 데이터베이스 컬렉션 내부에서 매개변수로 지정된 id와 일치하는 Document를 검색해 반환한다.
- 모델.remove(OPTION) : 데이터베이스 컬렉션 내부에서 파라미터로 지정한 조건과 일치하는 Document를 찾아 반환한다.
- 모델.findByIdAndRemove(id) : 데이터베이스 컬렉션 내부에서 파라미터로 지정한 아이디와 일치하는 Document를 찾아 삭제한다.
- 모델.findByOneAndRemove(OPTION) : 데이터베이스 컬렉션 내부에서 파라미터로 지정한 조건과 일치하는 Document를 찾아 삭제한다.
- 모델.findByIdAndUpdate(id, {업데이트하고싶은 데이터를 가진 객체}, 반환 데이터 상태) : 데이터베이스 컬렉션 내부에서 첫 번째 파라미터로 지정한 아이디와 일치하는 Document를 찾아 두 번째 파라미터로 지정한 객체의 데이터로 업데이트 한다. 세 번째 파라미터로 메소드의 반환 데이터를 지정할 수 있다.

### 요청 검증
- ObjectId 검증
    - mongoose 라이브러리의 ObjectId 모듈을 이용한 아이디(_id : 문서의 아이디)의 형식 검증
    - 미들웨어 만들어서 ObjectId 검증하기 : src/api/posts/posts.ctrl.js
    ```javascript
    ...
    /**
     * 요청할 때 ID 검증하기
    */
    exports.checkObjectId = (ctx, next) => {
        const {id} = ctx.params;
        if(!ObjectId.isValid(id)){
            ctx.status = 400; // 잘못된 요청을 받으면 400 오류를 발생시킨다.
            return null;
        }
        return next();
    };
    ...

    ```