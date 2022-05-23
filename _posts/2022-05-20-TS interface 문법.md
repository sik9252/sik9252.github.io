---
title: Typescript interface 문법
date: 2022-05-20 18:25 + 0900
category: [Typescript]
---

전에 type 키워드로 타입변수를 생성할 수 있다는 것을 배웠다. 오늘은 interface 라는 것에 대한 공부를 할 것인데, 이 키워드도 타입변수를 생성할 수 있다.

## interface 문법

object 타입지정시 사용 가능하다.

```ts
// 이전까지 object에 타입지정하던 방법
type Square = { color: string; width: number };
let square: Square = { color: "red", width: 100 };

// interface 문법을 사용하여 타입지정하는 방법
interface Square {
  color: string;
  width: number;
}
```

<br>

## interface의 장점 - extends

```ts
interface Student {
  name: string;
}
// Students interface가 가지고 있는 속성을
// Teacher에 복사해서 넣어라
interface Teacher extends Student {
  aeg: number;
}

let student: Student = { name: "kim" };
let teacher: Teacher = { name: "lee", age: 24 };
```

<br>

## intersection type

type alias 문법도 interface의 extends와 비슷한 행동을 하도록 할 수 있다. 바로 `& 기호`를 사용하는 것인데,

```ts
type Animal = { name: string };
type Cat = { age: number } & Animal;
```

이런식으로 하나의 타입변수를 만든 후 다른 하나의 타입변수에 & 기호를 이용해 서로 묶어주면 Cat 라는 타입변수는 Animal 타입의 속성도 가지게 된다.

**하지만 주의할점**

& 기호를 사용하는 것은 extends랑은 다른 의미를 가지고 있음에 주의해야 한다.

- & 기호: intersection type으로 두 타입을 전부 만족하는 타입이라는 뜻 (복사해달라는 뜻 아님)

- extends: 다른 하나가 가지고 있는 것을 복사해서 넣어달라는 뜻

그리고 interface도 & 기호를 사용 가능함.

<br>

## 그래서 type과 interface 차이는?

- interface: 중복선언 가능
- type: 중복선언 불가능

예를들어 interface는

```ts
interface Student {
  name: string;
}
interface Student {
  score: number;
}
```

이렇게 같은 이름을 가진 interface가 하나 이상 있어도 에러가 나지 않고, extends 한 것이랑 동일하게 Student라는 타입에는 name과 score가 모두 들어가도록 합쳐진다.

반면, type은 엄격해서 에러를 뱉는다.

> interface는 type 선언을 자주 사용하는 외부 라이브러리 이용시 type 선언을 내가 override 하기 편함!

**interface 이름 중복만 허용이지 안의 속성이 중복되는 경우에는 에러가 발생함에 주의**

<br>

## 실습 해보기

**Q1. interface 이용해서 간단한 타입을 만들어보자.**

```ts
// 요것의 타입지정을 해보자
let prod = { brand: "Samsung", serialNumber: 1360, model: ["TV", "phone"] };
```

```ts
// interface
interface ProductType {
  brand: string;
  serialNumber: number;
  model: string[];
}

let prod: ProductType = {
  brand: "Samsung",
  serialNumber: 1360,
  model: ["TV", "phone"],
};
```

<hr>

**Q2. 아래처럼 생긴 object들이 잔뜩 들어갈 수 있는 array의 타입지정을 interface 문법을 사용해서 해보자.**

```ts
let cart = [
  { product: "청소기", price: 7000 },
  { product: "삼다수", price: 800 },
];
```

```ts
// 타입은 그냥 이렇게 만들고
interface Cart {
  product: string;
  price: number;
}

// 만든 타입 사용할 때 array 타입으로 사용하면 됨
let cart: Cart[] = [
  { product: "청소기", price: 7000 },
  { product: "삼다수", price: 800 },
];
```

<hr>

**Q3. 위에서 만든 타입을 extends 해보자.**

- 갑자기 서비스가 업데이트되어 일부 상품에는 card 속성이 들어가게 되었다.

```ts
// 기존
interface Cart {
  product: string;
  price: number;
}

// extends
interface newCart extends Cart {
  card: boolean;
}
```
