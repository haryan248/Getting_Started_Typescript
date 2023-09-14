# 1. 타입 호환이란?

타입 호환(type compatibility)란 서로 다른 타입이 2개 있을 떄 특정 타입이 다른 타입에 포함되는지를 의미합니다.

```typescript
let a: string = "hi";
let b: number = 10;

b = a;
```

b = a 라는 코드를 작성하면 할당할때 타입 에러가 발생할 것 입니다.
문자열 타입은 숫자 타입에 할당할 수 없으므로 타입 에러가 납니다. 자바스크립트라면 b = a를 실행하면 타입 캐스팅(type casting)이 발생하며
b는 문자 타입으로 변환 되었을 것입니다.
따라서 b와 a는 서로 타입이 호환되지 않습니다.

```typescript
let a: string = "hi";
let b: "hi" = "hi";

a = b;
b = a; // 에러
```

이럴때는 'hi' 가 string에 속하기 때문에 a = b는 가능하지만, b = a는 타입 에러가 발생합니다.
이처럼 타입 간 할당 가능 여부로 '타입이 호환된다.', '호환되지 않는다' 라고 표현할 수 있습니다.

# 2. 다른 언어와 차이점

```typescript
interface Ironman {
  name: string;
}

class Avengers {
  name: string;
}

let i: Ironman;
i = new Avengers();
```

이렇게 했을때 타입스크립트는 에러를 발생시키지 않는데 그 이유는 타입스크립트의 '구조적 타이핑' 때문입니다.

## 2.1 구조적 타이핑

구조적 타이핑(structual typing)이란 다음과 같이 타입 유형보다는 타입 구조로 호환 여부를 판별하는 언어적 특성을 의미합니다.

```typescript
type Captain = {
  name: string;
};

interface Antman {
  name: string;
}
let a: Captain = {
  name: "캡틴",
};

let b: Antman = {
  name: "앤트맨",
};

b = a;
```

서로 다른 목적을 가진 타입이지만 두 개의 타입은 호환됩니다.
타입스크립트는 해당 타입이 어떤 타입 구조를 갖고 있는지로 타입 호환 여부를 판별합니다. 두 타입 모두 name 속성을 갖고 있기 때문에
타입 구조가 같다고 할 수 있습니다. 속성 유무뿐 아니라 속성 이름까지 모두 일치해야 호환됩니다.

# 3. 객체 타입의 호환

두 타입 간 동일한 타입을 가진 속성이 1개라도 존재하면 호환 가능합니다.

```typescript
type Person = {
  name: string;
};

interface Developer {
  name: string;
  skill: string;
}

var joo: Person = {
  name: "형주",
};

var capt: Developer = {
  name: "캡틴",
  skill: "방패 던지기",
};

joo = capt;
```

서로 타입이 완전히 일치하진 않지만 Person 타입 에 필요한 name 속성이 정의되어 있기 때문에 호환이 되는 것으로 판단합니다.
그러나 반대로 capt = joo 를 하면 타입에러가 발생합니다. (skill 속성이 없기 때문)

```typescript
type Person = {
  name: string;
};

interface Developer {
  name: string;
  skill?: string;
}

var joo: Person = {
  name: "형주",
};

var capt: Developer = {
  name: "캡틴",
  skill: "방패 던지기",
};

joo = capt;
capt = joo;
```

옵셔널 속성으로 skill을 바꿔주면 문제없이 호환됩니다.

# 4. 함수 타입의 호환

```typescript
let getNumber = function (num: number) {
  return num;
};

let sum = function (x: number, y: number) {
  return x + y;
};

getNumber = sum;
```

구조가 다른 두 함수는 서로 호환되지 않습니다.
당연하게도 파라미터가 서로 다르고 함수의 로직도 달라지는 함수기 때문에 타입스크립트에서는 에러를 내뱉습니다.
반대로 파라미터가 더 많은쪽(sum = getNumber)으로 할당하면 타입이 호환됩니다.

getNumber(10, 20) 을 실행하여도 20은 무시되고 10만 출력되어 함수의 동작은 보장합니다.
따라서 특정 함수 타입의 부분 집합에 해당하는 함수는 호환되지만, 더 크거나 타입을 만족하지 못하는 함수는 호환되지 않습니다.

# 5. 이넘 타입의 호환

# 5.1 숫자형 이넘과 호환되는 number 타입

```typescript
enum Language {
  C, // 0
  Java, // 1
  TypeScript, // 2
}

let a: number = 19;
a = Language.C;
```

숫자형 이넘은 숫자 타입과 호환됩니다.

# 5.2 이넘 타입 간 호환 여부

```typescript
enum Language {
  C, // 0
  Java, // 1
  TypeScript, // 2
}

enum Programming {
  C, // 0
  Java, // 1
  TypeScript, // 2
}

let langC = Language.C;
langC = Programming.C;
```

langC에 이넘값을 먼저 적용후 나중에 할당하면 타입에러가 발생합니다.
이넘 타입은 같은 속성과 값을 가져도 객체와 다르게 타입 간에 호환되지 않습니다.

# 5.3 제네릭 타입의 호환

```typescript
interface Empty<T> {}

var empty1: Empty<string>;
var empty2: Empty<number>;

empty1 = empty2;
empty2 = empty1;
```

empty1, empty2에 각각 string, number를 넘겼지만 둘은 서로 호환됩니다.

**_제네릭으로 받은 타입이 해당 타입 구조에서 사용되지 않는다면 타입 호환에 영향을 미치지 않는다._**

```typescript
interface NotEmpty<T> {
  data: T;
}

var notEmpty1: NotEmpty<string>;
var notEmpty2: EmNotEmptypty<number>;

notEmpty1 = notEmpty2;
notEmpty2 = notEmpty1;
```

두 타입은 서로 호환되지 않습니다. 제네릭으로 받은 타입이 해당 타입 구조내에서 사용되었기 때문에 서로 구조가 달라져
타입이 호환되지 않습니다.
