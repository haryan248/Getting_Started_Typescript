# 1. 이넘이란 ?

이넘이란 특정 값의 집합을 의미하는 데이터 타입입니다.

```typescript
function getDinnerPrice() {
  return 10000 + 2000;
}
// 코드만으론 어떤 가격인지 알 수 없기 때문에 상수로 변경할 수 있습니다.

function getDinnerPrice() {
  const RICE = 10000;
  const COKE = 2000;
  return RICE + COKE;
}
```

이러한 여러 개의 상수를 하나의 단위로 묶으면 이넘이 됩니다.

# 2. 숫자형 이넘

```typescript
enum Direction {
  Up, // 0
  Down, // 1
  Left, // 2
  Right, // 3
}

console.log(Direction.Left); // 2
```

0부터 차례로 할당이 되는 이유는 타입스크립트의 내부 규칙 때문에 그렇습니다.
[타입스크립트 플레이그라운드](https://www.typescriptlang.org/play) 에서 이넘의 코드를 반환해보면 숫자로 할당되는 것을 확인 할 수 있습니다.

```typescript
Console.log(Direction.Up); // 0;
Console.log(Direction[0]); // "Up";
```

이넘의 속성과 값이 거꾸로 연결되어 할당되는 것을 리버스 매핑(reverse mapping)이라고 합니다.

# 3. 문자형 이넘

문자형 이넘이란 이넘의 속성 값에 문자열을 연결한 이넘을 의미합니다.

```typescript
enum Direction {
  Up = "Up",
  Down = "Down",
  Left = "Left",
  Right = "Right",
}
console.log(Direction.Left); // 'Left'
```

이와 같이 아까의 예제와는 다르게 숫자가 아닌 문자열이 출력됩니다.
보통 이넘은 숫자로 관리하기보다 문자열로 관리하고 속성 이름과 값을 동일한 문자열로 관리하는 것도 일반적입니다.

# 4. 알아 두면 좋은 이넘의 특징

## 혼합 이넘

```typescript
enum Answer {
  Yes = "Yes",
  No = 1,
}
```

이렇게 숫자와 문자열을 혼합해서 사용해도 문제는 없지만 둘중의 하나의 데이터 타입으로 관리하는 것이 좋습니다.

## 다양한 이넘 속성 값 정의 방식

이넘의 속성 값은 고정 값 뿐만 아니라 다양한 형태로 값을 할당할 수 있습니다.

```typescript
enum Answer {
  User,
  Admin,
  SuperAdmin = User + Admin,
  God = "abc".length,
}
```

User와 Admin의 속성 값은 0과 1이고 SuperAdmin은 둘을 더한 1입니다. God은 length를 사용해 속성을 지정해 3입니다.
잘 사용되진 않지만 이런식으로 속성의 값을 설정할 수 있습니다.

## const 이넘

const 이넘이란 이넘을 선언할 때 앞에 const를 붙인 이넘을 말합니다.

```typescript
const enum logLevel {
  Debug = "Debug",
  Info = "Info",
  Error = "Error",
}
```

const를 이넘 앞에 붙이는 이유는 컴파일 결과물의 코드 양을 줄이기 위해서입니다.
const를 사용하지 않고 이넘코드를 자바스크립트로 변환시키면 속성 이름과 값을 연결해주는 객체를 생성합니다.
[이넘 예제](https://www.typescriptlang.org/play?#code/KYOwrgtgBANg9gcwDLAG7BlA3gWAFBRQAiwARmAlALxQBEJ5CtANPoQJIgBmc1dnPFmygBRAE5i4YvrXGSxQvAF98qAIbS1ABy0p0mGvGRoMAOjlSgA)

그러나 const를 사용해 이넘을 작성할 경우 객체를 생성하지 않고 변수에 바로 문자열이 할당되게 됩니다.
[이넘 예제](https://www.typescriptlang.org/play?#code/MYewdgzgLgBApmArgWxgGxAcwDJwG5xowDeAsAFAwwAicARopjALwwBEtDmbANBVQEkwAMxAt2Q0b34wAogCd5IeeLYKl86eQC+FPAEMV+gA7HcBIqww58hAHTrlQA)

이때는 다양한 속성값으로 할당하면 안되고 항상 속성에 고정값을 넣어줘야합니다.
