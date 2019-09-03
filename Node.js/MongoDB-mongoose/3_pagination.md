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

