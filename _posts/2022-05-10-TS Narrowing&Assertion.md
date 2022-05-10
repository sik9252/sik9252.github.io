---
title: Typescript Narrowing & Assertion
date: 2022-05-19 19:07 + 0900
category: [Typescript]
---

## Type Narrowing

Type이 아직 하나로 확정되지 않았을 경우(ex. union type) 사용한다.

```ts
function Test(x: number | string) {
  // 파라미터 타입이 number인 경우
  if (typeof x === "number") {
    return x + 1;
    // 파라미터 타입이 string인 경우
  } else if (typeof x === "string") {
    return x + 1;
    // 그 외의 경우
  } else {
    return 0;
  }
}
```

위처럼 if, else if, else 구문과 typeof를 사용해서 작성하는 것이 대표적인 Narrowing 방법이지만, in, instanceof 키워드를 사용한 Narrowing 방법도 있다.

<br>

## Type Assertion

타입을 덮어쓰는 문법으로 해당 변수의 타입을 X로 생각해주세요. 라는 의미이다. 얘는 그냥 **타입을 X처럼 생각해주라고 주장하는 역할**을 하지 **실제로 타입을 X로 바꿔주는 역할은 하지 않는다.**

```ts
function 내함수(x: number | string) {
  // as를 사용한 여기가 Assertion 문법
  return (x as number) + 1;
}
console.log(내함수(123));
```

그런데 편하다고 막 쓰면 안되고 이것의 용도는 아래와 같다.

1. union type과 같은 여러개의 복잡한 타입을 하나로 확정짓고 싶을 때 사용한다.
2. 어떤 타입이 들어올지 100% 확실하게 알고 있을 때 쓴다.

결론적으로는 딱히 쓸 일이 많이 없는것 같다. 왜 타입 에러가 나는지 정말 모르겠는 상황에 임시적인 에러 해결 방법 또는 어떤 타입이 들어와야 하는지 확실하게 알고 있는데 컴파일러 에러가 발생할 때 디버깅 용으로 사용하는 것이 좋다.

<br>

## 실습 해보기

Q1. 숫자와 문자가 섞여있는 배열을 숫자만 들어있는 배열로 클리닝하는 함수를 작성해보자.

```ts
function Cleaning(array: (number | string)[]) {
  let Cleaned: number[] = [];

  array.forEach((elem) => {
    if (typeof elem === "string") {
      Cleaned.push(parseInt(elem));
    } else {
      Cleaned.push(elem);
    }
  });
  return Cleaned;
}
```

<hr>

Q2. 선생님 object 자료를 파라미터로 전달하면 해당 선생님이 가르치고 있는 과목 중 맨 뒤의 1개를 반환하는 함수를 작성해보자.

아래와 같은 선생님들이 있다고 가정하자.

```ts
let 철수쌤 = { subject: "math" };
let 영희쌤 = { subject: ["science", "english"] };
let 민수쌤 = { subject: ["science", "art", "korean"] };
```

```ts
function TeachingSubject(teacher: { subject: string | string[] }) {
  // subject 타입이 string일때
  if (typeof teacher.subject === "string") {
    return teacher.subject;
    // subject 아팁이 배열일때
  } else if (Array.isArray(teacher.subject)) {
    return teacher.subject[teacher.subject.length - 1];
    // 그 외의 타입일때
  } else {
    return "과목이 없습니다";
  }
}
```
