# 1. 제네릭이란?

<hr >
제네릭은 타입을 미리 정의하지 않고 사용하는 시점에 원하는 타입을 정의해서 쓸 수 있는 문법입니다.

```typescript
function getText(text) {
  return text;
}
getText("hi");
getText(10);
```

getText는 받은값을 그대로 반환해주는 함수입니다. 이를 제네릭으로 변경하면 다음과 같습니다.

```typescript
function getText<T>(text: T): T {
  return text;
}
getText<string>("hi");
getText<number>(10);
```

getText함수를 실행할때 넘길 parameter의 타입도 같이 작성해주면 return 타입도 동일한 타입으로 추론해줍니다.

# 2. 왜 제네릭을 사용할까?

## 중복되는 타입 코드의 문제점

반복되는 타입 코드를 줄여줍니다.

```typescript
function getText(text: string): string {
  return text;
}
function getNumber(num: number): number {
  return num;
}
```

이처럼 같은 기능을 하는 함수에 타입을 여러개 정의하기 위해 함수도 여러개 만들어 작성하였습니다.
타입이 추가된다면 또 다른 함수를 작성해야 합니다.

## any를 쓰면 되지 않을까?

기능은 같지만 타입 때문에 계속해서 반복 생성되는 함수를 보면 any를 쓰면 되지 않겠느냐고 생각할 수 있습니다.

```typescript
function getText(text: any): any {
  return text;
}
```

any로 할 경우 타입스크립트 에러는 모두 없어지지만 타입스크립트의 장점이 모두 없어집니다.
코드의 자동완성, 에러의 사전 방지 혜택을 받지 못합니다. 그럼 코드를 실제 실행할때 에러가 발생하게 됩니다.

# 3. 인터페이스에 제네릭 사용하기

```typescript
interface ProductDropdown {
  value: string;
  selected: boolean;
}
interface StockDropdown {
  value: number;
  selected: boolean;
}
...
interface AddressDropdown {
  value: { city: string; zipCode: string };
  selected: boolean;
}
```

드랍두운에 들어가는 값이 계속해서 달라진다면 이런식으로 드랍다운이 추가될때마다 인터페이스를 추가해야 합니다.

```typescript
interface Dropdown<T> {
  value: T;
  selected: boolean;
}

let product: Dropdown<string>;
let stock: Dropdown<number>;
let address: Dropdown<{ city: string; zipCode: string }>;
```

인터페이스에 제네릭을 활용하여 드롭다운이 추가되어도 유연하게 타입을 적용할 수 있는 형태로 짤 수 있습니다.

# 4. 제네릭의 타입 제약

## extends를 사용한 타입 제약

제네릭의 장점은 타입을 미리 정의하지 않고 호출하는 시점에 타입을 정의해서 유연하게 확장할 수 있다는 점인데 이는 타입을 별도로 제약하지 않고 아무 타입이나 받아서 쓸 수 있다는 의미입니다.

```typescript
function embraceEverything<T>(thing: T): T {
  return thing;
}
embraceEverything<string>("hi");
embraceEverything<any>("hi");
```

어떠한 타입을 넘겨도 문제가 없는데 extends를 통해 제약을 걸 수 있습니다.

```typescript
function embraceEverything<T extends string>(thing: T): T {
  return thing;
}
```

이제는 타입이 string타입만 넘길 수 있게 제약을 걸 수 있습니다.

## 타입 제약의 특징

```typescript
function lengthOnly<T extends { length: number }>(value: T): T {
  return value.length;
}
```

length 속성을 갖는 타입으로 제약을 하게되면 문자열, 배열을 넘길 수 있습니다.
다른 타입을 제네릭의 타입으로 넘긴다면 에러를 발생시킵니다.

## keyof를 사용한 타입 제약

keyof은 특정 타입의 키 값을 추출해서 문자열 유니언 타입으로 변환시켜 줍니다.

```typescript
function printKeys<T extends keyof { name: string; skill: string }>(value: T) {
  console.log(value);
}
```

제네릭을 정의하는 부분을 문자열 타입 중에서도 name과 skill만 파라미터로 받도록 타입을 제약했습니다.

# 5. 제네릭을 처음 사용할 때 주의해야 할 사고방식

```typescript
function printTextLength<T>(text: T) {
  console.log(text.length);
}

printTextLength<string>("hello");
```

이 코드는 정상적으로 동작하지 않습니다.
타입스크립트 컴파일러 관점에서는 printTextLength() 함수에 어떤 타입이 들어올지 모르기 때문에 함부로 이 타입을 가정하지 않습니다.
따라서 text파라미터를 다룰 때 코드 자동 완성이나 미리 정의된 효과를 얻을 수 없습니다.

extends 키워드를 사용하여 제네릭으로 받을 수 있는 타입을 제한하면 타입에러가 발생하지 않습니다.

```typescript
function printTextLength<T extends { length: number }>(text: T) {
  console.log(text.length);
}
// 또는 배열 형태로 정의
function printTextLength<T>(text: T[]) {
  console.log(text.length);
}
printTextLength<string>("hello");
```
