# React?

  - 2013년 페이스북이 만든 뷰 라이브러리(View Library)
  - Babel 등의 트랜스파일러를 사용해 컴파일

## 장점

  - 모든 프레임워크와 서로 상호 교환할 수 있다.
  - 여러 복잡한 기능을 추가하기 전 오롯이 뷰 레이어에 집중해서 개발할 수 있음.
  - 생태계가 넓음 : 써드파티 라이브러리가 많이 개발됨. JQuery나 javascript로 만들어진 라이브러리들도 react로 많이 포팅되어 나옴.
    + 써드파티 라이브러리 : 독자적, 개인적으로 사용하기 위해 만든 라이브러리.
  - 새로 만들어지는 프로젝트나 리뉴얼 되는 프로젝트에서 많이 사용된다.
  
## 요소
  - **Component**
  - **바벨(Babel)**
    + 모든 브라우저가 ES6 문법을 해석할 수 있지는 않기 때문에, ES6 와 JSX 로 만들어진 어플리케이션은 바벨을 통해 ES5로 변환된다. 
  - **DOM** : 브라우저가 화면을 그리기 위한 정보가 담겨있는 문서
  - **virtual DOM** : Model 등에 변화가 일어나면 실제 브라우저의 DOM에 새로운 요소를 추가하는 것이 아닌, 자바스크립트로 이루어진 DOM에 한 번 랜더링을 하고, 기존의 DOM과 비교한 후 변화가 필요한 부분만 업데이트시킨다. DOM의 변화를 최소화시킴.
  - **webpack** : 파일을 묶어 가져오는 번들링 도구. import(혹은 require)로 모듈을 불러오면 번들링시켜 각 모듈들을 하나의 파일로 합쳐준다.

### **jsx**
  + React를 위해 만들어진 새로운 자바스크립트 문법
  + PHP의 개량판인 XHP에서 기원함
  + 한눈에 이해하기 쉬움(직관적)
  + 바벨이 코드를 변환하는 과정에서 오류를 감지해내기 때문에 편리함
  + 번들링 될 때 babel-loader를 통해 자바스크립트로 변환
  ```javascript
  var a = (
    <div>
      <h1>JSX의 <strong>변환</strong></hl>
    </div>
  );

  // javascript code로 변환(트리 구조가 됨)

  var a = React.createElement("div", null,
            React.createElement("h1", null, "JSX의 ", 
              React.createElement("strong", null, "변환")
            )
          );
  ```
  
## 브라우저의 **workflow**
  
  ![브라우저의 워크플로우](https://velopert.com/wp-content/uploads/2017/03/wvbwscn7oadykroobdd3.png)

  - **DOM Tree**
    - 브라우저가 HTML을 전달받으면 브라우저의 랜더 엔진이 파싱한 뒤, DOM노드로 이루어진 트리를 만듦.
    - 만들어진 노드는 각각의 HTML Element와 연관되어 있다.
  - **Render Tree**
    - 외부 CSS와 인이라랑라인 스타일을 파싱한다.
    - 스타일 정보와 DOM Tree를 기반으로 랜더트리를 생성함.
    - attatchment
      - webkit 에서 각 노드의 스타일을 처리하는 과정
      - 각각의 DOM노드에는 attach() 메소드가 있으며, attach()는 스타일 정보를 계산해 객체형대로 반환한다.
      - attach() : 동기적으로 작동한다. DOM 트리에 새 노드가 추가되면, 추가된 노드의 attach()가 작동됨.
  - **Layout(reflow)**
    - 각 노드에 스크린의 좌표를 배분한다.
  - **Painting**
    - 랜더링 된 요소들을 그려낸다.
    - 트리의 각 노드를 거치며 paint() 를 호출 -> 스크린에 각 요소가 출력됨

## 왜 Virtual DOM 을 사용할까?

  DOM을 하나 조작, 변경할 때마다 랜더트리 재생성 -> attatchment -> 레이아웃 작업 -> 페인팅 작업을 반복하게 된다.
  특히 SPA(Single Page Application) 에서는 DOM의 조작 횟수가 많아지기 때문에 이에 따른 연산의 수가 늘어나고, 전체적인 프로세스가 비효율적으로 돌아가버린다.
  <br/>
  이때, Virtual DOM을 사용하면 연산을 최소화 시킬 수 있다.
  Virtual DOM은 오프라인 DOM<sub>(랜더링이 필요하지 않으므로 연산비용이 적다.)</sub> 에 변경사항을 적용하고, 연산이 끝나면 최종 결과를 DOM에 전달한다.
  <br/>
  전달할 때, 변화된 모든 결과를 하나로 묶어서 한번에 처리<sub>동시에 일어난 변화만 한번에 처리할 수 있다.</sub>하기 때문에 레이아웃 계산규모와 리랜더링 규모는 커질 수 있어도 연산 자체의 횟수는 확실하게 줄일 수 있어 보다 빠르고 효율적으로 사용할 수 있다.
  <br/>
  DOM Fragment 또한 같은 작업을 할 수 있는데, Virtual DOM은 DOM Fragment를 관리하는 과정을 자동화, 추상화 시켜주기 때문에 더 편리하다.




-----

## 참고
- 리액트 도움닫기(The Road to learn React) : 로빈 위어크, 이수진
(https://github.com/the-road-to-learn-react/the-road-to-learn-react-korean/blob/master/manuscript/chapter1.md#12-%EC%A4%80%EB%B9%84-%EC%82%AC%ED%95%AD)
- 누구든지 하는 리액트 1편: 리액트는 무엇인가<br>(https://velopert.com/3612)
- [번역] 리액트에 대해서 그 누구도 제대로 설명하기 어려운 것 – 왜 Virtual DOM 인가?<br>(https://velopert.com/3236)
- 리액트를 다루는 기술 - 김민준 저