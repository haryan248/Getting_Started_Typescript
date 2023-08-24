# 1. 타입 가드란?

타입 가드란 여러 개의 타입으로 지정된 값을 특정 위치에서 원하는 타입으로 구분하는 것을 의미합니다.
타입스크립트 관점에서는 좁은 타입으로 범위를 좁히는 것을 의미합니다.

```typescript
function updateInput(textInput: number | string | boolean) {
  if (typeof textInput === "number") {
    console.log(textInput)
  }
}
```

textInput의 타입을 number로만 제한했기 때문에 textInput의 타입은 number로 타입추론 하게 됩니다.

# 2. 타입 가드는 왜 필요할까?

```typescript
function updateInput(textInput: number | string | boolean) {
  textInput.toFixed(2)
}
```

소수점 두 자리까지만 표기하고 싶은데 textInput은 3가지 타입을 받으므로 에러를 냅니다.

## 타입 단언으로 타입 에러 해결하기

```typescript
function updateInput(textInput: number | string | boolean) {
  ;(textInput as number).toFixed(2)
}
```

이렇게 number로 타입 단언을 해버리면 오류는 없어집니다. 그러나 좋지 않은 방법입니다.

- 런타입 에러는 막을 수 없다.
- 타입 단언을 쓸 때 계속 사용해야한다.

## 타입 가드로 해결하기

이때 사용해야 하는 것이 타입가드입니다.

```typescript
function updateInput(textInput: number | string | boolean) {
  if (typeof textInput === "number") {
    console.log(textInput.toFixed(2))
    return
  }
  if (typeof textInput === "string") {
    console.log(textInput.length)
    return
  }
}
```

textInput의 타입이 number일때만 실행하도록 막아주면 두가지 문제점을 모두 해결할 수 있습니다.
textInput의 타입에 따라 실행되는 코드가 서로 다르기 때문에 타입에러도 막고 런타임 에러도 막을 수 있습니다.

# 3. 타입 가드 문법

## typeof 연산자

typeof 연산자는 타입스크립트의 문법이 아닌 자바스크립트의 연산자입니다.

```typescript
typeof 10 // 'number'
typeof "hello" // 'string'
typeof function () {} // 'function'
```

아까 위의 예시처럼 typeof을 사용해 해당 타입의 경우에만 실행되도록 제한하여 타입을 가드할 수 있습니다.

## instanceof 연산자

instanceof 연산자도 마찬가지로 자바스크립트의 연산자입니다.
이 연산자는 변수가 대상 객체의 프로토타입 체인에 포함되는지 확인하여 true/false를 반환해줍니다.

```typescript
function Person(name, age) {
  this.name = name
  this.age = age
}

var captain = new Person("캡틴", 100)
captain instanceof Person // true

var hulk = { name: "헐크", age: 79 }
hulk instanceof Person // false
```

모든 객체는 기본적으로 Object를 프로토타입으로 상속받기 때문에 hulk 변수의 프로토타입은 Object가 되고, captain 변수는 생성자 함수로 생성된 객체이기 때문에 프로토타입이 Person이 됩니다.

```typescript
captain instanceof Person // true
Person instanceof Object // true

hulk instanceof Person // false
hulk instanceof Object // true
```

이제 타입가드에 활용되는 예시를 보겠습니다.

```typescript
class Person {
  name: string
  age: number

  constructor(name, age) {
    this.name = name
    this.age = age
  }
}

function fetchInfoByProfile(profile: Person | string) {
  if (profile instanceof Person) {
  }
}
```

if문 블록 안에서는 Person 타입으로만 추론되기 때문에 name과 age속성에 접근할 수 있습니다.

## in 연산자

in 도 자바스크립트의 연산자이며 객체에 속성이 있는지 확인해줍니다. 객체에 해당 속성이 있으면 true, 없으면 false를 반환해줍니다.

```typescript
interface Book {
  name: string
  rank: number
}

interface OnlineLecture {
  name: string
  url: string
}
function learnCourse(material: Book | OnlineLecture) {
  if ("url" in material) {
    // metrial 타입이 OnlineLecture인 타입으로 추론
  }
}
```

material 파라미터에 url 속성이 있는지 체크하는 로직을 작성했습니다. 이제 url속성은 OnlineLecture에만 있기 때문에 OnlineLecture로 추론됩니다.
두가지 공통으로 있는 name으로 가드하게되면 material의 타입이 특정타입으로 추론되지 않습니다.

# 4. 타입 가드 함수

타입 가드 함수란 타입 가드 역할을 하는 함수를 의미합니다. in 연산자와 역할은 같지만 좀 더 복잡한 경우에도 사용할 수 있습니다.

```typescript
function isPerson(someone: Person | Developer): someone is Person {
  // ...
}
```

이 코드는 Person 타입과 Developer 타입 중 Person 타입으로 구분하는 타입 가드 함수입니다.

## 타입 가드 함수 예시

```typescript
interface Person {
  name: string
  age: number
}
interface Developer {
  name: string
  skill: string
}

function greet(someone: Person | Developer) {
  if ("age" in someone) {
    // Person
  } else {
    // Developer
  }
}
```

in 연산자를 통해 someone을 타입가드하여 올바르게 추론되게 해놓은 코드가 있습니다. 이를 isPerson 이라는 타입가드 함수를 통해 사용해보겠습니다.

```typescript
function isPerson(someone: Person | Developer): someone is Person {
  return (someone as Person).age !== undefined
}
```

someone을 Person으로 타입 단언 한뒤 age속성이 있는지 확인하여 Person으로 is 연산자를 사용해 가드하는 함수입니다.

이를 적용한 코드입니다.

```typescript
function greet(someone: Person | Developer) {
  if (isPerson(someone)) {
    // Person
  } else {
    // Developer
  }
}
```

## 복잡한 경우의 타입 가드 예시

위의 예시는 굳이 함수로 빼야할까? 라는 느낌이 있습니다. 꼭 필요한 상황의 예시 입니다.

```typescript
interface Hero {
  name: string
  nickname: string
}

interface Person {
  name: string
  age: number
}

interface Developer {
  name: string
  age: string
  skill: string
}

function greet(someone: Hero | Person | Developer) {
  // ...
}
```

이 greet함수는 someone이 Person 타입일때 나이를 출력한다고 합시다.

```typescript
function greet(someone: Hero | Person | Developer) {
  // ...
  if ("age" in someone) {
    console.log(someone.age)
  }
}
```

age 속성이 Person과 Developer에서 가지고 있는 속성이므로 age는 string | number로 추론됩니다.
결국 in연산자 만으로는 someone의 타입을 Person으로 단정지을 수 없게됩니다. 이때 타입 가드 함수를 통해 추론을 하게 할 수 있습니다.

```typescript
function isPerson(someone: Hero | Person | Developer): someone is Person {
  return typeof (someone as Person).age === "number"
}
```

someone의 타입을 Person으로 단언하고 age 속성이 number인지 아닌지 구분하고 결과를 반환합니다.
그러면 number면 타입을 Person으로 구분해줍니다.

```typescript
function greet(someone: Hero | Person | Developer) {
  // ...
  if (isPerson(someone)) {
    console.log(someone.age)
  }
}
```

타입 가드 함수를 사용하면 이제 someone의 타입은 Person 하나로 추론하고, age의 타입도 number로 추론합니다.

# 5. 구별된 유니언 타입

구별된 유니언 타입(discriminated unions)이란 유니언 타입을 구성하는 여러 개의 타입을 특정 속성의 유무가 아니라 특정 속성 값으로 구분하는 타입 가드 문법을 의미합니다.

```typescript
interface Person {
  name: string
  age: number
}

interface Developer {
  name: string
  skill: string
}

function greet(someone: Person | Developer) {
  if ("age" in someone) {
    // 이 if 문 안에서는 Person 타입
  }
}
```

```typescript
interface Person {
  name: string
  age: number
  industry: "common"
}

interface Developer {
  name: string
  age: string
  industry: "tech"
}
```

이렇게 interface를 수정하면 두타입의 속성이 모두 같기 때문에 in 연산자를 통해 타입가드를 사용할 수 없게 됩니다.

타입가드를 변경한 코드입니다.

```typescript
function greet(someone: Person | Developer) {
  if (someone.industry === "common") {
    // 이 if 문 안에서는 Person 타입
  }
}
```

industry 값이 common 이면 Person 타입에 해당하므로 somone을 Person 타입으로 추론하게 됩니다.
이처럼 속성의 문자열 타입 값을 비교해서 타입을 구분해 내는 것이 **_구별된 유니언 타입_**입니다.

# 6. switch 문과 연산자

## switch 문

switch문으로 타입 가드를 적용한 코드 입니다.

```typescript
interface Person {
  name: string
  age: number
  industry: "common"
}

interface Developer {
  name: string
  age: string
  industry: "tech"
}

function greet(someone: Person | Developer) {
  switch (someone.industry) {
    case "common":
      console.log(someone.age.toFixed(2))
      break
    case "tech":
      console.log(someone.age.split(""))
      break
  }
}
```

if문 대신 switch 문으로 바꾼 코드입니다. 마찬가지로 각 case에 someone 타입이 올바르게 추론됩니다.

## 논리 비교 연산자

```typescript
function sayHi(message: string | null) {
  if (message.length >= 3) {
    console.log(message)
  }
}
```

위와 같이 message.length 를 바로 사용하면 message가 null일 수도 있기 때문에 null일때는 return 하도록 타입가드 해줘야합니다.

```typescript
function sayHi(message: string | null) {
  if (message === null) {
    return
  }
  if (message.length >= 3) {
    console.log(message)
  }
}
```

또는 null assertion으로 사용할 수 도 있습니다.

```typescript
function sayHi(message: string | null) {
  if (message!.length >= 3) {
    console.log(message)
  }
}
```

아니면 논리 연산자를 통해 타입가드를 할 수 있습니다.

```typescript
function sayHi(message: string | null) {
  if (message && message.length >= 3) {
    console.log(message)
  }
}
```

message 가 있으면 message.length를 확인하기 때문에 message가 null일때는 if문안으로 들어가지 않게되어 타입이 올바르게 추론됩니다.
