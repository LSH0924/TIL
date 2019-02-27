# REPL(Read Eval Print Loop)

- 윈도우 커멘드나 UNIX/LINUX shell처럼 사용자가 커멘드를 입력하면 시스템이 값을 반환하는 환경
- Node.js는 REPL 환경과 함께 제공된다.
- 대화형 환경<sub>스칼라, 파이썬 등에서도 제공한다.</sub>
- 자바스크립트 코드를 테스팅, 디버깅 할 때 유용하다.

- 기능

  - **Read** : 사용자가 입력한 값을 javascript데이터 구조로 변환해 메모리에 저장한다.
  - **Eval(Evaluate)** : 데이터를 처리한다.
  - **Print** : 결과값 출력
  - **Loop** : Read, Eval, Print 를 유저가 Ctrl+C를 두번 눌러 종료할때까지 반복

- 쉘이나 콘솔에 파라미터 없이 `node`를 입력해 사용할 수 있다.
- 간단한 표현식, 연산자 사용하기
  ```terminal
  $ node
  > 10+20
  30
  > 1+(2*3)/4*5
  8.5
  ```
- 변수 사용하기
  ```terminal
  $ node
  > x=100
  100
  > var y = 12
  undefined
  > x+y
  112
  > console.log("x + y = " + (x + y))
  x + y = 112
  undefined // console.log() 를 실행하면 undefined 가 출력. console.log() 자체가 변수가 아니라 가지고 있는 값이 없어서?
  ```
- 여러 줄 표현식(multiLine)
  - 여러 줄의 계산, 표현식을 REPL 환경에서 사용할 수 있다.
  ```terminal
  $ node
  > var num = 1;
  undefined
  > do{
  ... console.log("var num is " + num++);
  ... }while(num < 5);
  var num is 1
  var num is 2
  var num is 3
  var num is 4
  undefined  // console.log()에 의한 undefined
  > console.log(num)
  5
  undefined
  ```
- Underscore(\_) 변수

  - 언더스코어 변수는 가장 최근의 결과값을 가리킨다.

  ```terminal
  $ node                                                                   s.
  > str ="this is string";
  'this is string'
  > str2 = " this is String 2."
  ' this is String 2.'
  > str + str2
  'this is string this is String 2.'
  > strings = _
  'this is string this is String 2.'
  > console.log(strings)
  this is string this is String 2.
  undefined
  ```

## REPL Command

- **Ctrl+C** : 현재 명령어 종료
- **Ctrl+C 두번** : Node REPL모드를 종료
- **Ctrl+D** : Node REPL모드를 종료
- **↑ / ↓** : 명령어 히스토리 탐색
- **Tab** : 현재 입력란에 쓴 값으로 시작하는 명령어나 변수 목록 확인
- **.help** : Node REPL의 모든 커맨드 목록 확인
- **.break** : 멀티 라인 표현식 입력 종료
- **.clear** : .break 와 같음
- **.save filename** : 현재 Node REPL 세션을 파일로 저장
- **.load filename** : Node REPL 세션을 파일에서 가져옴

---

- https://velopert.com/235 [ [Node.JS] 강좌 04편: REPL 터미널 ]
- https://www.katacoda.com/courses/nodejs/playground [웹 Node.js 실행]
