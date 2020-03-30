---
layout: post
title: redux-tutorial/02_about state and meet redux
description: redux state
tags: [redux]
---
👉 https://github.com/happypoulp/redux-tutorial/wiki
<!-- Tutorial 02 - about-state-and-meet-redux.js -->
# Tutorial 02 - about state and meet redux

<!-- Sometimes the actions that we'll handle in our application will not only inform us
that something happened but also tell us that data needs to be updated. -->

가끔 애플리케이션에서 액션은 무언가 발생했다는 것을 알려주지 않고 데이터 갱신이 필요하다는것도 말해주지 않는다.

<!-- This is actually quite a big challenge in any app.
Where do I keep all the data regarding my application along its lifetime?
How do I handle modification of such data?
How do I propagate modifications to all parts of my application? -->

이건 우리 앱에서 꽤 큰 문제다. 애플리케이션이 떠있는 동안 그 데이터는 어디에 보관해야할까?
어떻게 데이터를 수정할까?
어떻게 그 변경사항들을 내 애플리케이션 곳곳에 전파시킬까?

<!-- Here comes Redux. -->
그래서 리덕스가 나오게 되었다.

<!-- Redux (https://github.com/reactjs/redux) is a "predictable state container for JavaScript apps" -->

[리덕스](https://github.com/reactjs/redux)는 "자바스크립트 앱에서 예견할 수 있는 상태 컨테이너" 이다.

<!-- Let's review the above questions and reply to them with Redux vocabulary (flux vocabulary too for some of them): -->

위 문제들을 다시 살펴보고 Redux의 용어로 얘기해보자. (flux 용어도 살짝 있음):

<!-- Where do I keep all the data regarding my application along its lifetime?
    You keep it the way you want (JS object, array, Immutable structure, ...).
    Data of your application will be called state. This makes sense since we're talking about
    all the application's data that will evolve over time, it's really the application's state.
    But you hand it over to Redux (Redux is a "state container", remember?).
How do I handle data modifications?
    Using reducers (called "stores" in traditional flux).
    A reducer is a subscriber to actions.
    A reducer is just a function that receives the current state of your application, the action,
    and returns a new state modified (or reduced as they call it)
How do I propagate modifications to all parts of my application?
    Using subscribers to state's modifications. -->

애플리케이션이 떠있는 동안 그 데이터는 어디에 보관해야할까?
- 원하는 곳에 저장한다 (자바스크립트 object, array, Immutable structure, ...).
- 데이터는 상태라고 불려진다. 이건 우리 앱 내의 데이터들이 시간이 지남에 따라 진화하기 때문에 애플리케이션의 상태라고 얘기해도 무방하다.
- 그런데 이걸 Redux에 넘길거다 (Redux는 "상태 컨테이너" 라고 했었다).

어떻게 데이터를 수정할까?
- 리듀서를 이용한다 (flux에서는 "stores" 라고 불려진다)
- 하나의 리듀서는 액션들을 구독한다.
- 하나의 리듀서는 쉽게말해 지금 애플리케이션의 상태와 액션을 받고 새롭게 수정된 상태를(아니면 받은대로) 리턴하는 함수다.

어떻게 그 변경사항들을 내 애플리케이션 곳곳에 전파시킬까?
- 변경사항들을 구독하고 있는 구독자를 통해서.

<!-- Redux ties all this together for you.
To sum up, Redux will provide you:
    1) a place to put your application state
    2) a mechanism to dispatch actions to modifiers of your application state, AKA reducers
    3) a mechanism to subscribe to state updates -->

Redux 는 이 모든게 하나로 묶여있다.
요약하자면, Redux는:
1. 애플리케이션 상태를 저장하고
1. 애플리케이션 상태를 수정할 수 있도록 액션을 보내는 리듀서라는 것과
1. 상태 갱신을 구독하는 장치를 제공한다.

<!-- The Redux instance is called a store and can be created like this:
/*
    import { createStore } from 'redux'
    var store = createStore()
*/ -->
Redux 인스턴스는 상태라고 불려지고 이렇게 생성한다.
```javascript
import { createStore } from 'redux'
var store = createStore()
```

<!-- But if you run the code above, you'll notice that it throws an error:
    Error: Invariant Violation: Expected the reducer to be a function. -->

위 코드를 실행해보면, 이런 에러가 발생하는데:
>Error: Invariant Violation: Expected the reducer to be a function.

<!-- That's because createStore expects a function that will allow it to reduce your state. -->
`createStore`에 상태를 reduce할 함수를 전달해야한다.

<!-- Let's try again

import { createStore } from 'redux'

var store = createStore(() => {}) -->

다시 해보면
```
import { createStore } from 'redux'
var store = createStore(() => {})
```

<!-- Seems good for now... -->
이제 잘 동작한다...

<!-- Go to next tutorial: 03_simple-reducer.js -->
다음: [03_simple-reducer.md](/2020/03/14/redux-tutorial-03-simple-reducer/)
