# Basic Types

TypeScript는 JavaScript와 거의 동일한 데이터 타입을 지원하며, 열거 타입을 사용하여 더 편리하게 사용할 수 있습니다.

### 불리언(Boolean)

TypeScript에서 boolean 값이라고 일컫는 참/거짓(true/false) 값입니다.

```typescript
let isDone: boolean = false;
```

### 숫자 (Number)

TypeScript의 모든 숫자는 부동 소수 값입니다. 부동 소수에는 number라는 타입이 붙혀집니다.

```typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

### 문자열 (String)

TypeScript에서는 텍스트 테이터 타입을 string으로 표현합니다.

```typescript
let color: string = "blue";
color = "red";
```

### 배열 (Array)

TypeScript는 JavaScript처럼 값들을 배열로 다룰 수 있게 해줍니다. 배열 타입은 두 가지 방법으로 쓸 수 있습니다.

1. 배열 요소들을 나타내는 타입 뒤에 []를 쓰는 것입니다

```typescript
let list: number[] = [1, 2, 3];
```

2. 제네릭 베열 타입을 쓰는 것입니다.

```typescript
let list: Array<number> = [1, 2, 3];
```

### 튜플 (Tuple)

튜플 타입을 사용하면, 요소의 타입과 개수가 고정된 배열을 표현할 수 있습니다. 단 요소들의 타입이 모두 같을 필요는 없습니다.

```typescript
// 튜플 타입으로 선언
let x: [string, number];
// 초기화
x = ["hello", 10]; // 성공
// 잘못된 초기화
x = [10, "hello"]; // 오류
```

### 열거 (Enum)

JavaScript의 표준 자료형 집합과 사용하면 도움이 될만한 데이터 형은 enum입니다.

```typescript
enum Color {
  Red,
  Green,
  Blue,
}
let c: Color = Color.Green;
```

기본적으로, enum은 0부터 시작하여 멤버들의 번호를 매깁니다. 멤버 중 하나의 값을 수동으로 설정하여 번호를 바꿀 수 있습니다.

```typescript
enum Color {
  Red = 1,
  Green,
  Blue,
} // 1번 부터 번호를 시작할 수 있다. Green = 2 | Blue = 3
let c: Color = Color.Green;
```

또는, 모든 값을 수동으로 설정할 수 있습니다

```typescript
enum Color {
  Red = 1,
  Green = 2,
  Blue = 4,
}
let c: Color = Color.Green;
```

### Any

애플리케이션을 만들 때, 알지 못하는 타입을 표현해야 할 수도 있습니다. 이 값들은 동적인 콘텐츠에서 올 수도 있습니다. 이 경우 타입 검사를 하지 않고, 그 값들이 컴파일 시간에 검사를 통과하길 원합니다. 이를 위해, any 타입을 사용할 수 있습니다.

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // 성공, 분명히 부울입니다.
```

any 타입은 기존에 JavaScript로 작업할 수 있는 강력한 방법으로, 컴파일 중에 점진적으로 타입 검사를 하거나 하지 않을 수 있습니다(자주 사용하면 JavaScript와 다를 바 없음)

또한, any 타입은 타입의 일부만 알고 전체는 알지 못할 때 유용합니다. 예를 들어, 여러 다른 타입이 섞인 배열을 다룰 수 있습니다.

```typescript
let list: any[] = [1, true, "free"];

list[1] = 100;
```

### Void

void는 어떤 타입도 존재할 수 없음을 나타내기 때문에, any의 반대 타입 같습니다. void는 보통 함수에서 반환 값이 없을 때 반환 타입을 표현하기 위해 쓰이는 것을 볼 수 있습니다.

```typescript
function warnUser(): void {
  console.log("This is my warning message");
} // 아무 타입도 반환하지 않음을 의미
```

void를 타입 변수를 선언하는 것은 유용하지 않은데, 왜냐하면 (--strictNullChecks을 사용하지 않을 때만) 그 변수에는 null 또는 undefined 만 할당할 수 있기 때문입니다.

```typescript
let unusable: void = undefined;
unusable = null; // 성공  `--strictNullChecks` 을 사용하지 않을때만
```

### Null and Undefined

TypeScript는 undefined 과 null 둘 다 각각 자신의 타입 이름으로 undefined , null로 사용합니다.

```typescript
// 이 밖에 이 변수들에 할당할 수 있는 값이 없습니다!
let u: undefined = undefined;
let n: null = null;
```

### Never

never 타입은 절대 발생할 수 없는 타입을 나타냅니다. 예를 들어, never는 함수에서 항상 오류를 발생시키거나 절대 반환하지 않는 반환 타입으로 쓰입니다(never를 반환하는 함수는 함수의 마지막에 도달할 수 없다)

```typescript
// never를 반환하는 함수는 함수의 마지막에 도달할 수 없다.
function error(message: string): never {
  throw new Error(message);
}

// 반환 타입이 never로 추론된다.
function fail() {
  return error("Something failed");
}

// never를 반환하는 함수는 함수의 마지막에 도달할 수 없다.
function infiniteLoop(): never {
  while (true) {}
}
```

### 객체 (Object)

object는 원시 타입이 아닌 타입을 나타냅니다. 예를 들어, number, string, boolean, bigint, symbol, null, 또는 undefined 가 아닌 나머지를 의미합니다.

```typescript
declare function create(o: object | null): void;

create({ prop: 0 }); // 성공
create(null); // 성공

create(42); // 오류
create("string"); // 오류
create(false); // 오류
create(undefined); // 오류
```

### 타입 단언 (Type assertions)

가끔, TypeScript보다 개발자가 값에 대해 더 잘 알고 일을 때가 있습니다. 대개, 이런 경우는 어떤 엔티티의 실제 타입이 현재 타입보다 더 구체적일 때 발생합니다. 타입 단언(Type assertions) 은 컴파일러에게 "날 믿어, 난 내가 뭘 하고 있는지 알아"라고 말해주는 방법입니다(진짜 확실할 때만 사용한다) 타입 단언에는 두 가지 형태가 있습니다.

1. "angle-bracket" 문법입니다

```typescript
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

2. as-문법 입니다.

```typescript
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```
