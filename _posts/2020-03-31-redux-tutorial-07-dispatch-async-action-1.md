---
layout: post
title: redux-tutorial/07_dispatch async action 1
description: redux dispatch async action
tags: [redux]
---
👉 https://github.com/happypoulp/redux-tutorial/wiki
<!-- Tutorial 07 - dispatch-async-action-1.js -->
# Tutorial 07 - dispatch async action 1

<!-- We previously saw how we can dispatch actions and how those actions will modify the state of our application thanks to reducers. -->

액션을 어떻게 dispatch하고 이런 액션들로 어떻게 리듀서를 이용해서 어플리케이션의 상태를 바꿀 수 있는지 배웠다.

<!-- But so far we've only considered synchronous actions or, more exactly, action creators that produce an action synchronously: when called an action is returned immediately. -->

그런데 우린 지금까지 동기 액션, 즉, 동기로 액션을 내보내는 액션 creator에 대해서만 다루어왔다: 액션을 호출하면 즉시 리턴됨

<!-- Let's now imagine a simple asynchronous use-case:
1) user clicks on button "Say Hi in 2 seconds"
2) When button "A" is clicked, we'd like to show message "Hi" after 2 seconds have elapsed
3) 2 seconds later, our view is updated with the message "Hi" -->

먼저 간단한 비동기 사용 예제를 상상해보자:
1. 유저가 "2초 뒤에 안녕이라고 해" 버튼을 클릭한다.
2. 버튼 "A"를 클릭하면, 2초가 지난 뒤에 "안녕"이라는 메세지를 보여주고 싶다.
3. 2초 뒤, 뷰는 "안녕" 메세지를 보여준다.

<!-- Of course this message is part of our application state so we have to save it in Redux store. But what we want is to have our store save the message only 2 seconds after the action creator is called (because if we were to update our state immediately, any subscriber to state's modifications - like our view - would be notified right away
and would then react to this update 2 seconds too soon). -->

물론 이 메세지는 우리 어플리케이션 상태의 일부이기 때문에 Redux store에 저장해야한다. 그런데 우리가 하고싶은 건 action creator 가 호출되고 2초 뒤에 store에 메세지를 저장하게 하는 것이다. (왜냐면 우리가 즉시 state를 업데이트 하게되면, state의 변경사항을 구독하고 있던 - 예를 들면 뷰가 - 즉시 알게되고 2초 뒤에 너무 바로 반응하게 될 것이다).

<!-- If we were to call an action creator like we did until now...

import { createStore, combineReducers } from 'redux'

var reducer = combineReducers({
    speaker: function (state = {}, action) {
        console.log('speaker was called with state', state, 'and action', action)

        switch (action.type) {
            case 'SAY':
                return {
                    ...state,
                    message: action.message
                }
            default:
                return state;
        }
    }
})
var store_0 = createStore(reducer)

var sayActionCreator = function (message) {
    return {
        type: 'SAY',
        message
    }
}

console.log("\n", 'Running our normal action creator:', "\n")

console.log(new Date());
store_0.dispatch(sayActionCreator('Hi'))

console.log(new Date());
console.log('store_0 state after action SAY:', store_0.getState())
// Output (skipping initialization output):
//     Sun Aug 02 2015 01:03:05 GMT+0200 (CEST)
//     speaker was called with state {} and action { type: 'SAY', message: 'Hi' }
//     Sun Aug 02 2015 01:03:05 GMT+0200 (CEST)
//     store_0 state after action SAY: { speaker: { message: 'Hi' } } -->

지금까지 했던대로 action creater를 호출해본다면...

```js
import { createStore, combineReducers } from 'redux'

var reducer = combineReducers({
    speaker: function (state = {}, action) {
        console.log('speaker 가 전달받은 상태', state, '와 액션', action)

        switch (action.type) {
            case 'SAY':
                return {
                    ...state,
                    message: action.message
                }
            default:
                return state;
        }
    }
})
var store_0 = createStore(reducer)

var sayActionCreator = function (message) {
    return {
        type: 'SAY',
        message
    }
}

console.log("\n", 'Running our normal action creator:', "\n")

console.log(new Date());
store_0.dispatch(sayActionCreator('Hi'))

console.log(new Date());
console.log('SAY 액션 이후 store_0 상태:', store_0.getState())
// Output (초기화 output 은 건너뜀):
//     Sun Aug 02 2015 01:03:05 GMT+0200 (CEST)
//     speaker 가 전달받은 상태 {} 와 액션 { type: 'SAY', message: 'Hi' }
//     Sun Aug 02 2015 01:03:05 GMT+0200 (CEST)
//     SAY 액션 이후 store_0 상태: { speaker: { message: 'Hi' } }
```

<!-- ... then we see that our store is updated immediately.

What we'd like instead is an action creator that looks a bit like this:

var asyncSayActionCreator_0 = function (message) {
    setTimeout(function () {
        return {
            type: 'SAY',
            message
        }
    }, 2000)
} -->

... 그러면 store가 즉시 갱신되었다.

우리가 원하는 action creator 는 대략 이런식이다:

```js
var asyncSayActionCreator_0 = function (message) {
    setTimeout(function () {
        return {
            type: 'SAY',
            message
        }
    }, 2000)
}
```

<!-- But then our action creator would not return an action, it would return "undefined". So this is not quite the solution we're looking for. -->

그런데 이렇게하면 action creator 는 action 을 리턴하지 않게되고, `undefined`를 리턴하게 될 것이다. 그래서 이건 우리가 찾던 방법이 아니다.

<!-- Here's the trick: instead of returning an action, we'll return a function. And this function will be the one to dispatch the action when it seems appropriate to do so. But if we want our function to be able to dispatch the action it should be given the dispatch function. Then, this should look like this:

var asyncSayActionCreator_1 = function (message) {
    return function (dispatch) {
        setTimeout(function () {
            dispatch({
                type: 'SAY',
                message
            })
        }, 2000)
    }
} -->

여기 꼼수가 있다: action을 리턴하는 대신 함수를 리턴할 것이다. 그리고 이것은 적절한 시점에 action을 dispatch할 함수가 될 것이다. 그런데 action을 dispatch할 수 있으려면 dispatch 함수가 있어야할 것이고, 이런식일 것이다:

```js
var asyncSayActionCreator_1 = function (message) {
    return function (dispatch) {
        setTimeout(function () {
            dispatch({
                type: 'SAY',
                message
            })
        }, 2000)
    }
}
```

<!-- Again you'll notice that our action creator is not returning an action, it is returning a function.
So there is a high chance that our reducers won't know what to do with it. But you never know, so let's try it out and find out what happens... -->

이것은 다시 action을 리턴하지않고 함수를 리턴하는 action creator가 되었다. 이걸로 reducer는 우리가 뭘 하려는 건지 모를 가능성이 크지만 당신은 절대 알지 못한다. 어떤일이 발생하는지 살펴보자...

<!-- Go to next tutorial: 08_dispatch-async-action-2.js -->
다음: [08_dispatch-async-action-2.md](/2020/03/31/redux-tutorial-08-dispatch-async-action-2/)
