# 1. 타입 단언이란?

<hr >
타입 단언(type assertion)은 타입스크립트의 타입 추론에 기대지 않고 개발자가 직접 타입을 명시하여 해당 타입으로 강제하는 것을 의미합니다.

```typescript
let myName = "현준"
```

변수에 초깃값을 설정했으므로 string으로 타입을 추론합니다. 근데 as 키워드를 통해 타입을 정해줄 수 있습니다.

```typescript
let myName = "현준" as string
```

다른 예시를 보자면,

```typescript
interface Person {
  name: string
  age: number
}

var ha = {}
ha.name = "현준"
ha.age = 22
```

이렇게 사용하면 에러가 발생합니다. 그이유는 ha를 빈객체로 선언했기 때문에 컴파일러는 어떤 값이 들어갈지 모르기 때문입니다.
이때 as 키워드를 통해 객체에 들어갈 속성을 미리 정의하는 것입니다.

```typescript
var ha = {} as Person
ha.name = "현준"
ha.age = 22
```

# 2. 타입 단언 문법

## 타입 단언의 대상

타입 단언은 숫자, 문자열, 객체 등 원시값 뿐만 아니라 변수나 함수의 호출 결과에도 사용이 가능합니다.

```typescript
function getId(id) {
  return id
}

let myId = getId("josh") as number
```

함수의 결과값을 number로 타입단언하여 사용할 수 있습니다.

## 타입 단언 중첩

타입 단언은 여러번 중첩해서 사용도 가능합니다.

```typescript
let num = 10 as any as number
```

이코드는 타입단언을 여러번 중첩해서 사용이 가능하다는 것을 보여주기 위한 예시이며 위는 타입추론이 되기 때문에 굳이 타입 단언을 할 필요가 없는 코드입니다.

## 타입 단언을 사용할 때 주의할 점

**_as 키워드는 구문 오른쪽에서만 사용한다_**
타입 단언은 변수 이름에 사용할 수 없습니다.

```typescript
let num as number = 10;
```

이럴떈 타입 표기로 타입을 정하는게 올바른 방법입니다
.
**_호환되지 않는 데이터 타입으로는 단언할 수 없다_**

타입 단언을 사용하면 원하는 타입으로 단언할 수 있을 것 같지만 실제로는 그렇게 하지 못합니다.

```typescript
let num = 10 as string
```

number인 10을 string으로 변환할 수 없기에 타입 에러가 발생합니다.

**_타입 단언 남용하지 않기_**

타입 단언은 코드를 실행하는 시점에서 아무런 역할도 하지 않기 때문에 에러에 취약한 측면이 있습니다.
타입 에러를 쉽게 해결하려고 타입을 단언해서 타입에러는 해결하지만 실행했을때는 에러를 방지 못할 경우가 생깁니다.

```typescript
interface Profile {
  name: string;
  id: string;
}
function getProfile() {
  ...
}
let myProfile = getProfile() as Profile;

renderId(myProfile.id);
```

getProfile이라는 함수가 타입을 정의하기 복잡해 as를 사용해 타입을 단언하고 myProfile을 넘기는 함수가 있다고 합시다.
그러면 renderId에 id를 넘길떄 단언을 했기때문에 타입에러는 나지 않습니다. 그러나 실행했을 때 오류가 발생할 수 있습니다.

예를 들어 getProfile에서 받아온 값이 undefined인 경우 id가 없기 때문에 런타임 에러가 발생합니다.
myProfile 자체가 undefined이니 undefined.id는 당연히 오류가 발생하기 떄문입니다.

# 3. null 아님 보장 연산자: !

null 아님 보장 연산자(non null assertion)은 null 타입을 체크할 때 유용하게 쓰는 연산자 입니다.

```typescript
function shuffleBooks(books) {
  let result = books.shuffle()
  return result
}
```

다음 함수를 실행하면 book을 랜덤하게 섞는 함수라고 가정합시다.
이때 books가 없다면 shuffle메소드가 실행되지 않기 때문에 오류를 발생시킬 것입니다.

이런 상황을 방지하기 위해 null 값 체크가 필요합니다.

```typescript
function shuffleBooks(books: Books || null) {
  if(books ==== null || books === undefined) return;
  let result = books.shuffle()
  return result
}
```

```typescript
function shuffleBooks(books: Books || null) {
  let result = books!.shuffle()
  return result
}
```

이때 books가 null이 아니라는 확신이 있으면 ! 연산자를 사용하여 타입에러를 없애줄 수 있습니다.
그러나 위코드에서는 null이 들어올 가능성이 있기 때문에 위의 코드처럼 if문으로 방어해주는게 올바른 방법입니다.

보통 자주 사용되는 것은 웹 API를 사용할떄 자주 사용됩니다.

```typescript
let divElement = document.querySelector("div") // HTMLDivElement | null
// if(divElement) divElement.addEventListener("click", clickHandler)
divElement!.addEventListener("click", clickHandler)
```

divElements는 null일 수 있기 때문에 if문으로 감싸야 타입에러를 없앨 수 있는데 div태그가 무조건 있는게 확신이 들면 !로 간편하게 쓸 수 있습니다.
그래도 가급적 타입 단언보다는 타입 추론에 의지하는 것이 좋습니다.
