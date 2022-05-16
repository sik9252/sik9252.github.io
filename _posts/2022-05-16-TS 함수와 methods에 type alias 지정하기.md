---
title: Typescript 함수와 methods에 type alias 지정하기
date: 2022-05-16 23:30 + 0900
category: [Typescript]
---

## 함수 타입도 type alias로 지정할 수 있다

```ts
type FuncType = (x: number, y: number) => number;
```

이렇게 화살표 함수를 이용해서 만든다.

이제 만든 함수 타입을 부착하려면 함수 표현식을 사용해야 한다.

```ts
let funcName: FuncType = function () {};
```

정리하면 아래와 같다.

- 함수 타입은 () => {} 형태로 만든다.
- 함수 표현식에만 만든 type alias 사용이 가능하다.

<br>

## methods 안에 타입 지정하기

object 안에 함수를 만들 수 있다는 사실을 알고 있는가?

```ts
let memberInfo = {
  name: "kim",
  plusOne() {
    // 이렇게 함수 지정이 가능함
  },
  // 화살표 함수도 가능
  changeName: () => {},
};

// 함수를 사용할 때는 이렇게
memberInfo.plusOne();
```

Q1. 위처럼 사용할 수 있는데 이렇게 object 안에 있는 함수의 타입 지정은 어떻게 할까? 아래 조건을 만족하도록 memberInfo 변수에 타입 지정을 해보자.

- plusOne 속성은 함수이고, 숫자를 넣어서 숫자를 반환하는 함수여야한다.
- changeName 속성은 함수이고, 아무것도 반환해서는 안된다.

```ts
type MemberInfoType = {
  name: string;
  plusOne: (x: number) => number;
  changeName: () => void;
};

let memberInfo: MemberInfoType = {
  name: "kim",
  plusOne() {},
  changeName: () => {},
};
```

<br>

## 실습 해보기

Q2. 아래 조건을 만족하는 함수 2개를 만들어보고 타입을 정의해보자.

- cutZero() 라는 함수를 만들자. 이 함수는 문자를 하나 입력했을 때, 맨 앞에 '0'이 있으면 이를 제거하고 문자 타입으로 반환해준다.
- removeDash() 라는 함수를 만들자. 이 함수는 문자를 하나 입력했을 때 '-'가 있다면 이를 전부 제거하고 숫자 타입으로 반환한다.
- 함수에 타입 지정 시 type alias를 사용한다.

```ts
// cutZero 함수 타입
type cutZeroType = (x: string) => string;
// removeDash 함수 타입
type removeDashType = (x: string) => number;

// fuction cutZero
let cutZero: cutZeroType = (x) => {
  let answer = x.replace(/^0+/, "");
  return answer;
};

// function removeDash
let removeDash: removeDashType = (x) => {
  let answer = x.replace(/-/g, "");
  return parseInt(answer);
};
```

<hr>

Q3. Q2 에서 만든 함수들을 파라미터로 넣을 수 있는 함수를 제작하고 싶다. 이 때 이 함수는 파라미터 3개가 들어가는데 첫째는 문자, 둘째와 셋째는 함수를 집어넣을 수 있다. 이 함수를 실행하면 아래와 같은 동작을 수행하게 된다.

- 첫째 파라미터를 둘째 파라미터에 파라미터로 집어넣는다.
- 둘째 파라미터에서 반환된 결과를 셋째 파라미터에 집어넣는다.
- 셋째 파라미터에서 반환된 결과를 콘솔창에 출력한다.

그리고 둘째 파라미터에는 cutZero, 셋째 파라미터엔 removeDash 라는 함수들만 입력할 수 있게 파라미터의 타입도 지정해보자.

```ts
// 둘째 파라미터에 들어올 함수 타입 지정
type func1Type = (x: string) => string;
// 셋째 파라미터에 들어올 함수 타입 지정
type func2Type = (x: string) => number;
// 원하는 조건의 동작을 수행할 함수의 타입 지정
type newFuncType = (x: string, func1: func1Type, func2: func2Type) => void;

// 원하는 조건의 동작을 수행할 메인 함수
let newFunc: newFuncType = (x, func1, func2) => {
  let answer = func1(a);
  let result = func2(answer);
  console.log(result);
};

newFunc("010-1111-2222", cutZero, removeDash);
```

Q2 에서 작성한 코드와 합친 전체 코드 답안

```ts
type cutZeroType = (x: string) => string;
type removeDashType = (x: string) => number;

let cutZero: cutZeroType = (x) => {
  let answer = x.replace(/^0+/, "");
  return answer;
};

let removeDash: removeDashType = (x) => {
  let answer = x.replace(/-/g, "");
  return parseInt(answer);
};

type func1Type = (x: string) => string;
type func2Type = (x: string) => number;
type newFuncType = (x: string, func1: func1Type, func2: func2Type) => void;

let newFunc: newFuncType = (x, func1, func2) => {
  let answer = func1(x);
  let result = func2(answer);
  console.log(result);
};

newFunc("010-1111-2222", cutZero, removeDash);
```
