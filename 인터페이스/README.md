# 1. 인터페이스란?

인터페이스(interface)는 객체 타입을 정의할 때 사용하는 문법입니다.

인터페이스 타입으로 사용되는 곳

- 객체의 속성과 속성 타입
- 함수의 파라미터와 반환 타입
- 함수의 스펙(파라미터 개수와 반환값 여부 등)
- 배열과 객체를 접근하는 방식
- 클래스

# 2. 인터페이스를 이용한 함수 타입 정의

객체는 함수의 파라미터 또는 반환값으로 사용되기에 인터페이스를 통해 타입 선언이 가능합니다.

```typescript
interface Person {
  name: string;
  age: number;
}

function logAge(someone: Person) {
  console.log(someone.age);
}
```

# 3. 인터페이스의 옵션 속성

인터페이스에서도 옵션 속성을 사용하여 선택적으로 값을 넘길 수 있습니다.

```typescript
interface Person {
  name?: string;
  age: number;
}

function logAge(someone: Person) {
  console.log(someone.age);
}

logAge({ age: 100 });
```

# 4. 인터페이스 상속

인터페이스 상속은 extends 예약어를 사용합니다.

```typescript
// 인터페이스 확장
interface Person {
  name: string;
  age: number; // 옵셔널 선택자 ? 동일하게 적용 가능
}
interface Developer extends Person {
  language: string;
}
const joo: Developer = { name: "joo", age: 20, language: "ts" };
```

인터페이스의 상속은 여러번 받아서 정의할 수 있습니다.

```typescript
interface Hero {
  power: boolean;
}
interface Person extends Hero {
  name: string;
  age: number; // 옵셔널 선택자 ? 동일하게 적용 가능
}
interface Developer extends Person {
  language: string;
}
```

# 5. 인덱스 시그니처란 ?

```typescript
interface SalaryMap {
  [level: string]: string;
}
```

정확한 속성 이름을 명시하지 않고 속성 이름의 타입과 속성 값의 타입을 정의하는 문법을 인덱스 시그니처(index signature)라고 합니다.

예를 들어 다음과 같은 객체가 있을 때는 아래와 같이 타이핑을 하게 됩니다.

```typescript
var salary = {
  junior: "100",
};

interface SalaryInfo {
  junior: string;
}
```

그러나 계속해서 객체가 늘어남에 따라 인터페이스도 수정해야 합니다.

```typescript
var salary = {
  junior: "100",
  mid: '400'
  senior: '700'
};

interface SalaryInfo {
  junior: string;
  mid: string;
  senior: string;
}
```

인덱스 시그니처를 활용하면 계속해서 추가되더라도 유연하게 사용이 가능합니다.

### 인덱스 시그니처는 언제 사용해야 할까?

인덱스 시그니처를 사용할경우 에디터에서 자동완성이 되지 않습니다.
구체적인 속성을 지정해주고 싶을때는 타입을 명시하는 것이 좋습니다.

```typescript
interface User {
  [property: string]: string;
  id: string;
  name: string;
}
```

이처럼 타입을 지정하면 id, name은 추론해주고 이외에도 string으로 이뤄진 키값은 계속해서 추가할 수 있습니다.
