---
layout: post
title: "찾다 못해 내가 쓴다. 뉴비를 위한 React-Redux 튜토리얼"
subtitle:
tags: [React, Redux]
excerpt_separator: <!--more-->
---

`Redux(리덕스)`를 처음 배우기 시작했을 때, 나는 가장 간단한 튜토리얼을 찾길 원했다. 여러 훌륭한 자료들에도 불구하고 Redux의 개념들 중 일부는 이해하기가 어려웠다.  
**state**가 뭔지는 알았지만 **action**, **action creator** 그리고 **reducer**?? 뭔가 아리까리 했다.  
특히 **React**와 **Redux**를 서로 연결하는 방법에서 좌절.<!--more-->

그래서 나는 나만의 React Redux 튜토리얼을 쓰기 시작했고 그러면서 많은 것을 배웠다. 이 글을 쓰면서 Redux의 기본 원리를 스스로 배우게 되었기에, 이 글이 React와 Redux를 배우는 디른 사람들에게도 유용하기를 바란다.

그럼 즐독!

바쁜 사람은 <u>2-1. Redux store에 대해 알아보자</u> 부터 읽어도 상관없다.

<br>

## 목차
-------------
  
- 1-1. 누구를 위한 글인가?
- 1-1. 이 글에서 당신이 배울 내용
- 1-2. 최소한의 React 개발 환경
- 1-3. state란 무엇인가?
- 1-4. Redux가 도대체 어떤 문제를 해결해 주는가?
- 1-5. Redux 꼭 배워야함?
- 1-6. Redux 안쓰면 안됨?
- 1-7. Redux store에 대해 알아보자
- 2-1. Redux reducer에 대해 알아보자
- 2-2. Redux action에 대해 알아보자
- 2-3. reducer를 리팩토링 해보자
- 2-4. Redux store의 Method를 살펴보자
- 3-1. React와 Redux 연결하기
- 3-2. react-redux
- 3-3. App component 그리고 Redux store
- 3-4. List component 그리고 Redux store
- 3-5. Form component 그리고 Redux action
- 4-1. 마무리
- 4-2. 후기
  
-------------

<br>

#### 1. 누구를 위한 글인가?

다음에 해당된다면 매우 적절하다고 본다.
- Javascript, ES6 그리고 React를 그럭저럭 이해하고 있다. (*모르면 찾아보지 뭐...*)
- Redux를 가장 간단한 방법으로 배우길 기대하고 있다.

<br>

#### 2. 이 글에서 당신이 배울 내용

- Redux란 무엇인가?
- Redux를 React와 함께 사용하는 방법

<br>

#### 3. 최소한의 React 개발 환경

시작하기 전에 **React 개발 환경을 구축할 준비가 되었는지 확인**하길 바란다.  
여기서는 [`create-react-app`](https://github.com/facebook/create-react-app){: target="_blank" }
을 사용하도록 하겠다. (React에 대해서는 다른 글 참조)

{% highlight shell %}
  $ npx create-react-app react_redux_tutorial
{% endhighlight %}

<br>

#### 4. state란 무엇인가?

**Redux가 무엇인지 이해**하려면 먼저 `state`가 무엇인지 알아야 한다.  
만약 React로 작업을 한 적이 있다면, state라는 용어는 낯설지 않을것이다.

이미 다음과 같은 **stateful component**를 사용해 봤을것이다.

{% highlight javascript %}
  import React, { Component } from "react";

  class StatefulComponent extends Component {
    state = {
      list: [
        { id: 1, title: "React" },
        { id: 2, title: "Redux" }
      ]
    }

    render() {
      const { list } = this.state;
      return (
        <ul>
          {list.map(item => <li key={item.id}>{item.title}</li>)}
        </ul>);
    }
  }
{% endhighlight %}

모든 stateful component는 **스스로 state를 가지고 있다**.

> 참고로 **stateful** component는 class component라고도 하며<br>
> state가 없은 **stateless** component가 있다.(functional component라고도 한다)

React에서 state는 **데이터**를 보관하며, component는 사용자에게 보관된 데이터[*data*]를 렌더[*render*]할 것이다. 또한 액션[*action*] 및 이벤트[*event*]에 의하여 변경될 수 있다.  (React에서는 컴포넌트의 state를 setState로 변경이 가능하다)

그래서 **결국 state가 뭐냐**

state라는 용어는 React에서만 한정된 것이 아니다.

state는 **당신 주위에 항상 있다**.  
심지어 **가장 단순한 자바스크립트 어플리케이션도 state를 가지고 있다**.

다음 예를 한 번 생각해 보자.

1. 유저가 버튼을 클릭한다   
2. 그리고 나서 모달이 열린다

이 사소한 상호작용에서 우리가 다뤄야 할 state가 있다.

우리는 다음과 같이 순수 자바스크립트 객체[*object*]로 초기 state를 나타낼 수 있다.

{% highlight javascript %}
  var state = {
    buttonClicked: 'no',
    modalOpen: 'no'
  }
{% endhighlight %}

그리고 유저가 버튼을 클릭하면,

{% highlight javascript %}
  var state = {
    buttonClicked: 'yes',
    modalOpen: 'yes'
  }
{% endhighlight %}

자, 어떻게 하면 state를 객체에 저장하는것 이외에, **순수 자바스크립트에서 위와 같이 state의 변화를 추적** 할 수 있을까?  **state를 추적하는 데 도움이 될 만한 라이브러리**가 있나?

<br>

#### 5. Redux가 도대체 어떤 문제를 해결해 주는가?

전형적인 자바스크립트 어플리케이션은 state로 채워져있다.  
잘 모르겠다면 여기 몇 가지 state 예가 있다.

- 사용자에게 보여지는 데이터(*data*)
- 외부에서 가져오는(*fetch*) 데이터
- 사용자에게 표시되는 URL
- 페이지 내에서 선택되는 항목(*item*)
- 어플리케이션에 오류가 있는가? 그것도 state다

state는 자바스크립트 어디에나 있다.

당신이 볼 수 있는 가장 단순한 자바스크립트 어플리케이션조차 state를 갖고있다.

React 어플리케이션이 **얼마나 많은 state를 가지고 있는지** 상상해 보자.

어플리케이션이 작은 상태로 유지되는 한 state를 부모 컴포넌트 안에 두는 것으로 그럭저럭 관리할 수 있을것이다.

하지만 state를 위아래로(부모-자식 컴포넌트) 통과하기 시작한다면 단순한 투두 리스트(todo list)조차도 작업이 매우 까다로워질 것이다.

게다가 점점 거대해지는 이 React 컴포넌트를 누가 원하겠는가?

여기서 잠깐, 그렇다고 나는 프론트엔드에서 비즈니스 로직에 대해 알아야 한다고 말하는게 아니다.

React 컴포넌트의 state를 관리하기 위한 대안이 있지않을까 하고 생각해 보는 것이다.

그 중 하나가 Redux다.

Redux는 시작단계에서 명확하지 않을 수도 있는 문제를 해결해 줄 것이다.  
(*※ 각 React 컴포넌트가 필요한 정확한 state를 제공하는 것과 같은 문제*)

Redux는 **한 곳**에서 state를 관리하며, React의 바깥에 있는 state를 가져오고 관리하는 로직도 가지고 있다.

이러한 접근의 이점은 지금으로써는 그다지 확실하게 와닿지 않을 수도 있다. Redux에 발을 담그는 순간 모든 것이 분명해질 것이다.

다음 섹션에서 Redux를 배워야 하는 이유와 어플리케이션에서 언제 Redux를 사용해야 하는지를 알아보자.

<br>

#### 6. Redux 꼭 배워야함?

Redux는 단어 자체로 대부분의 뉴비들이 겁을 낸다. 하지만 이 글을 보는 당신의 경우는 아니다. **Redux는 그렇게 어렵지 않다**. 핵심은 어떠한 이유에서든 Redux를 배우기 위해 서두르지 말라는 것이다.

만약 당신이 Redux에 대해 동기부여가 되어있고 열정적이라면 지금 Redux를 배우기 시작해야 한다.

나는 Redux를 다음의 이유로 배우기 시작했다.

- Redux가 어떻게 작동하는지 매우 관심이 많았다.
- React와 관련된 스킬을 향상시키고 싶었다.
- React/Redux 조합은 매우 흔하디 흔하다.
- 무엇보다 Redux는 **프레임워크에 구애받지 않는다**. 한 번 배우면 어디서나 사용할 수 있다. (Vue JS, Angular 등)

Redux 또는 그와 비슷한 state 관리 라이브러리를 배우는 것은 필수적이다.

또 다른 사실은 실제 자바스크립트 애플리케이션은 항상 상태 관리 라이브러리(state management library)를 사용 한다는 것이다.

Redux는 다른 것들과 마찬가지로 흔한 라이브러리 중 하나이지만, 가장 중요한 것 중 하나이다.

그런데 앞으로 Redux가 사라지진 않을까?

아마도..

그렇다고 해도 그 **패턴은 영원히 지속될 것이다**.

PHP가 더 이상 트렌디하지 않다는 이유만으로 OOP를 배우지 않을건가?

Redux도 마찬가지다.

당신은 state를 관리하는 **패턴**을 배워야 한다. 왜냐하면 그것이 당신의 커리어에서 매우 가치를 갖기 때문이다.

<br>

#### 7. Redux 안쓰면 안됨?

state 관리를 위해 Redux 또는 Flux(또는 Mobx)를 사용하는 것은 당신에게 달려있다.

아마도 당신은 이 라이브러리들 중 어느 것도 필요하지 않을 수도 있을것이다. 무엇보다 이것들은 비용(시간)이 든다.

하지만 나는 Redux를 비용으로서가 아니라 투자로 생각한다. 

Redux 초보자들에게 또 다른 일반적인 질문은 다음과 같다.  
**어플리케이션에서 Redux가 필요한지 어떻게 아는가**??

언제 **state 관리를 위한 Redux**가 필요한지 결정하는 규칙은 따로 없다.

내가 새로운 React 프로젝트를 시작할 때 나는 항상 Redux를 바로 추가하고 싶은 유혹을 느낀다. 그런데 생각해 보면 개발자로서 우리는 항상 필요 이상으로 코딩을 하지 않는가?

Redux를 선택하기 전에 다른 대안에 대해 찾아보자. 특히 React의 state나 props를 최대한 활용하도록 노력하라.

그리고 이 사실을 잊지 말도록. React 프로젝트는 Redux를 나중에 포함 하도록 간단히 리팩토링 할 수 있다는 것이다.

결론적으로 나는 다음과 같은 경우에 Redux의 사용을 고려해 볼 만 하다고 생각한다.

- React 컴포넌트가 같은 state를 참조해야 하지만 서로 부모/자식 관계가 아니다.
- state를 props와 함께 여러 컴포넌트로 넘기는 것에 뭔가 어색함(불편함)을 느끼기 시작한다.

만일 여전히 감이 안온다고해도 너무 걱정하지 말길 바란다. 나도 똑같았으니깐.

Redux는 소규모 앱에서는 유용하지 않다는 점에 유의하길 바란다. 규모가 좀 더 큰 앱에서 그 빛을 발휘할 것이다. 어쨌든, 비록 큰 앱과 관계 없더라도 Redux를 배우는 것은 누구에게도 해가 되지 않는다.

이제 본격적으로 Redux에 대해서 알아보도록 하자.

<br>

#### 8. Redux store에 대해 알아보자

**Redux에서는 store가 모든 부분을 지휘 한다**.  
뭐라고??

`store`!!

Redux에서 store는 마치 사람의 두뇌와 같다. 핵심이라고 보면 된다. 모든 어플리케이션의 state는 store 안에 존재한다. 따라서 Redux를 시작하려면 state를 포함하고 있는 store를 만들어야 한다.

자, 이제 본격적으로 React 개발환경에서 Redux를 설치해 보자.

{% highlight shell %}
  $ yarn add redux --dev
{% endhighlight %}

그리고 store를 저장할 폴더도 만들자

{% highlight shell %}
  $ mkdir -p src/js/store
{% endhighlight %}

위의 폴더에 `index.js` 파일을 만들고 다음과 같이 store를 초기화 하자.

{% highlight javascript %}
  // src/js/store/index.js

  import { createStore } from "redux";
  import rootReducer from "../reducers/index";

  const store = createStore(rootReducer);

  export default store;
{% endhighlight %}

createStore는 store를 만들기 위한 함수이다.

createStore는 첫번째 인수로 reducer를 갖는다. 여기선 rootReducer다.

그런데 reducer는 또 뭐지??

Redux에서 reducer는 state를 생산한다. 하지만 여기서 state는 당신이 직접 만드는 무언가가 아니다.

이 사실을 잘 생각하면서 Redux reducer로 발길을 옮겨보자.

<br>

#### 9. Redux reducer에 대해 알아보자

그럼 reducer란 무엇인가?

reducer는 어렵지 않다. 그저 **두개의 파라미터를 받는 순수 자바스크립트 함수**다. **두개의 파라미터는 현재의 state**와 **action** 이다.  Redux에서 **state는 전적으로 reducer로부터 받아야(*return*) 한다**.

React에서 state는 setState로 변경하지만, Redux에서는 그렇게 할 수가 없다.

action에 대해선 다음 섹션에서 자세히 알아 보기로 하고, 그 전에 Redux의 세 번째 원칙인 불변함(*immutable*).  
([Redux의 세 가지 원칙](https://paulkogel.gitbooks.io/redux-docs/content/docs/introduction/ThreePrinciples.html){: target="_blank" })  
즉, 순수한 함수여야 한다. 순수 함수란 주어진 인풋값에 대해 정확히 같은 아웃풋을 리턴하는걸 의미한다.

우리는 **초기 상태(*initial state*)**를 **첫 번째 파라미터**로 갖는 간단한 **reducer**를 만들 것이다. **두 번째 파라미터**로서는 **action**을 줄 것이다. 지금의 reducer는 initial state를 되돌려 주는 것 외에는 아무 것도 하지 않을 것이다.

다음의 폴더를 만들고

{% highlight shell %}
  $ mkdir -p src/js/reducers
{% endhighlight %}

위의 폴더에 `index.js`를 다음과 같이 만들자

{% highlight javascript %}
  // src/js/reducers/index.js

  const initialState = {
    articles: []
  };

  const rootReducer = (state = initialState, action) => state;

  export default rootReducer;
{% endhighlight %}

이 튜토리얼을 가능한 단순하게 하려다 보니 이런 멍청한 reducer가 만들어 졌다. 이것은 아무것도 하지 않고 initial state를 단순히 리턴 할 뿐이다.

다음 섹션에선 action을 추가할 것이다. 좀 더 흥미로워질테니 기대하도록

<br>

#### 10. Redux action에 대해 알아보자

Redux에서 reducer는 의심의 여지없이 가장 중요한 개념이다. reducer는 어플리케이션의 state를 제공한다.

하지만 reducer는 state를 언제 제공해야 하는지 어떻게 알 수 있을까?

Redux의 두 번째 원칙은 말한다. **state를 바꾸는 유일한 방법은 store에 신호를 보내는 것**이라고. 이 신호가 **action**이다. "**action을 dispatch한다.**" 즉, 신호를 보내 처리한다는 뜻이다.

자, 그러면 어떻게 불변한(*immutable*) state를 바꿀 수 있을까? 현재 state를 복사 후 새 데이터(*data*)를 추가하여 새로운 state를 만드는 방법이 있다. 기존의 state에는 영향이 없다.

여기서 알아야 할 것은, **action은 단지 자바스크립트 객체**일 뿐이라는 것이다.

{% highlight javascript %}
  {
    type: 'ADD_ARTICLE',
    payload: {
      name: 'React Redux Tutorial',
      id: 1
    }
  }
{% endhighlight %}

이것이 action 이다. 모든 action들은 state가 어떻게 변해야 하는지를 나타내는 **type**속성을 필요로 한다. 

payload도 지정할 수 있다. 위의 예에서, payload는 새로운 article 객체이다. reducer는 나중에 그 article을 현재의 state에 추가할 것이다.

여기서 **모든 action을 함수로 감싸는** 것은 최선의 방법이 될 것이다. 그러한 함수를 **action creator**라고 한다.

일단 간단한 Redux action을 만들어 보면서 모든 것을 종합해 보도록 하자.

action을 위한 폴더를 만들고

{% highlight shell %}
  $ mkdir -p src/js/actions
{% endhighlight %}

`index.js` 파일을 만들자.

{% highlight javascript %}
  // src/js/actions/index.js

  export const addArticle = article => ({ 
    type: "ADD_ARTICLE",
    payload: article
  });

{% endhighlight %}

여기서, type속성은 단지 문자열(*string*) 그 이상도 아니다.

reducer는 state를 어떻게 처리해야 하는지를 판단하기 위해 이 문자열을 사용할 것이다.

문자열은 오타가 나거나 중복되기 쉬운 경향이 있기 때문에 **action의 type을 상수로 선언하는 편이 좋다**.

이 방법은 디버깅하기 어려운 오류를 피하는 데 도움이 된다.

새 폴더를 만들자

{% highlight shell %}
  $ mkdir -p src/js/constants
{% endhighlight %}

그리고 다음의 파일을 만들자 `action-types.js`

{% highlight javascript %}
  // src/js/constants/action-types.js
  
  export const ADD_ARTICLE = "ADD_ARTICLE";
{% endhighlight %}

이제 `src/js/actions/index.js` 파일을 열고, **action creator**인 addArticle 함수를 만들어 주자.

{% highlight javascript %}
  // src/js/constants/action-types.js
  
  import { ADD_ARTICLE } from "../constants/action-types";

  export const addArticle = article => ({ 
    type: ADD_ARTICLE,
    payload: article
  });
{% endhighlight %}

<br>

#### 11. reducer를 리팩토링 해보자

다음 단계로 넘어가기 전에 주요 Redux 개념을 다시 요약하자면,

- **Redux store**는 두뇌와 같다. Redux의 **모든 부분을 총괄**한다.
- store 안에서 어플리케이션의 **state는 하나의 불변의(*immutable*) 객체(*object*)로 살아간다**.
- store는 **action을 받는 즉시 reducer를 작동**시킨다.
- reducer는 state를 반환한다(*return*).

Redux reducer는 무엇으로 만들어졌는가?

- reducer는 **state와 action 두 가지 매개변수**를 취하는 자바스크립트 함수다.
- reducer 함수는 스위치 구문을 갖는다.(if 문도 상관없다.)
- reducer는 action type에 따라서 새로운 state를 계산하고, 일치하는 action type이 없을 때는 적어도 초기 state를 리턴 할 것이다.

이전 섹션에서 만든 우리의 reducer는 초기 state를 반환하는것 이외에는 아무것도 하지 않았다. 자 수정해 보자.

`src/js/reducers/index.js`를 열고 reducer를 다음과 같이 수정하자.

{% highlight javascript %}
  import { ADD_ARTICLE } from "../constants/action-types";
  
  const initialState = {
    articles: []
  };

  const rootReducer = (state = initialState, action) => {
    switch (action.type) {
      case ADD_ARTICLE:
        state.articles.push(action.payload);
        return state;
      default:
        return state;
    }
  };

  export default rootReducer;
{% endhighlight %}

여기서 뭐가 보이나?

비록 이것이 유효한 코드이긴 하지만, 위의 reducer는 세 번째 Redux 원칙, 불변성을 깨뜨린다.

[push](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push){:target="_blank"}는 순수함수가 아니다. 그것은 원래의 배열을 바꾸기 때문이다.

우리의 reducer를 순수함수로 만드는 것은 쉽다. 다음과 같이 push대신 [concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat){:target="_blank"}을 사용하는 것으로 초기 배열을 불변의 상태로 유지하기에 충분하다.

{% highlight javascript %}
  import { ADD_ARTICLE } from "../constants/action-types";
  
  const initialState = {
    articles: []
  };

  const rootReducer = (state = initialState, action) => {
    switch (action.type) {
      case ADD_ARTICLE:
        return { ...state, articles: state.articles.concat(action.payload) };
      default:
        return state;
    }
  };

  export default rootReducer;
{% endhighlight %}

아직 끝이 아니다! [spread 연산자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax){:target="_blank"}로 더 훌륭하게 만들 수 있다.

{% highlight javascript %}
  import { ADD_ARTICLE } from "../constants/action-types";
  
  const initialState = {
    articles: []
  };

  const rootReducer = (state = initialState, action) => {
    switch (action.type) {
      case ADD_ARTICLE:
        return { ...state, articles: [...state.articles, action.payload] };
      default:
        return state;
    }
  };

  export default rootReducer;
{% endhighlight %}

위의 예에서 초기 state는 전혀 영향을 받지 않았다.

초기 articles 배열은 그 자리에서 바뀌지 않는다.

초기 state 객체도 변경되지 않는다. 리턴하는 state는 초기 state의 복사본이다.

....

<br>

#### 12. Redux store의 Method를 살펴보자

이번엔 정말 간단할 것이다.

Redux가 어떻게 동작하는지 빠른게 이해하기 위해 브라우저의 콘솔에서 작업을 하려고 한다.

Redux는 2KB의 작은 라이브러리이다. Redux store는 state를 관리하기 위한 간단한 [API](https://redux.js.org/api/store#store){:target="_blank"}를 제공한다. 중요한 method는 다음과 같다.

- 현재 state를 반환하는 `getState`.
- action을 보내는 `dispatch`.
- state의 변화에 귀를 기울이는 `subscribe`.

브라우저의 콘솔에서 시험해 보기위해 글로벌 변수로써 앞에서 만든 store와 action을 가져와서 다음과 같이 수정하자.  `src/App.js`

{% highlight javascript %}
  // src/App.js

  import React, { Component } from 'react';
  import './App.css';
  
  import store from "./js/store/index";
  import { addArticle } from "./js/actions/index";
  window.store = store;
  window.addArticle = addArticle;

  class App extends Component {
    render() {
      return (
        <div>
        </div>
      );
    }
  }

  export default App;

{% endhighlight %}

React 서버를 실행 하면

{% highlight shell %}
  $ yarn start
{% endhighlight %}

이제 브라우저의 개발자환경 콘솔창에서 테스트를 해보자.

`getState()`를 통해 현재 state에 접근할 수 있다.

{% highlight shell %}
  $ store.getState()
{% endhighlight %}

{% highlight shell %}
  {articles: Array(0)}
{% endhighlight %}

물론 state값은 비어있다.

다음로 `subscribe()`로 state의 변화를 알 수 있다.

**subscribe은 action이 보내(dispatch)질 때마다 콜백함수가 호출** 된다. action을 dispatch한다는 것은 state를 바꾸기 위해 store에 알리는 것을 의미한다.

다음과 같이 콘솔창에서 콜백을 등록하자.

{% highlight shell %}
  store.subscribe(() => console.log('Hello, Redux!'))
{% endhighlight %}

state를 바꾸기 위해 action을 dispatch 할 필요가 있다. action을 dispatch 하기위해서는 `dispatch()`를 호출하면 된다.

우리는 앞에서 하나의 action을 만들었었다. state에 새로운 객체를 추가하는 addArticle이라는 action이다.

이 action을 dispatch해보자.

{% highlight shell %}
  store.dispatch( addArticle({ title: 'React Redux Tutorial', id: 1 }) )
{% endhighlight %}

다음과 같이 출력되는 것을 볼 수 있을것이다.

{% highlight shell %}
  Hello, Redux!
{% endhighlight %}

이제 state가 변경 된 것을 확인해 보자.

{% highlight shell %}
  store.getState()
{% endhighlight %}

{% highlight shell %}
  {articles: Array(1)}
{% endhighlight %}

배열이 추가 된것을 확인 했는가? 이것이 Redux다.

어려운가??

시간을 좀 갖고 위의 세가지 method들을 연습하길 바란다.

Redux를 시작하기 위해 알아야 할 것은 이것뿐이다.

일단 자신감을 갖게 되면 다음 섹션으로 넘어가자. 우리는 이제 Redux와 React를 연결할 것이다.

<br>

#### 13. React와 Redux 연결하기

<br>

#### 14. react-redux

<br>

#### 15. App component 그리고 Redux store

<br>

#### 16. List component 그리고 Redux store

<br>

#### 17. Form component 그리고 Redux action

<br>

#### 18. 마무리

<br>

#### 19. 후기


{% highlight shell %}
{% endhighlight %}



