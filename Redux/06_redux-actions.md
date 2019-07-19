# redux-actions

리덕스 액션들을 관리할 때 유용한 `createAction`, `handleAction` 함수를 제공한다.

## 설치

```s
$ yarn add redux-actions
```

## createAction

- 액션 생성 함수를 간단하게 만들어준다.
- 생성된 함수를 호출할 때 함수에 파라미터를 넣어 호출하면 `payload` 키에 파라미터로 받은 값을 넣은 객체를 반환한다.
- 파라미터를 여러 개 넣고 싶다면 객체를 만들어 넣어주거나, `payload`를 작성하는 화살표 함수를 넣어준다.

## handleAction

- 리듀서에서 사용했던 `switch`문 대신 사용한다.
  - switch 문을 사용하면 scope가 리듀서 함수가 되기 때문에, 서로 다른 case에서 같은 이름의 let, const를 사용할 수 없다.
- handleAction 의 첫번째 파라미터는 액션에 따라 실행할 함수들을 가진 객체, 두번째 파라미터는 initialState이다.
