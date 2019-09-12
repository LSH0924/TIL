# Pagination
- 한 페이지 당 표시할 데이터의 개수 등을 설정
- mongoose-pagination 라이브러리를 사용해 간편하게 구현할 수 있다.
- sort() : 데이터 정렬
    > Model.find().sort({keyName: -1}).exec();
    - sort()의 파라미터에 넣는 객체는 정렬 기준을 설정한다.
    - keyName 은 정렬의 기준이 되는 키 이름이며, value를 1로 설정하면 오름차순 정렬, -1로 설정하면 내림차순 정렬을 뜻한다.

- limit() : 표시 개수 제한
    > Model.find().limit(숫자).exec();
    - limit()의 파라미터에 들어가는 숫자만큼 표시

- skip() : 페이지 구현
    > Model.find().limit(숫자).skip(숫자).exec();
    - skip()의 파라미터에 들어가는 숫자만큼 건너뛴 다음 데이터를 검색한다.

- 마지막 페이지 번호 알려주기
    - Response 헤더의 Link를 설정하거나, 커스텀 헤더를 설정한다.
    - `ctx.set()`을 이용하면 커스텀헤더를 설정할 수 있다.
    - 책에서는 count()를 사용했지만 Model.count()는 deprecated 되었으므로, Model.countDocuments() 나 Model.estimatedDocumentCount()를 사용한다.

- 미리보기 내용 제한
    - mongoose 를 이용해 조회한 데이터들은 mongoose의 문서 인스턴스로 return 되기 때문에 `.map()` 등으로 작업할 때 쓸데없는 정보까지 들어갈 수 있다.
    - mongoose의 문서 인스턴스가 아닌 JSON 형태로 return 받기 위해 조회할 때 `lean()`을 사용한다.
    - 혹은, `.toJSON()`을 이용해 mongoose의 문서 인스턴스를 JSON으로 형변환시킨다.
