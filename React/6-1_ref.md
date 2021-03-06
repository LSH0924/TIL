# ref(reference)
- HTML에서 사용하는 id와 비슷하게, 리액트 프로젝트 내부에서 DOM에 이름을 다는 방법이다.
- 리액트 컴포넌트에는 특수한 경우를 제외하고는 id 사용을 권장하지 않는다. 같은 컴포넌트가 몇개이고 생기면 id 또한 중복될 수 있기 때문.
- ref 는 전역적으로 작동하지 않고 컴포넌트 내부에서만 작동한다.
- DOM 을 직접 건들이는 작업을 해야 할 때 사용한다.
    - 특정 input 에 포커스를 줄 때
    - 스크롤 박스 조작
    - 캔버스에 그림 그리기 등

## ref 사용하기
- props 를 설정할 때와 마찬가지로 컴포넌트의 요소 ref 안에 **콜백함수**를 전달한다. 콜백함수의 내부에서는 컴포넌트의 내부 변수에 ref를 담는 코드를 작성한다.
- 컴포넌트에서 ref를 사용할 때에도 마찬가지.

```javascript
// this.input 은 input Element 의 DOM 을 가리킨다.
<input ref = {r => {this.inputName = r}}></input>
```
