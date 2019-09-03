# MongoDB
- 기존 관계형 데이터 베이스의 한계를 극복한 문서(Document) 지향적 NoSQL 데이터베이스
- 관계형 데이터 베이스의 한계
    - 고정된 데이터 스키마 : 새로 등록하는 데이터의 형식이 기존 데이터 형식과 다르면, 이전 데이터를 전부 수정하고 나서야 새 형식의 새 데이터를 집어넣을 수 있다. (번거로움)
    - 확장성 : 처리해야 할 데이터의 양이 늘어나면 여러 컴퓨터에 분산시키는 것이 아닌, DB 서버의 성능을 업그레이드 하는 방식으로 확장할 수 밖에 없다.
- 유동적인 스키마를 가진다.
- 서버에서 처리해야 할 데이터 양이 늘어나면 여러 컴퓨터로 분산 처리 할 수 있도록 확장하기 쉽게 설계되어 있다.
- 서버 하나에 여러 개의 DB를 가지고 있을 수 있으며, 각 DB 안에 컬렉션을 여러 개 가지고 있다. 컬렉션 안에는 문서들이 들어있는 구조

## 설치
- 이제 MongoDB는 Windows에 따로 설치하지 않고도 클라우드로 이용할 수 있다.
- npm 혹은 yarn 으로 mongodb를 프로젝트에 추가
    ```s
    # npm add mongodb
    $ yarn add mongodb
    ```
- [https://cloud.mongodb.com/](https://cloud.mongodb.com/) 여기로 들어가 가입 후 클러스터 생성
- 클러스터를 만들고 사용자를 등록한 뒤 프로젝트와 데이터베이스를 연결하는 코드를 집어넣는다.
    ```javascript
    const MongoClient = require('mongodb').MongoClient;
    const uri = "mongodb+srv://데이터베이스유저이름:유저 비밀번호@클러스터이름-rduj9.mongodb.net/test?retryWrites=true&w=majority";
    const client = new MongoClient(uri, { useNewUrlParser: true });
    client.connect(err => {
        const collection = client.db("test").collection("devices");
        // perform actions on the collection object
        client.close();
    });
    ```
- 주의점 : 개발환경이 유동 아이피를 사용할 경우는 Network Access에서 모든 액세스 허용하기를 설정해두도록 하자.

## Document(문서)
- 관계형 데이터베이스의 레코드와 비슷한 개념
- 한 개 이상의 key-value 쌍으로 이루어져 있다.
    ```javascript
    {
        "_id": ObjectId("1523a186f41a86c85asd"),
        "username": "LSH",
        "name": {
            firts: "SH", last: "L"
        }
    }
    ```
- **BSON**(바이너리 형태의 JSON) 타입으로 저장
- JSON 형태의 객체를 데이터베이스에 저장할 때, 큰 공수를 들이지 않고도 데이터를 데이터베이스에 등록할 수 있어 편하다.
- `"_id"` : 시간과 머신 아이디, 프로세스 아이디, 순차 번호로 되어 있는 고유값. 문서의 식별자로 사용한다.
- 문서 내부에 또 다른 문서를 넣을 수 있는데, 이를 서브다큐먼트라고 한다.
- 서브 다큐먼트 또한 일반 문서를 다룰 때 처럼 쿼리할 수 있다.
- 문서 하나에 들어가는 최대 데이터 양 : 16MB

## Collection(컬렉션)
- 문서의 집합
- 관계형 데이터베이스의 테이블과 비슷한 개념
- 다른 스키마를 가지고 있는 문서들이 한 컬렉션 안에서 공존할 수 있다.
    ```javascript
    {
        "_id": ObjectId("1523a186f41a86c85asd"),
        "username": "LSH",
        "name": {
            firts: "SH", last: "L"
        }
    },
    {
        "_id": ObjectId("18561fasd5f88s5f5asd"),
        "username": "LSH2",
        "age": 100
    }
    ```