---
layout: post
title: skipWhile, switchMap 이해하기
tags: [rxjs, skipWhile, switchMap]
---
RxJS의 operator 중 filtering 역할을 하는 **skipWhile** 과 transformation 역할을 하는 **switchMap** 두 가지를 써본 사례를 공유해보려한다.

## 문제
비동기로 통신하는 두 개의 API가 있다고 할 때, 먼저 하나의 API의 요청이 끝나면 그 응답결과를 받아서 다른 하나의 API요청을 보내야 한다.
(먼저 보낸 API가 아직 응답을 받지 못했다면, 기다렸다가 나머지 요청을 보내야한다)

예를 들어, 주문금액을 계산하는 API(amount)와 결제를 요청하는 API(order)가 있다.
amount API 는 쿠폰을 변경할 때 마다 요청을 보내서 UI에서 주문 예상 금액을 보여준다.
order API 는 a에서 받은 최종 주문 예상 금액을 가지고 결제를 요청한다.
이때 만약 amount API의 응답이 늦어지는 상황인데 응답을 받기도전에 order API를 요청해버리면 최종 결제금액을 넘기지 못하고 결제오류가 발생하게 된다.

이것을 어떻게 해결할 수 있을까?

## 해결
BehaviorSubject로 amount API의 상태를 관리하기로 하였다.

1. amount API가 pending 상태인지(true) 아닌지(false)를 체크하기 위해 먼저 isPending 이라는 변수에 상태를 false로 초기화해준다.
```
couponChange() {
  // 초기화
  const isPending = new BehaviorSubject(false);
}
```
2. 쿠폰 변동에 따라 호출되는 함수(couponChange)에서 amount API를 호출하고 응답을 받으면 isPending에 true로 값을 넘긴다.
```
couponChange() {
  const isPending = new BehaviorSubject(false);
  http.get(‘amount’).subscribe(res => {
    // isPending = true
    isPending.next(true);
  });
}
```
3. order API를 호출하기 전에 isPending 값을 가져와서 pending 상태가 아닐때(false) API를 요청할 수 있게 한다.
```
order() {
  isPending
    .skipWhile(res => res)			// isPending 이 true 일 때는 건너뛰고(skip) false 일 때 요청할 수 있게됨
    .switchMap(() => http.post(‘order’))	// 이전의 API요청을 취소해줌. 혹 API요청이 되었더라도 취소하고 다시 요청함
    .subscribe(res => {
      // …
    });
}
```

### skipWhile
그림으로 표현하자면 쿠폰에 변동이 생길 때마다 pending 상태를 보내고
skipWhile 이 pending 상태(빨간공) 일때는 계속 건너뛰다가
pending 상태가 아닐 때(파란공)부터 Observable을 방출(emit)해서 order API요청을 가능하게 한다.
![discard items emitted by an Observable until a specified condition becomes false](/images/posts/2019-04-04/skipwhile.png)
왼쪽은 ReactiveX 공식사이트에서 [SkipWhile](http://reactivex.io/documentation/operators/skipwhile.html) operator 를 설명하는 이미지고, 오른쪽은 내가 사용한 skipWhile 에 대해 그림으로 표현해본 이미지다.

skipWhile 로 pending 상태일 때는 요청을 건너뛰고 보내지않게 되었다.
￼￼![skipped request](/images/posts/2019-04-04/skipwhile-pending.png)

### switchMap
skipWhile을 쓰지않고 switchMap만 쓰게되면, 이전의 order API를 취소하는 모습을 다음과 같이 크롬 Network 패널에서 확인할 수 있다.
하지만 skipWhile을 쓰면 요청을 못보내게해서 이렇게 취소되는 경우는 없어졌다.
￼￼![cancled api](/images/posts/2019-04-04/switchmap.png)


참고로 swithcMap을 쉽게 이해하게 도와준 동영상
https://egghead.io/lessons/rxjs-starting-a-stream-with-switchmap?course=step-by-step-async-javascript-with-rxjs
