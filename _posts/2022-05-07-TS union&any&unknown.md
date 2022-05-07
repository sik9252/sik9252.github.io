---
title: Typescript 타입을 미리 정하기 애매할 때
date: 2022-05-07 11:44 + 0900
category: [Typescript]
---

해당 변수에 들어올 값이 A 타입일지 B 타입일지 애매할 때 사용할 수 있는 방법들이 있다.

## Union Type

타입 2개 이상을 합친 새로운 타입을 만드는 것으로 `해당 변수에는 A 혹은 B 타입이 들어올 수 있습니다.` 라는 타입 정의를 할 수 있도록 해준다.

```ts
let name: string | number = "Lee"; // string 타입
nane = 100; // number 타입
```

`let name: (string | number)`와 같이 소괄호를 쳐도 된다.

array 혹은 object 자료를 만들 때 union type은 아래와 같이 허용할 타입을 소괄호로 묶어 사용한다.

```ts
let array: (number | string)[] = [1, "2", 3];
let object: { data: number | string } = { data: "123" };
```

다만, 여기서 알면 좋은 점은 **변수에 정의된 union type은 값 할당과 동시에 OR의 역할이 사라지**는데, **배열이나 객체에 정의된 union type은 값이 할당되어도 OR의 역할이 유지**된다.

## any type

any type은 모든 자료형을 허용해주는 타입이며 아래와 같이 사용한다.

```ts
let name: any;
name = 모든 자료형 가능
```

다만, 이것은 타입스크립트의 의미가 사라지므로 타입 관련 버그나 나도 잡아주지 않는 `타입 실드 해제 문법`이다.

```ts
let name: any;
name = 100;
name = {};

// 실드를 완전히 해제하기 때문에 오류가 나지 않는다
let testVar: string = name;
```

따라서 코드를 위와 같이 작성했을 때 testVar 변수에 any type을 가진 name이란 값이 할당되었으므로 testVar 변수의 실드가 완전히 해제되어 오류가 발생하지 않는다.

> any는 타입 실드를 해제하므로 타입스크립트를 쓰는 이유를 없애기 때문에 비상시 **변수 타입체크 해제기능** 용도로 사용하자.

## unknown type

any의 용도와 같다. 모든 자료형을 허용해준다.

```ts
let name: unknown;
name = 모든 자료형 가능
```

다만, 얘는 any보다 조금 더 안전하다고 할 수 있는데, 그 이유는 아래와 같은 코드를 사용했을 때, 아까 위에서 타입 실드가 적용된 다른 변수에 any를 사용한 변수를 할당했을 때는 오류가 발생하지 않았는데 이번에는 해당 변수에서 타입 오류가 발생하게 된다.

```ts
let name: unknown;
name = 100;
name = {};

// string만 사용가능하다는 오류가 발생한다.
let testVar: string = name;
```

## 타입스크립트의 엄격함

```ts
let age: string | number;
age + 1; // 오류!
```

Q. 자바스크립트에서는 string 자료형에도 number 자료형에도 +1이 가능하다. 근데 왜 위 코드에서는 에러가 나는 것일까?

A. 타입스크립트는 타입 엄격한것을 좋아한다! 따라서 `string + 1, number + 1은 각각 허용`하지만, `string | number + 1은 허용하지 않는 것`이다.

_string | number 이것도 하나의 새로운 타입임에 주의해야 한다._

## 실습 해보기

Q1. 다음 변수 4개에 타입을 지정해보자. age에는 숫자도 들어올 수 있다.

```ts
let user = "kim";
let age = undefined;
let married = false;
let userInfo = [user, age, married];
```

### 내 풀이

```ts
let user: string = "kim";
let age: undefined | number = undefined;
let married: boolean = false;
let userInfo: (string | number | undefined | boolean)[] = [user, age, married];
```

<br>

Q2. 타입 지정을 안했더니 아래 코드를 실행했을 때, 터미널에 에러가 발생한다. 에러가 발생하지 않게 school 이란 변수에 타입을 지정해보자.

```ts
let school = {
  score: [100, 97, 84],
  teacher: "Phil",
  friend: "John",
};

school.score[4] = false;
school.friend = ["Lee", school.teacher];
```

### 내 풀이

1. 기본적으로 score은 string[] 타입, teacher과 friend는 string 타입이다.

2. school.score[4] = false; -> boolean 타입을 추가해야 할 것 같다.

3. school.friend = ["Lee", school.teacher];
   -> friend에 string[] 타입을 추가해야 할 것 같다.

```ts
let school: {
  score: (number | boolean)[];
  teacher: string;
  friend: string | string[];
} = {
  score: [100, 97, 84],
  teacher: "Phil",
  friend: "John",
};

school.score[4] = false;
school.friend = ["Lee", school.teacher];
```
