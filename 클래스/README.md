# 1. 클래스란?

<hr >

```typescript
var capt = {
  name: "캡틴",
  skill: "방패 던지기",
};

var ha = {
  name: "현준",
  skill: "좋은 코드 짜기",
};
```

두 객체는 모두 name과 skill이라는 공통된 속성을 가지고 있습니다. 이렇게 모양이 유사한 객체는 일일이 정의하기 보단 생성자 함수를 사용하는 것이 유리합니다.

```typescript
function Person(name, skill) {
  this.name = name;
  this.skill = skill;
}

var capt = new Person("캡틴", "방패 던지기");
var lee = new Person("현준", "좋은 코드 짜기");
```

이처럼 객체를 여러개 생성한다면 생성자를 활용해 코드를 짤경우 쉬워집니다. 이를 클래스 문법으로 변환할 수 있습니다.

```typescript
class Person {
  constructor(name, skill) {
    this.name = name;
    this.skill = skill;
  }
}

var capt = new Person("캡틴", "방패 던지기");
var lee = new Person("현준", "좋은 코드 짜기");
```

위와 동일혼 동작을 하지만 문법만 변경된 코드입니다.

## 클래스 기본 문법

```typescript
function Person(name, skill) {
  this.name = name;
  this.skill = skill;
}

Person.prototype.sayHi = function () {
  console.log("hi");
};
```

Person() 생성 함수에 sayHi라는 속성 함수를 추가합니다.
person 생성자로 만든 객체의 프로토타입에는 sayHi라는 속성 함수가 있습니다. 그러므로 sayHi함수에 접근할 수 있습니다.
이를 클래스 문법으로 변경하면 다음과 같습니다.

```typescript
class Person {
  constructor(name, skill) {
    this.name = name;
    this.skill = skill;
  }
  sayHi() {
    console.log("hi");
  }
}
```

이 Person 클래스 코드는 name과 skill 값을 받아 객체를 생성할 수 있게 **생성자 메서드(constructor)**를 선언하고,
sayHi()라는 **클래스 메서드(class method)**를 선언한 코드입니다.
name과 skill은 **클래스 필드** 또는 **클래스 속성**이라고 하며 클래스로 생성된 객체를 **클래스 인스턴스(class instance)**라고 합니다.

## 타입스크립트의 클래스

```typescript
class Chatgpt {
  constructor(name: string) {
    this.name = name;
  }
  sum(a: number, b: number): number {
    return a + b;
  }
}
```

이렇게 작성을 하면 타입스크립트에서 에러를 발생시킵니다.
바로 "Property 'name' does not exist on type 'Chatgpt'. ts(2339)" 타입에러가 발생합니다.
타입스크립트로 클래스를 작성할 떄는 생성자 메서드에서 사용될 클래스 속성들을 미리 정의해주어야 합니다.

```typescript
class Chatgpt {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  sum(a: number, b: number): number {
    return a + b;
  }
}
```

이렇게 name에 타입을 추가해주면 오류가 없어집니다.

## 클래스 접근 제어자

클래스 접근 제어자를 사용하면 클래스 속성의 노출범위를 정의하고 복잡한 기능을 구현할때 유용합니다.

```typescript
class Person {
  name: string;
  skill: string;

  constructor(name: string, skill: string) {
    this.name = name;
    this.skill = skill;
  }
}

let ha = new Person("현준", "코드 짜기");
ha.name = "현중";
console.log(ha.name); // 현중
```

이렇게 직접적으로 name속성을 변경할 수 있습니다.

```typescript
class WaterPurifier {
  waterAmount: number;

  constructor(waterAmount: number) {
    this.waterAmount = waterAmount;
  }

  wash() {
    if (this.waterAmount > 0) {
      console.log("정수기 동작 성공");
    }
  }
}

let purifier = new WaterPurifier(30);
purifier.wash();
purifier.waterAmount = 0;
purifier.wash(); // 동작 안함
```

물의 양을 30 에서 0으로 바꿨기 때문에 마지막에 동작을 하지 않습니다. 클래스의 속성을 의도치 않게 조작했을 때 발생하는 에러를 방지해야 합니다.
이를 해결하기 위해서는 waterAmount에 private 접근제어자를 추가해주면 해결됩니다.
그러나 접근제어자를 설정하여도 실행 시점(런타입)의 에러는 보장해주지 못하기 때문에 코드는 실행됩니다.

보다 정확하게 해결하기 위해서는 private 문법(#)을 적용하면 됩니다.

```typescript
class WaterPurifier {
  #waterAmount: number = 0;

  constructor(waterAmount: number) {
    this.#waterAmount = waterAmount;
  }

  wash() {
    if (this.#waterAmount > 0) {
      console.log("정수기 동작 성공");
    }
  }
}

let purifier = new WaterPurifier(30);
purifier.wash();
purifier.#waterAmount = 0;
purifier.wash(); // 동작 안함
```

[예제 url](https://www.typescriptlang.org/play?ts=5.0.4&ssl=17&ssc=23&pln=17&pc=24#code/MYGwhgzhAEDqYBcCmAnACgVxQSwGbdWgG8BYAKGmgGIB3RVAQQFsB7DAOwQC5p2MmARoQC80AAwBucuUrAW7CAhQZgCFigAUdZCmZtOPPoNQBKYjMrQEAC2wQAdLXq7WHBNFHbGrzlIrQAX2l-OghrDTNSf0o8aA0bO0cvF313AD5xSItLaDkFFhAkexAWAHMNACJAVAnADCHABjroQE3mwETx6EBGQcBXmoqTPxyg-37+8kL3AAcsPAIUD14kGjhnTBx8VA0AZjEe8nHlqftQ8K2yHcnUJOc9NxnJbYmVlH3IQ4loAHpXptbACVHAC1WgA)

이를 사용하려면 tsconfig의 target을 ES2015이상으로 해야합니다.
