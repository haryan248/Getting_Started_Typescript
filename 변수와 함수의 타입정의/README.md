# 1. 기본 타입

<hr>
주요 데이터 타입 9가지

- string
- number
- boolean
- object
- Array
- tuple
- any
- null
- undefined

```typescript
var name: string = "hyunjoon";
var age: number = 10;
var isLogin: boolean = false;
var hero: object = { name: "captain", age: 100 };
// object의 경우 보다 상세히 타입을 선언해야 합니다.

var compaines: Array<string> = ["네이버", "삼성", "인프런"];
var compaines: string[] = ["네이버", "삼성", "인프런"];

// 튜플
var items: [string, number] = ["hi", 11];
```

### null 과 undefined

null은 의도적인 빈값, undefined는 기본적으로 할당되는 초깃값입니다.

# 2. 함수에 타입 정의하기

<hr>

매개변수의 타입과 함수의 반환값에 타입을 정의할 수 있습니다.

```typescript
function sayHyunJoon(word: string): string {
  return word + "hyun joon";
}
```

매개변수가 하나일때 올바르지 않은 매개변수를 미리 확인할 수 있습니다.

```typescript
sayHyunJoon("hi", "hah");
```

옵셔널 파라미터로 선택적으로 매개변수를 사용할 수도 있습니다.

```typescript
function callMyName(firstName: string, lastName?: string): string {
  return firstName + lastName;
}

callMyName("ha"); // ha
```
