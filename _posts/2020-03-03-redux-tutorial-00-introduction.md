---
layout: post
title: redux-tutorial/00_introduction
description: redux 소개
tags: [redux]
---
<!-- Tutorial 0 - introduction.js -->
<!-- 
Why this tutorial?
While trying to learn Redux, I realized that I had accumulated incorrect knowledge about flux through articles I read and personal experience. I don't mean that articles about flux are not well written but I just didn't grasp concepts correctly. In the end, I was just applying documentation of different flux frameworks (Reflux, Flummox, FB Flux) and trying to make them match with the theoretical concept I read about (actions / actions creators, store, dispatcher, etc).
Only when I started using Redux did I realize that flux is more simple than I thought. This is all thanks to Redux being very well designed and having removed a lot of "anti-boilerplate features" introduced by other frameworks I tried before. I now feel that Redux is a much better way to learn about flux than many other frameworks. That's why I want now to share with everyone, using my own words,
flux concepts that I am starting to grasp, focusing on the use of Redux.
-->

👉 https://github.com/happypoulp/redux-tutorial/wiki

### 이 글을 쓰는 이유.
Redux를 공부하던중에 내가 flux 글을 잘못 읽고 있었음을 깨달았다. 그 글이 잘못됐다는건 아니고 내가 잘못 이해하고 있었다. 내가 다른 flux 프레임워크 문서를 읽으면서 개념들을 이해하려하고 있었던것이다.

Redux를 사용하기 시작하면서 flux 가 생각보다 단순하다는 것을 깨달았다. 이건 전부 Redux가 되게 잘 설계되어있고 내가 해봤던 다른 다른 프레임워크에 비해 "anti-boilerplate features" 를 많이 걷어내어준 덕분이다. 다른 프레임워크에비해 Redux는 flux에 대해 알기 쉽다. 그래서 Redux에 초점을 두고 flux 개념에 대해 내가 이해하기 시작한 내용들을 공유하려고 한다.

<!-- You may have seen this diagram representing the famous unidirectional data flow of a flux application: -->

이건 flux 애플리케이션의 단방향 로직을 보여주는 다이어그램
```
/*
                 _________               ____________               ___________
                |         |             |            |             |           |
                | Action  |------------▶| Dispatcher |------------▶| callbacks |
                |_________|             |____________|             |___________|
                     ▲                                                   |
                     |                                                   |
                     |                                                   |
 _________       ____|_____                                          ____▼____
|         |◀----|  Action  |                                        |         |
| Web API |     | Creators |                                        |  Store  |
|_________|----▶|__________|                                        |_________|
                     ▲                                                   |
                     |                                                   |
                 ____|________           ____________                ____▼____
                |   User       |         |   React   |              | Change  |
                | interactions |◀--------|   Views   |◀-------------| events  |
                |______________|         |___________|              |_________|

*/
```

<!-- In this tutorial we'll gradually introduce you to concepts of the diagram above. But instead of trying to explain this complete diagram and the overall flow it describes, we'll take each piece separately and try to understand why it exists and what role it plays. In the end you'll see that this diagram makes perfect sense
once we understand each of its parts. -->

이번 튜토리얼에선 위 다이어그램의 개념을 알아보자.
일단 전체 흐름을 알기 전에 각 부분들이 왜 필요하고 무슨 역할을 하는지 알고나면 이 다이어그램을 완벽하게 이해할 수 있을것이다.

<!-- But before we start, let's talk a little bit about why flux exists and why we need it...
Let's pretend we're building a web application. What are all web applications made of?
1) Templates / html = View
2) Data that will populate our views = Models
3) Logic to retrieve data, glue all views together and to react accordingly to user events, data modifications, etc. = Controller -->

시작하기 앞서, 왜 flux가 필요한지 살짝 얘기해보자면...
우리가 웹 애플리케이션을 만들고 있다고 치자. 웹 애플리케이션은 무엇으로 구성되어 있는가?
1) Models = 뷰에 바인딩될 데이터
2) View = 템플릿, html
3) Controller = 로직, 사용자 이벤트에 따른 반응, 데이터 수정 등

<!-- This is the very classic MVC that we all know about. But it actually looks like concepts of flux, just expressed in a slightly different way:
- Models look like stores
- user events, data modifications and their handlers look like
  "action creators" -> action -> dispatcher -> callback
- Views look like React views (or anything else as far as flux is concerned) -->

이게 우리가 알고있는 가장 정통적인 MVC 인데, flux도 이와 살짝만 다를 뿐 비슷하다.
1) Store 는 Models
2) React Views 는 View
3) Action Creators -> Action -> Dispatcher -> callbacks 가 사용자 이벤트, 데이터 조작, 등록된 이벤트들

<!-- So is flux just a matter of new vocabulary? Not exactly. But vocabulary DOES matter, because by introducing these new terms we are now able to express more precisely things that were regrouped under various terminologies... For example, isn't a data fetch an action? Just like a click is also an action?
And a change in an input is an action too... Then we're all already used to issuing actions from our applications, we were just calling them differently. And instead of having handlers for those actions directly modify Models or Views, flux ensures all actions go first through something called a dispatcher, then through our stores, and finally all watchers of stores are notified. -->

그렇다고 flux의 새로운 어휘들이 문제일까? 아니, 하지만 중요하다. 다양한 용어들로 재정의된것들을 더 정확하게 표현할 수 있는 새로운 용어이기 때문이다. 예를들어, 데이터를 가져오는게 액션일까? 그냥 클릭하는것도 액션일가? 인풋이 바뀌는것도 액션인가?... 그렇다면 우린 앱에서 액션을 이미 사용해보았다. 우린 그저 그것을 다르게 해봤을 뿐이다. 그리고 모델이나 뷰를 직접 수정하는 액션 핸들러 대신에 flux는 액션을 맨 처음 dispatcher 라고 불리우는 것을 통해 전달하고 그런 다음 stores, 그리고 stores를 바라보고있던 것들은 알림을 받는다.

<!-- To get more clarity how MVC and flux differ, we'll take a classic use-case in an MVC application:
In a classic MVC application you could easily end up with:
1) User clicks on button "A"
2) A click handler on button "A" triggers a change on Model "A"
3) A change handler on Model "A" triggers a change on Model "B"
4) A change handler on Model "B" triggers a change on View "B" that re-renders itself -->

MVC와 flux의 차이점을 더 분명히 하기위해서 MVC 애플리케이션 예제로 살펴보자:
MVC 애플리케이션에서는:
1) 사용자가 버튼 "A" 를 클릭한다
2) 버튼 "A" 에 등록되어있는 클릭 이벤트가 모델 "A" 를 변경시킨다
3) 모델 "A" 에 등록되어있는 체인지 이벤트가 모델 "B" 를 변경시킨다
4) 모델 "B" 에 등록되어있는 체인지 이벤트가 뷰 "B"를 다시 렌더링하게한다

<!-- Finding the source of a bug in such an environment when something goes wrong can become quite challenging very quickly. This is because every View can watch every Model, and every Model can watch other Models, so
basically data can arrive from a lot of places and be changed by a lot of sources (any views or any models). -->

이런 환경에서 버그를 찾긴 어렵다. 왜냐면 뷰는 모든 모델을 바라볼 수 있고, 모든 모델은 다른 모델을 바라볼 수 있어서 데이터는 여러 곳에서 유입되고 많은 곳(모델이 될수도 뷰가 될 수도 있다)에서 영향을 받을 수 있다.

<!-- Whereas when using flux and its unidirectional data flow, the example above could become:
1) user clicks on button "A"
2) a handler on button "A" triggers an action that is dispatched and produces a change on Store "A"
3) since all other stores are also notified about the action, Store B can react to the same action too
4) View "B" gets notified by the change in Stores A and B, and re-renders -->

flux와 flux의 단방향 데이터 흐름을 이용하면 위 예제는 아래와 같아진다:
1) 사용자가 버튼 "A" 를 클릭한다
2) 버튼 "A" 에 등록되어있는 클릭 이벤트를 받은 action을 트리거하고 Store "A"에 변경사항을 보낸다
3) 다른 store들도 action에 대한 알림을 받기때문에, Store "B"도 같은 action에 반응할 수 있다
4) 뷰 "B"는 Store "A"와 "B"의 변경사항에 대해 알림을 받고 다시 렌더링한다

<!-- See how we avoid directly linking Store A to Store B? Each store can only be modified by an action and nothing else. And once all stores have replied to an action, views can finally update. So in the end, data always flows in one way: action -> store -> view -> action -> store -> view -> action -> ... -->

어떻게 Store A에서 Store B로 직접 연결하지 않았는지 알 수 있었다. 각각의 store는 오직 action에 의해서만 수정될 수 있고 한번 action에 의해서 반응하기만하면, 뷰들은 업데이트된다. 결국 데이터는 단방향으로 흐르게 된다: action -> store -> view -> action -> store -> view -> action -> ...

<!-- Just as we started our use case above from an action, let's start our tutorial with actions and action creators. -->

위에서 action을 예제로 시작해보았던 것처럼 action creator 에 대해서도 알아보자.

<!-- Go to next tutorial: 01_simple-action-creator.js -->

다음: [01_simple-action-creator.md](./01_simple-action-creator.md)

