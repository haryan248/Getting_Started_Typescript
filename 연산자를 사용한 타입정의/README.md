# 1. 유니온 타입

<hr >

### 유니온 타입이란?

여러 개의 타입 중 한개만 쓰고 싶을 때 사용하는 문법

장점

- string, number 에서 제공하는 **_toString()_**을 자동완성 시켜줍니다.
- 오탈자가 발생해도 error를 내줘 any와 다르게 유용합니다.

```typescript
function logText(text: string | number) {
  console.log(text.toString());
}

function logText(text: any) {
  console.log(text.toString());
}
```

### 주의할 점

```typescript
interface Person {
  name: string;
  age: number;
}

interface Developer {
  name: string;
  skill: string;
}

function introduce(someone: Person | Developer) {
  console.log(someone.age); // error
  console.log(someone.skill); // error
}
```

someone은 Person 또는 Developer이기 때문에 두 객체에 있는 타입일때만 error가 나지 않습니다.
어떤 타입이 올지 모르기 때문에 in 연산자를 사용해 로직을 작성해야 합니다.

```typescript
function introduce(someone: Person | Developer) {
  if ("age" in someone) {
    console.log(someone.age);
    return;
  }
  if ("skill" in someone) {
    console.log(someone.skill);
    return;
  }
}
```

각각 속성이 있는지 판단하면 typescript는 someone의 객채를 올바르게 추론할 수 있습니다.

처음 에시도 타입을 추론할 수 있게 바꿔보면

```typescript
function logText(text: string | number) {
  if (typeof text === "string") {
    console.log(text.toUpperCase());
    // text는 string으로 추론
  }
  if (typeof text === "number") {
    console.log(text.toLocaleString());
    // text는 number로 추론
  }
}
```

이렇게 typeof나 in연산자를 통해 특정 타입의 속성과 메소드만 사용하게 막는 것을 **_타입가드_** 라고 합니다.

# 2. 인터섹션 타입

인터섹션 타입이란 타입 2개를 하나로 합쳐서 사용할 수 있는 타입입니다.

```typescript
interface Avenger {
  name: string;
}
interface Hero {
  skill: string;
}
function introduce(someone: Avenger & Hero) {
  console.log(someone.name);
  console.log(someone.skill);
}

introduce({ name: "캡틴" });
// 둘중 하나의 속성이라도 빠질 경우 에러가 발생합니다.
```

someone 파라미터로는 두가지 속성을 모두 사용할 수 있고, 두인자를 넘겨받을 수 있습니다.
