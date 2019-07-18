# Redux 04 - Immutable.js 와 Immer.js : 데이터 불변성 손쉽게 관리하기

## Immutable.js

[![Edit 0516](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/0516-2kxz4?fontsize=14)

- 불변성을 유지해주어야 하는 객체의 값을 더 쉽게 업데이트 할 수 있다.
- 설치
  ```s
  $ yarn add immutable
  ```
### Immutable.js 사용하기

#### Map
- Map 함수 안에 객체를 넣어 호출한다.
- 객체 내부에 또 다른 객체들이 있다면 내부 객체들도 Map으로 감싸주어야 나중에 사용하기 편하다.(나중에 setIn, getIn을 사용할 수 있음)
- 객체의 내용을 네트워크에서 받아오거나, 전달받는 객체가 너무 복잡하다면 `fromJS`를 사용한다. `fromJS`를 사용하면 내부에 있는 객체는 Map으로 감싸주지 않아도 됨.
  ```javascript
  const { Map, fromJS } = Immutable;

  const data = Map({
    a: 1,
    b: 2,
    c: Map({
      c_1: 0,
      c_2: 1
    })
  });

  const data2 = fromJS({
    a: 1,
    b: 2,
    c: {
      c_1: 123,
      c_2: 456
    }
  });

  // Immutable 객체를 일반 객체로 변환하기
  const des = data.toJS();
  console.log(des); // Object {a: 1, b: 2, c: Object}
 
  // 특정 키의 값 불러오기
  console.log(data.get("a")); // 1

  // 깊이 있는 값 가져오기
  console.log(data.getIn(["c", "c_1"])); // 0

  // 값 설정하기 : 데이터가 실제로 변하는것이 아니라 주어진 변화를 적용한 새 Map을 만들어 반환한다.
  const newData = data.set("a", 1234567890);
  console.log(newData === data); // false

  // 깊숙히 있는 값 수정하기
  const newData2 = data.setIn(["c", "c_2"], "99999999");
  console.log(newData2.get("c").toJS()); // Object {c_1: 0, c_2: "99999999"}

  // 여러 값 동시에 설정하기(성능상으로는 set(), setIn() 을 여러번 하는게 조금 더 빠르다)
  const newData3 = data.merge({a:10, b:20});
  const newData4 = data.mergeIn(["c"],{c_1:10, c_2:20});
  ```

### List
- 배열 대신 사용
- 배열과 동일하게 map, filter, sort, push, pop 등의 함수를 내장하고 있다.
- 위 내장함수를 사용하면 기존 List를 변경하는 것이 아닌, 변경된 새로운 List 를 반환한다.
  ```javascript
  const {List, Map, fromJS} = Immutable;

  const list = List([
    Map({value : 1}), Map({value : 2}), 3, 4
  ]);

  const list2 = fromJS([
    {value : 1}, {value : 2}, 3, 4
  ]);

  // List 의 요소를 가져오거나 설정하는 방법은 Map 을 사용할때와 같다.
  console.log(list.get(0).toJS()); // Object {value: 1}
  console.log(list.getIn([0, "value"])); // 1
  console.log(list.set(0, Map({value : 13579})) === list); // false
  console.log(list.setIn([0, "value"], 246810).toJS()); // fe {size: 4, _origin: 0, _capacity: 4, _level: 5, _root: null…}
  // 기존 값을 참조하면서 값을 설정하고싶을 때는 update 를 사용한다.
  // 첫번째 파라미터는 선택할 인덱스 값, 두번째 파라미터는 선택한 원소를 업데이트 하는 함수.
  const newList = list.update(0, item => item.set("value", item.get("value")*5));
  console.log(newList.toJS()); // [Object{value: 5}, Object{value: 2}, 3, 4]

  // 요소 추가하기 : 맨 뒤
  const newList2 = list.push(5);
  console.log(newList2.toJS()); // [Object, Object, 3, 4, 5]
  // 요소 추가하기 : 맨 앞
  const newList3 = list.unshift(0);
  console.log(newList3.toJS()); // [0, Object, Object, 3, 4]

  // 요소 제거하기
  // 파라미터로 제거하고싶은 인덱스를 넣는다.
  const newList4 = list.delete(0);
  console.log(newList4.toJS()); // [Object{value: 2}, 3, 4]
  // 마지막 아이템을 제거하고싶을땐 pop
  const newList5 = list.pop();
  console.log(newList5.toJS()); // [Object, Object, 3]

  // 비어있는지 확인하려면
  console.log(list.isEmpty()); // false
  ```

### 적용

- immutable.js 를 설치 후, 원래의 state 객체 등을 Map 으로 바꿔준다.

  - src/store/module/counter.js

  ```javascript
  import { Map } from "immutable";
  ...
  // 초기값 설정하기
  // -> initialState 를 Map 으로 설정
  const initialState = Map({
    color: "red",
    number: 0,
    diff: 1
  });

  // 리듀서 작성
  // export default : 단일 값을 내보내거나 모듈의 반환값을 사용하고자 할 때 명시한다.
  // -> state를 설정할 때는 set, 가져올 때는 get 을 사용
  export default function counter(state = initialState, action) {
    switch (action.type) {
      case CHANGE_COLOR:
        // 기존 코드
        // return {
        //   ...state,
        //   color: action.color,
        //   number: 0,
        //   diff: action.diff
        // };
        return state
          .set("color", action.color)
          .set("number", 0)
          .set("diff", action.diff);
      case INCREMENT:
        // 기존 코드
        // return { ...state, number: state.number + state.diff };
        return state.update(
          "number",
          () => state.get("number") + state.get("diff")
        );
      case DECREMENT:
        return state.update("number", () =>
          state.number > 0 ? state.number - 1 : state.number
        );
        // 기존 코드
        // const count = state.number > 0 ? state.number - 1 : state.number;
        // return { ...state, number: count };
      default:
        return state;
    }
  }
  ```

  - waiting.js

  ```javascript
  import { List, Map } from "immutable";
  ...
  // 초기상태 정의하기
  const initialState = Map({
    input: "",
    list: List([
      Map({
        id: 0,
        name: "대기자1",
        entered: false
      })
    ])
  });

  export default handleActions(
    {
      [CHANGE_INPUT]: (state, action) => state.set("input", action.payload),
      // 기존 코드
      // ({
      //   ...state,
      //   input: action.payload
      // })
      [CREATE]: (state, action) =>
        state.set("input", "").update("list", list =>
          list.push(
            Map({
              id: action.payload.id,
              name: action.payload.text,
              entered: false
            })
          );
        ),
      // 기존 코드
      // ({
      //   ...state,
      //   list: state.list.concat({
      //     id: action.payload.id,
      //     name: action.payload.text,
      //     entered: false
      //   }),
      //   input: ""
      // })
      [ENTER]: (state, action) => {
        // 액션으로 받은 ID 와 리스트 안의 ID 가 같은 인덱스를 찾는다.
        const index = state
          .get("list")
          .findIndex(item => item.get(id) === action.payload);
        // setIn 으로 state 안쪽에 파고들어서 entered 값만 반전시킴
        return state.updateIn(["list", index, "entered"], entered => !entered);
      },
      // 기존 코드
      // ({
      //   ...state,
      //   list: state.list.map(item =>
      //     item.id === action.payload ?
      // { ...item, entered: !item.entered } : item
      //   )
      // })
      [LEAVE]: (state, action) => {
        // 액션으로 받은 ID 와 리스트 안의 ID 가 같은 인덱스를 찾는다.
        const index = state
          .get("list")
          .findIndex(item => item.get(id) === action.payload);
        //deleteIn 으로 state 안의 list 의 index 번째 요소를 삭제한다.
        return state.deleteIn(["list", index]);
      }
      // 기존 코드
      // ({
      //   ...state,
      //   list: state.list.filter(item => item.id !== action.payload)
      // })
    },
    initialState
  );
  ```

  - 바뀐 데이터 형식에 맞게 각 Container 또한 바꿔준다.
  - CounterContainer.js

  ```javascript
  ...
  //counter 객체를 받아 props용 객체 설정
  // -> get 을 사용해서 초기값을 가져오도록 수정
  const stateToProps = ({ counter }) => ({
    number: counter.get("number"),
    color: counter.get("color")
  });
  ...
  ```

  - PaletteContainer.js

  ```javascript
  ...
  //props 에 들어갈 스토어 상태값 : 색깔
  const stateToProps = state => ({
    color: state.counter.get("color")
  });
  ...
  ```

  - WaitingListContainer.js

  ```javascript
  ...
  // waiting 의 초기값을 가져와 props 로 설정해주기
  const stateToProps = ({ waiting }) => ({
    input: waiting.get("input"),
    list: waiting.get("list")
  });
  ...
  ```

  - WaitingList.js

  ```javascript
  ...
  const WaitingList = ({
  waitingList,
  onEnter,
  onLeave,
  onSubmit,
  onChange,
  input
  }) => {
    // list 받아서 컴포넌트로 재구성하기
    const waitingPeople = waitingList.map(wait => (
      <WaitingItem
        key={wait.get("id")}
        text={wait.get("name")}
        entered={wait.get("entered")}
        onEnter={() => onEnter(wait.get("id"))}
        onLeave={() => onLeave(wait.get("id"))}
      />
    ));

    const style = {
      width: "70%"
    };
  ...
  ```

- 장점

  - state 의 객체 구조가 매우 복잡할 때 유용하다.
  - 새 state 를 설정할 때 `대상.setIn(["대상 안의 대상키", "특정할수있는 대상정보", "대상정보2"], "새 값")` 과 같이 간단하게 처리할 수 있다.

- 단점
  - 번거로움 : 데이터를 불러와 값을 조회할 때 일일히 `대상.get("대상특정")` 과 같이 사용해야한다.

## Immer.js

[![Edit colorful-counter](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/colorfulcounter-8tvlu?fontsize=14)

- 설치

```s
$ yarn add immer
```

- 이전에 사용했던 Immutable.js 는 비활성화 해준다.
- immer의 produce를 import 한 뒤, produce 함수 안에서 불변성을 신경쓰지 않았던 옛날로 돌아가 업데이트를 해주면 라이브러라가 알아서 유지해준다.
- produce 함수의 첫번째 파라미터는 BaseState, 두번째 파라미터는 NextState 에 반영될 객체이다.
- immer.js 를 적용시킨 뒤 코드 다시 수정하기

  - src/store/modules/counter.js

  ```javascript
  import produce from "immer";
  ...
  export default function counter(state = initialState, action) {
    switch (action.type) {
      case CHANGE_COLOR:
        // return {
        //   ...state,
        //   color: action.color,
        //   number: 0,
        //   diff: action.diff
        // };
        return produce(state, draft => {
          draft.color = action.color;
          draft.number = 0;
          draft.diff = action.diff;
        });
      case INCREMENT:
        // return { ...state, number: state.number + state.diff };
        return produce(state, draft => {
          draft.number = state.number + state.diff;
        });
      case DECREMENT:
        // const count = state.number > 0 ? state.number - 1 : state.number;
        // return { ...state, number: count };
        return produce(state, draft => {
          draft.number = state.number > 0 ? state.number - 1 : state.number;
        });
      default:
        return state;
    }
  }
  ```

  - src/store/modules/waiting.js

  ```javascript
  import produce from "immer";
  ...
  export default handleActions(
    {
      [CHANGE_INPUT]: (state, action) =>
        produce(state, draft => {
          draft.input = action.payload;
        }),
      // ({
      //   ...state,
      //   input: action.payload
      // })
      [CREATE]: (state, action) =>
        produce(state, draft => {
          draft.list.push({
            id: action.payload.id,
            name: action.payload.text,
            entered: false
          });
          draft.input = "";
        }),

      // ({
      //   ...state,
      //   list: state.list.concat({
      //     id: action.payload.id,
      //     name: action.payload.text,
      //     entered: false
      //   })
      //   ,
      //   input: ""
      // })
      [ENTER]: (state, action) =>
        produce(state, draft => {
          draft.list.map(item =>
            item.id === action.payload
              ? { ...item, entered: !item.entered }
              : item
          );
        }),
      // ({
      //   ...state,
      //   list: state.list.map(item =>
      //     item.id === action.payload ? { ...item, entered: !item.entered } : item
      //   )
      // })
      [LEAVE]: (state, action) =>
        produce(state, draft => {
          draft.list.filter(item => item.id !== action.payload);
        })
      // ({
      //   ...state,
      //   list: state.list.filter(item => item.id !== action.payload)
      // })
    },
    initialState
  );
  ```

- 장점
  - 일반적인 자바 스크립트 객체 및 배열과 같으므로 새로운 API를 배울 필요가 없다.
  - 상용구, 수식어가 감소해 노이즈가 적고 코드가 간결하다.
  - 가볍다.

---

## 참고

github - immerjs/immer [https://github.com/immerjs/immer](https://github.com/immerjs/immer)
<br>
velog - Redux (4) Immutable.js 혹은 Immer.js 를 사용한 더 쉬운 불변성 관리 [https://velog.io/@velopert/20180908-1909-작성됨-etjltaigd1#4-2.-immer-로-불변성-유지하기](https://velog.io/@velopert/20180908-1909-작성됨-etjltaigd1#4-2.-immer-로-불변성-유지하기)
