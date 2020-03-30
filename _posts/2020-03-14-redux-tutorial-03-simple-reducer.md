---
layout: post
title: redux-tutorial/03_simple reducer
description: redux reducer
tags: [redux]
---
👉 https://github.com/happypoulp/redux-tutorial/wiki
<!-- Tutorial 03 - simple-reducer.js -->
# Tutorial 03 - simple reducer

<!-- Now that we know how to create a Redux instance that will hold the state of our application we will focus on those reducer functions that will allow us to transform this state. -->

어떻게 애플리케이션의 상태를 보관할 Redux 인스턴스를 생성하는지 배웠다. 이제 이 상태를 변경하는 리듀서 함수들에 집중해보자.

<!-- A word about reducer VS store:
As you may have noticed, in the flux diagram shown in the introduction, we had "Store", not
"Reducer" like Redux is expecting. So how exactly do Store and Reducer differ?
It's more simple than you could imagine: A Store keeps your data in it while a Reducer doesn't.
So in traditional flux, stores hold state in them while in Redux, each time a reducer is
called, it is passed the state that needs to be updated. This way, Redux's stores became
"stateless stores" and were renamed reducers. -->

reducer VS store:

알아차렸을 지 모르겠지만, 처음 소개할 때 보여준 flux 다이어그램에서 Redux의 "Reducer"가 아닌 "Store" 가 있었다. 이 두개는 어떻게 다른걸까?
생각보다 쉽다: Store는 Reducer 와 다르게 데이터를 보관한다.
그래서 flux에서 Store는 상태를 보관하고 있지만 Redux에서는 매번 reduce가 호출된다. 상태가 갱신되어야할 때마다 reduce에 전달되는 것이다. 이런식으로, Redux의 상태는 상태가 없는 store(stateless store)가 되고 이걸 reducer 라고 부른다.

<!-- As stated before, when creating a Redux instance you must give it a reducer function...
import { createStore } from 'redux'

var store_0 = createStore(() => {}) -->

이전에 말한대로, Redux 인스턴스를 생성할 때 reducer 함수를 전달해야한다.
```js
import { createStore } from 'redux'

var store_0 = createStore(() => {})
```

<!-- ... so that Redux can call this function on your application state each time an action occurs.
Giving reducer(s) to createStore is exactly how Redux registers the action "handlers" (read reducers) we
were talking about in section 01_simple-action-creator.js. -->

... 이렇게 Redux는 액션이 발생할 때마다 애플리케이션 상태에대해 이 함수를 호출하게된다.
reducer를 createStore에 전달하는 게 [01_simple-action-creator.md](./01_simple-action-creator.md) 에서 말했었던 Redux에서 액션 "핸들러"를 등록하는 방법이다.

<!-- Let's put some log in our reducer

var reducer = function (...args) {
    console.log('Reducer was called with args', args)
}

var store_1 = createStore(reducer)

Output: Reducer was called with args [ undefined, { type: '@@redux/INIT' } ] -->

reducer 에 로그를 심어보자.
```js
var reducer = function (...args) {
    console.log('Reducer was called with args', args)
}

var store_1 = createStore(reducer)

// Output: Reducer was called with args [ undefined, { type: '@@redux/INIT' } ]
```

<!-- Did you see that? Our reducer is actually called even if we didn't dispatch any action...
That's because to initialize the state of the application,
Redux actually dispatches an init action ({ type: '@@redux/INIT' }) -->

어떤가? 우리가 아무런 액션을 dispatch하지 않았는데도 reducer가 호출되었다...
애플리케이션의 상태를 초기화해야하기 때문이다, 사실 Redux는 초기 액션을 dispatch 한다. `({ type: '@@redux/INIT' })`

<!-- When called, a reducer is given those parameters: (state, action)
It's then very logical that at an application initialization, the state, not being
initialized yet, is "undefined" -->

호출되게되면, reducer는 이런 파라미터를 받게되는데: (state, action)
이건 애플리케이션 초기화에서 매우 논리적이다. 상태는 아직 초기화되지 않았기 때문에 "undefined" 이다. 

<!-- But then what is the state of our application after Redux sends its "init" action? -->

그렇지만 Redux가 "init" 액션을 보내고 나서 애플리케이션의 상태는 무엇이될까?

<!-- Go to next tutorial: 04_get-state.js -->
다음: [04_get-state.md](/2020/03/14/redux-tutorial-04-get-state/)
