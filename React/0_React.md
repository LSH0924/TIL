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
  - Component
  - jsx
    + React를 위해 만들어진 새로운 자바스크립트 문법
    + PHP의 개량판인 XHP에서 기원함
    + 한눈에 이해하기 쉬움(직관적)
  - 바벨(Babel)
    + 모든 브라우저가 ES6 문법을 해석할 수 있지는 않기 때문에, ES6 와 JSX 로 만들어진 어플리케이션은 바벨을 통해 ES5로 변환된다. 
  - DrtOM : 브라우저가 화면을 그리기 위한 정보가 담겨있는 문서
  - virtual DOM : Model 등에 변화가 일어나면 실제 브라우저의 DOM에 새로운 요소를 추가하는 것이 아닌, 자바스크립트로 이루어진 DOM에 한 번 랜더링을 하고, 기존의 DOM과 비교한 후 변화가 필요한 부분만 업데이트시킨다. DOM의 변화를 최소화시킴.

## 왜 virtual DOM을 사용할까?
  
  - 브라우저의 workflow
    <br>![브라우저의 워크플로우](https://www.google.com/url?sa=i&source=images&cd=&cad=rja&uact=8&ved=2ahUKEwjUktzyqsXgAhXIgbwKHfmsBrEQjRx6BAgBEAU&url=https%3A%2F%2Fvelopert.com%2F3236&psig=AOvVaw2PGsRxd7Lg4ZAAHxwbB92p&ust=1550581101710163)

    

-----

## 참고
- 리액트 도움닫기(The Road to learn React) : 로빈 위어크, 이수진
(https://github.com/the-road-to-learn-react/the-road-to-learn-react-korean/blob/master/manuscript/chapter1.md#12-%EC%A4%80%EB%B9%84-%EC%82%AC%ED%95%AD)
- 누구든지 하는 리액트 1편: 리액트는 무엇인가<br>(https://velopert.com/3612)
- [번역] 리액트에 대해서 그 누구도 제대로 설명하기 어려운 것 – 왜 Virtual DOM 인가?<br>(https://velopert.com/3236)