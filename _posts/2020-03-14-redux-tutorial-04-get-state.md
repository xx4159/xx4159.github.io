---
layout: post
title: redux-tutorial/04_get state
description: redux get state
tags: [redux]
---
👉 https://github.com/happypoulp/redux-tutorial/wiki
<!-- Tutorial 04 - get-state.js -->
# Tutorial 04 - get state

<!-- How do we retrieve the state from our Redux instance?

import { createStore } from 'redux'

var reducer_0 = function (state, action) {
    console.log('reducer_0 was called with state', state, 'and action', action)
}

var store_0 = createStore(reducer_0)
// Output: reducer_0 was called with state undefined and action { type: '@@redux/INIT' } -->

Redux 인스턴스에서 상태를 어떻게 받을까?
```js
import { createStore } from 'redux'

var reducer_0 = function (state, action) {
    console.log('reducer_0 이 전달받은 상태', state, '와 액션: ', action)
}

var store_0 = createStore(reducer_0)
// Output: reducer_0 이 전달받은 상태 undefined 와 액션 { type: '@@redux/INIT' }
```

<!-- To get the state that Redux is holding for us, you call getState

console.log('store_0 state after initialization:', store_0.getState())
// Output: store_0 state after initialization: undefined -->

Redux가 갖고있는 상태를 얻기 위해서 `getState`를 호출한다.
```js
console.log('초기화 이후 store_0 의 상태:', store_0.getState())
// Output: 초기화 이후 store_0 의 상태: undefined
```

<!-- So the state of our application is still undefined after the initialization? Well of course it is, our reducer is not doing anything... Remember how we described the expected behavior of a reducer in "about-state-and-meet-redux"?
    "A reducer is just a function that receives the current state of your application, the action,
    and returns a new state modified (or reduced as they call it)"
Our reducer is not returning anything right now so the state of our application is what
reducer() returns, hence "undefined". -->

애플리케이션의 상태는 아직 undefined 이다. 당연히, reducer가 아무것도 안하고 있으니까...
[02_about-state-and-meet-redux.md](./02_about-state-and-meet-redux.md) 에서 reducer를 어떻게 설명했었는지 기억해보자.
> 하나의 리듀서는 쉽게말해 지금 애플리케이션의 상태와 액션을 받고 새롭게 수정된 상태를(아니면 받은대로) 리턴하는 함수다.

우리가 만든 reducer는 지금 아무것도 리턴하고 있지않다. 그래서 우리 reducer가 리턴한 애플리케이션 상태값이 `undefined` 이다.

<!-- Let's try to send an initial state of our application if the state given to reducer is undefined:

var reducer_1 = function (state, action) {
    console.log('reducer_1 was called with state', state, 'and action', action)
    if (typeof state === 'undefined') {
        return {}
    }

    return state;
}

var store_1 = createStore(reducer_1)
// Output: reducer_1 was called with state undefined and action { type: '@@redux/INIT' }

console.log('store_1 state after initialization:', store_1.getState())
// Output: store_1 state after initialization: {} -->

reducer에 상태값이 `undefined` 일 때 초기 상태를 보내보자.
```js
var reducer_1 = function (state, action) {
    console.log('reducer_1 이 전달받은 상태', state, '와 액션', action)
    if (typeof state === 'undefined') {
        return {}
    }

    return state;
}

var store_1 = createStore(reducer_1)
// Output: reducer_1 이 전달받은 상태 undefined 와 액션 { type: '@@redux/INIT' }

console.log('초기화 이후 store_1 의 상태:', store_1.getState())
// Output: 초기화 이후 store_1 의 상태: {}
```
<!-- As expected, the state returned by Redux after initialization is now {}

There is however a much cleaner way to implement this pattern thanks to ES6:

var reducer_2 = function (state = {}, action) {
    console.log('reducer_2 was called with state', state, 'and action', action)

    return state;
}

var store_2 = createStore(reducer_2)
// Output: reducer_2 was called with state {} and action { type: '@@redux/INIT' }

console.log('store_2 state after initialization:', store_2.getState())
// Output: store_2 state after initialization: {} -->

예상대로, 초기화 이후 Redux에 리턴된 상태는 이제 `{}` 이다.
그런데 이 코드는 ES6로 더 깔끔하게 작성할 수 있다.

```js
var reducer_2 = function (state = {}, action) {
    console.log('reducer_2 가 전달받은 상태', state, '와 액션', action)

    return state;
}

var store_2 = createStore(reducer_2)
// Output: reducer_2 가 전달받은 상태 {} 와 액션 { type: '@@redux/INIT' }

console.log('초기화 이후 store_2 의 상태:', store_2.getState())
// Output: 초기화 이후 store_2 의 상태: {}
```

<!-- You've probably noticed that since we've used the default parameter on state parameter of reducer_2, we no longer get undefined as state's value in our reducer's body. -->

reducer_2의 상태 파라미터에 기본 파라미터를 적용하였고 더이상 reducer의 바디에서 상태값을 `undefined`로 받지 않게되었다.

<!-- Let's now recall that a reducer is only called in response to an action dispatched and let's fake a state modification in response to an action type 'SAY_SOMETHING'

var reducer_3 = function (state = {}, action) {
    console.log('reducer_3 was called with state', state, 'and action', action)

    switch (action.type) {
        case 'SAY_SOMETHING':
            return {
                ...state,
                message: action.value
            }
        default:
            return state;
    }
}

var store_3 = createStore(reducer_3)
// Output: reducer_3 was called with state {} and action { type: '@@redux/INIT' }

console.log('store_3 state after initialization:', store_3.getState())
// Output: store_3 state after initialization: {} -->

액션을 dispatch 하여 reducer를 호출해보고 액션 타입 `'SAY_SOMETHING'` 에서 가상으로 상태를 수정해보자.

```js
var reducer_3 = function (state = {}, action) {
    console.log('reducer_3 이 전달받은 상태', state, '와 액션', action)

    switch (action.type) {
        case 'SAY_SOMETHING':
            return {
                ...state,
                message: action.value
            }
        default:
            return state;
    }
}

var store_3 = createStore(reducer_3)
// Output: reducer_3 이 전달받은 상태 {} 와 액션 { type: '@@redux/INIT' }

console.log('초기화 이후 store_3 의 상태:', store_3.getState())
// Output: 초기화 이후 store_3 의 상태: {}
```

<!-- Nothing new in our state so far since we did not dispatch any action yet. But there are few important things to pay attention to in the last example:
    0) I assumed that our action contains a type and a value property. The type property is mostly a convention in flux actions and the value property could have been anything else.
    1) You'll often see the pattern involving a switch to respond appropriately to an action received in your reducers
    2) When using a switch, NEVER forget to have a "default: return state" because if you don't, you'll end up having your reducer return undefined (and lose your state).
    3) Notice how we returned a new state made by merging current state with { message: action.value }, all that thanks to this awesome ES7 notation (Object Spread): { ...state, message: action.value }
    4) Note also that this ES7 Object Spread notation suits our example because it's doing a shallow copy of { message: action.value } over our state (meaning that first level properties of state are completely overwritten - as opposed to gracefully merged - by first level property of
       { message: action.value }). But if we had a more complex / nested data structure, you might choose
       to handle your state's updates very differently:
       - using Immutable.js (https://facebook.github.io/immutable-js/)
       - using Object.assign (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
       - using manual merge
       - or whatever other strategy that suits your needs and the structure of your state since
         Redux is absolutely NOT opinionated on this (remember, Redux is a state container). -->

상태에 아직 어떤 액션도 dispatch하지 않았기 때문에 상태값이 변경되지 않고 그대로다. 그런데 이전 예제에서 주목할 부분이 있다.
1. 액션에 type과 value 속성이 있다고 가정했다. type은 대부분의 flux에 존재하는 컨벤션이고 value 속성은 무엇이든 될 수 있다.
1. 이런 패턴을 종종 보게될 것이다. 이 패턴들로 리듀서가 받은 액션에 따라 어떤 응답을 할지 선택한다.
1. `switch`를 쓸 때, `default: return state` 를 꼭 써야한다. 만약 안쓰게되면, `undefined`를 리턴할 수도 있다. (그리고 상태를 잃어버린다.)
1. 현재 상태값과 새로운 상태값 `{ message: action.value }`을 어떻게 합쳤는지 보면, 이건 전부 ES7(Object Spread) 덕분: `{ ...state, message: action.value }`
1. 또한 ES7 Object Spread 는 우리 예제와 찰떡인 것이 `{ message: action.value }` 를 얕은 복사로 상태에 복사한다. (상태의 첫 번째 뎁스의 속성을 완전히 덮어쓴다) 하지만 만약 이보다 복잡한 깊은 데이터 구조였다면, 어떻게 상태를 갱신할 지 선택해야한다:
    - [Immutable.js](https://facebook.github.io/immutable-js/) 를 사용하거나
    - [Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 을 사용하거나
    - 수동 머지를 하거나
    - 아니면 필요에 따라 맞는 다른 전략을 선택해야한다. Redux는 이런 선택에 간섭하지 않는다. (다시 말하지만, 리덕스는 상태 컨테이너이다)

<!-- Now that we're starting to handle actions in our reducer let's talk about having multiple reducers and combining them. -->

reducer에서 액션을 다루어 보았으니 이제 여러개의 reducer들이 있고 그것들을 합쳐보는 것에 대해 얘기해보자.

<!-- Go to next tutorial: 05_combine-reducers.js -->
다음: [05_combine-reducers.md](/2020/03/15/redux-tutorial-05-combine-reducers/)

