---
title: Typescript Literal Types
date: 2022-05-15 21:26 + 0900
category: [Typescript]
---

## Literal Types

string, number, boolean과 같은 것만 타입이 될 수 있는것이 아니다. 일반 글자같은 것도 타입이 될 수 있다.

```ts
// 이런식으로 원하는 타입을 만들 수 있다.
// student랑 office worker라는 타입을 만든 것이다.
let me: "student";
let you: "office worker";
```

이렇게 작성하면 이제 me 라는 변수에는 student라는 글자만 할당할 수 있고, you 라는 변수에는 office worker라는 글자만 할당할 수 있다. (or 기호를 사용한 union type도 지정 가능)

또한 함수도 마찬가지로 원하는 타입으로 지정해줄 수 있다.

```ts
function func(a: "hello"): 1 | 0 | -1 {
  return 1;
}
```

파라미터 타입 선언 시 글자나 숫자를 넣으면 해당 타입만 파라미터로 넣을 수 있고 return 타입 선언 시에도 글자나 숫자를 넣으면 해당 타입만 return 할 수 있다.

이렇게 **특정 글자나 숫자만 가질 수 있게 제한을 두는 것**을 **Literal Type** 이라고 한다.

즉, 정리해보면 Literal Type 은

- 들어올 수 있는 자료를 미리 정해(제한해)두는 것
- 변수에 뭐가 들어올지 더 엄격하게 관리 가능

<br>

## const 변수의 한계

const 변수는 2개 이상의 값을 저장하지 못함. 그러나 Literal Type 쓰면 or 기호를 통해 2개 이상의 값을 저장할 수 있음.

<br>

## Literal Type의 문제점과 as const

만약 아래와 같은 과정이 있다고 가정해보자.

```ts
let object = {
  name: "kim",
};

function func(a: "kim") {}

func("kim"); // OK
func(object.name); // Error
```

위의 코드를 보면 분명 object.name == 'kim' 이다.

그런데 func('kim')은 오류가 나지 않지만, func(object.name)은 object.name이 'kim' 인데도 불구하고 에러가 발생한다.

이러한 오류가 발생하는 이유는 바로 `(a: 'kim')이라고 지정한 것은 'kim'이라는 자료만 들어올 수 있다가 아닌 'kim'이라는 타입만 들어올 수 있다를 의미`하기 때문이다.

따라서 `object.name은 타입이 'kim'이 아닌 string이기 때문에` 오류가 발생한다고 할 수 있다.

이러한 문제를 해결하기 위한 해결책에는 크게 3가지가 있다.

**해결책(1)**
object 만들 때 직접 하나하나 타입 지정을 확실하게 해준다.

**해결책(2)**
Assertion 문법을 사용해 as 문법으로 타입을 속인다.

**해결책(3)**
as const 라는 키워드를 사용한다.

```ts
let object = {
  name: "kim",
} as const;
```

as const는 해당 속성의 타입을 오른쪽에 있는 값으로 지정해주라는 의미를 가지고 있다. 또한 object 속성들에 모두 readonly를 붙여준다.

<br>

## 실습 해보기

Q1. 아래 조건에 맞는 함수를 작성해보자.

- '가위', '바위', '보' 문자들만 파라미터로 입력할 수 있다.
- '가위', '바위', '보' 라는 문자들만 담을 수 있는 array 자료만 return 할 수 있다.
- 예를 들어 ['가위', '보', '가위']와 같이 return 가능하다.
- ['가위', '바보'] 이런거 return 하면 에러가 발생해야 한다.

```ts
function RCP(x: "가위" | "바위" | "보"): ("가위" | "바위" | "보")[] {
  return ["가위", "보"];
}
```
