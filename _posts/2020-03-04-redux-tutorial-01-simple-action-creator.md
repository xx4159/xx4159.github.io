---
layout: post
title: redux-tutorial/01_simple action creator
description: redux 소개
tags: [redux]
---
👉 https://github.com/happypoulp/redux-tutorial/wiki
<!-- Tutorial 1 - simple-action-creator.js -->
# Tutorial 1 - simple action creator

<!-- We started to talk a little about actions in the introduction but what exactly are those "action creators" and how are they linked to "actions"? -->

우리는 이전 튜토리얼에서 action에 대해서 얘기해보았다. 그런데 "action creators"는 정확히 뭐고 그것들은 "actions"에 어떻게 연결되는걸까?

<!-- It's actually so simple that a few lines of code can explain it all! -->

그건 사실 되게 쉽고 몇 줄의 코드만으로 설명할 수 있다!

<!-- The action creator is just a function...
var actionCreator = function() {
    // ...that creates an action (yeah, the name action creator is pretty obvious now) and returns it
    return {
        type: 'AN_ACTION'
    }
} -->

action creator 는 쉽게말해 그냥 하나의 함수다...
```javascript
var actionCreator = function() {
    // ...action 생성하고 action 리턴
    return {
        type: 'AN_ACTION'
    }
}
```

<!-- So is that all? yes. -->
이게 전부다.

<!-- However, one thing to note is the format of the action. This is kind of a convention in flux that the action is an object that contains a "type" property. This type allows for further handling of the action. Of course, the action can also contain other properties to pass any data you want. -->

그치만 주의할것이 action의 포맷이다. flux에서의 컨벤션 중 하나이다.
action은 "type" 프로퍼티를 갖고있는다. 이게 나중에 action을 핸들링한다.
물론 다른 프로퍼티도 가질 수 있고 원하는 데이터를 넘길 수 있다.

<!-- We'll also see later that the action creator can actually return something other than an action,
like a function. This will be extremely useful for async action handling (more on that
in dispatch-async-action.js). -->

나중에 action creator 가 action 말고 다른것을 리턴하는것을 해볼거다. 예를 들어 함수라던가. 이건 비동기 action 핸들링에 유용하다 ([07_dispatch-async-action-1.md](./07_dispatch-async-action-1.md)).

<!-- We can call this action creator and get an action as expected:
console.log(actionCreator())
Output: { type: 'AN_ACTION' } -->

우린 이제 action creator를 호출할 수 있고 예상하는 action을 얻을 수 있다:
```javascript
console.log(actionCreator())
// Output: { type: 'AN_ACTION' }
```

<!-- Ok, this works but it does not go anywhere...
What we need is to have this action be sent somewhere so that
anyone interested could know that something happened and could act accordingly.
We call this process "Dispatching an action". -->

작동하지만 이게 다다...
우리는 이 action이 어딘가로 보내져서 관심을 갖고있는 누군가가 알게되고 그것에 맞춰서 행동할 수 있게 해야한다. 우린 이걸 "action 을 Dispatch 한다" 고 말한다.

<!-- To dispatch an action we need... a dispatch function ("Captain obvious").
And to let anyone interested know that an action happened, we need a mechanism to register
"handlers". Such "handlers" to actions in traditional flux application are called stores and
we'll see in the next section how they are called in Redux. -->

action을 dispatch 하기위해서는... dispatch 함수가
그리고 누군가가 action이 발생했다는것을 알게하려면, 핸들러를 등록할 방법이 필요하다. flux 애플리케이션에서 action에 등록할 그런 "핸들러"들은 store 라고 불려지고 Redux에서는 어떻게 불려지는지 다음에 알아보자.

<!-- So far here is the flow of our application:
ActionCreator -> Action -->

여기까지가 애플리케이션의 흐름이다: ActionCreator -> Action

<!-- Read more about actions and action creators here:
http://redux.js.org/docs/recipes/ReducingBoilerplate.html -->

action과 action creator에 대해서는 [여기서](https://redux.js.org/recipes/reducing-boilerplate#reducing-boilerplate) 더 자세히 알 수 있다.

<!-- Go to next tutorial: 02_about-state-and-meet-redux.js -->
다음: [02_about-state-and-meet-redux.md](./02_about-state-and-meet-redux.md)
