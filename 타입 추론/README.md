# 1. 타입 추론이란?

<hr >
타입 추론이란 타입스크립트가 코드를 해석하여 적절한 타입을 정의하는 동작을 의미합니다.

```typescript
let width = 100
```

width 변수는 number타입으로 추론됩니다.
이렇게 변수를 초기화하거나 함수의 파라미터에 기본값을 설정하거나 반환값을 설정했을 때 지정된 값을 기반으로 적당한 타입을 제시하고 정의해주는 것을 **_타입 추론_**이라고 합니다.

# 2. 함수의 타입 추론

### 반환 타입

```typescript
function sum(a: number, b: number): number {
  return a + b
}

let result = sum(2, 3)
```

반환 타입을 number로 지정했으니 result는 당연히 number로 추론합니다.
여기서 반환타입을 제거해도 number 로 추론됩니다.

```typescript
function sum(a: number, b: number) {
  return a + b
}

let result = sum(2, 3)
```

a와 b모두 number이고 더한 값도 당연히 number이기 때문에 number로 추론됩니다.

### 파라미터 타입

```typescript
function getA(a: number) {
  return a
}
```

a를 받아서 a를 리턴하는 함수인데 a를 number로 지정해줬기 때문에 반환타입 또한 number 타입을 추론합니다.

```typescript
function getA(a: number) {
  let c = "hi"
  return a + c
}
```

이때는 자바스크립트 해석기 내부적으로 숫자를 문자열로 변환하여 문자열로 합치기 때문에 string으로 추론합니다.

# 3. 인터페이스와 제네릭의 추론 방식

```typescript
interface Dropdown<T> {
  title: string
  value: T
}

let shoppingItem: Dropdown<number> = {
  // title: '길벗 책'
  // value: 100
}
```

shoppingItem의 타입을 제네릭으로 number를 넘겨줬기 때문에 value를 number 타입으로 추론하게 됩니다.

# 4. 복잡한 구조에서 타입 추론 방식

```typescript
interface Dropdown<T> {
  title: string
  value: T
}

interface DetailedDropdown<K> extends Dropdown<K> {
  tag: string
  description: string
}

let shoppingDetailItem: DetailedDropdown<number> = {}
```

Dropdown 인터페이스를 상속하고 제네릭으로 넘겨주는 타입인데 이떄 number를 넘겨줬으므로 Dropdown T에도 number가 들어가서 그에 맞게 타입을 추론합니다.

```typescript
interface DetailedDropdown {
  title: string
  value: number
  tag: string
  description: string
}
```

이와 같이 value에 number 타입으로 들어가 올바르게 추론할 수 있습니다.
