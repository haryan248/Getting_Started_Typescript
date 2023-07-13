# 1. 타입 별칭이란?

<hr>

타입 별칭이란 특정 타입이나 인터페이스 등을 참조할 수 있는 타입 변수를 의미합니다.

```typescript
type MyMessage = string | number;
function logText(text: MyMessage) {
  // ....
}

var message: MyMessage = "안녕하세요.";
logText(message);
```

매 변수마다 string| number 의 타입을 반복하기 보다 타입 별칭을 통해 중복되는 타입을 합쳐서 쓸 수 있습니다.

# 2. 타입 별칭 vs 인터페이스

```typescript
type User = {
  id: string;
  name: string;
};

interface User {
  id: string;
  name: string;
}
```

코드 에디터에 표기되는 정보가 서로 다릅니다.

- 타입별칭으로 작성하게 될 경우 타입에 올려봤을 때 타입의 정보가 모두 표기
- 인터페이스로 작성하게 될 경우 interface명만 표기

사용할 수 있는 타입의 차이

- 타입 별칭의 경우 유니언 타입, 인터섹션 타입에도 사용이 가능
- 또한 제너릭이나 유틸리티 타입에 사용이 가능

```typescript
type ID = string;
type Product = TShirt | Shoes;
type Teacher = Person & Adult:

type Gillbut<T> = {
  book: T
}

type MyBeer = Pick<Beer, 'brand'>
```

타입 별칭의 장점이 더 많은 것 같지만 타입 확장이라는 관점에서는 서로 다릅니다.

```typescript
interface Person {
  name: string;
  age: number;
}

interface Developer extends Person {
  skill: string;
}

var joon: Developer = {
  name: "현준",
  age: 21,
  skill: "웹개발",
};
```

extends 키워드를 사용해 상속을 해서 타입을 확장해서 사용할 수 있습니다.
물론 타입 별칭도 & 연산자를 통해 똑같은 기능을 하는 타입을 만들 수 있습니다.

```typescript
type Person = {
  name: string;
  age: number;
};

type Developer = {
  skill: string;
};
type Joon = Person & Developer;
var joon: Joon = {
  name: "현준",
  age: 21,
  skill: "웹개발",
};
```

- 선언 병합(declaration merging)
  인터페이스는 동일한 이름으로 인터페이스를 선언하면 인터페이스 내용을 합치는 특성

```typescript
interface Person {
  name: string;
  age: number;
}
interface Person {
  skill: string;
}
var joon: Person = {
  name: "현준",
  age: 21,
  skill: "웹개발",
};
```

즉, 동일한 이름으로 인터페이스를 여러 번 선언했을 때 인터페이스의 타입내용을 합치는 것을 선언 병합이라고 합니다.

# 3. 타입 별칭은 언제 쓰는 것이 좋을까?

2021년 이전의 타입스크립트 공식 문서에서는 '좋은 소프트웨어는 확장이 용이해야 한다.' 라는 관점에서 인터페이스의 사용을 권장했습니다.
그러나 현재는 일단 인터페이스를 주로 사용해보고 타입 별칭이 필요할 때 타입 별칭을 쓰라고 안내하는 것으로 변경되었습니다.

정리하자면 '타입 별칭으로만 타입 정의가 가능한 곳에서는 타입 별칭을 사용하고 벡엔드와의 인터페이스를 정의하는 곳에서는 인터페이스를 이용하자'

### 타입 별칭으로만 정의할 수 있는 타입들

타입 별칭으로만 정의할 수 있는 타입은 인터섹션, 유니언 타입입니다.

```typescript
type MyString = string;
type StringOrNumber = string | number;
type Admin = Person & Developer;
```

그리고 제네릭, 유틸리티 타입(utility type), 맵드 타입(mapped type)에서도 사용할 수 있습니다.

```typescript
type Dropdown<T> = {
  id: string;
  title: T;
};

// 유틸리티 타입
type Admin = { name: string; age: number; role: string };
type OnlyName = Pick<Admin, "name">;

// 맵드 타입
type Picker<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

인터페이스에서 할 수 없는 곳에서는 타입 별칭을 사용

### 벡엔드와의 인터페이스 정의

```typescript
// 1. 타입 별칭으로 API 함수의 응답형태 정의
type User = {
  id: stirng;
  name: string;
};

function fetchData(): User {
  return axios.get("http://localhost:3000/users/1");
}

// 2. 인터페이스로 API 함수의 응답 형태를 정의

interface User {
  id: string;
  name: string;
}

function fetchData(): User {
  return axios.get("http://localhost:3000/users/1");
}
```

타입 별칭이 주는 에디터에서 미리보는 효과를 감안하면 타입별칭을 사용하는 것도 나쁘지 않ㅅ급니다.
하지만 이 장점보다 인터페이스를 이용했을 때 이점이 더 큽니다.

기존에 존재하는 타입을 결합하여 표시하도록 요구사항이 변경되었다면, 타입의 확장의 측면에서 인터페이스로 정의하는 것이 수월합니다.

```typescript
interface Admin {
  role: string;
  department: string;
}

// 상속을 통한 인터페이스 확장
interface User extends Admin {
  id: string;
  name: string;
}

// 선언 병합을 통한 타입 확장
interface User {
  skill: string;
}

// 최종적으로 User의 타입
interface User {
  id: string;
  name: string;
  role: string;
  department: string;
  skill: string;
}
```

이런식으로 인터페이스로 할경우 유연하게 타입확장이 가능합니다
