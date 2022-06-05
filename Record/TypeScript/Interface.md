# Interface

### 예제

```typescript
interface LabeledValue {
  label: string;
}

function printLabel(labeledObj: LabeledValue) {
  console.log(labeledObj.label);
}

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);
```

인터페이스 타입 검사는 프로퍼티들의 순서를 요구하지 않습니다. 단지 인터페이스가 요구하는 프로퍼티들이 존재하는지와 프로퍼티들이 요구하는 타입을 가졌는지만을 확인합니다.

### 선택적 프로퍼티 (Optional Properties)

인터페이스의 모든 프로퍼티가 필요한 것은 아닙니다. 어떤 조건에서만 존재하거나 아예 없을 수도 있습니다.

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
  let newSquare = { color: "white", area: 100 };
  if (config.color) {
    newSquare.color = config.color;
  }
  if (config.width) {
    newSquare.area = config.width * config.width;
  }
  return newSquare;
}

let mySquare = createSquare({ color: "black" });
```

선택적 프로퍼티를 가지는 인터페이스는 다른 인터페이스와 비슷하게 작성되고, 선택적 프로퍼티는 선언에서 프로퍼티 이름 끝에 ?를 붙여 표시합니다.

### 읽기전용 프로퍼티 (Readonly properties)

일부 프로퍼티들은 객체가 처음 생성될 때만 수정 가능해야합니다. 프로퍼티 이름 앞에 readonly를 넣어서 이를 지정할 수 있습니다.

```typescript
interface Point {
  readonly x: number;
  readonly y: number;
}

let p1: Point = { x: 10, y: 20 };
p1.x = 5; // 오류!
```

### 초과 프로퍼티 검사 (Excess Property Checks)

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
  // ...
}

let mySquare = createSquare({ colour: "red", width: 100 });
```

createSquare의 매개변수가 color대신 colour로 전달된 것에 유의하세요. 이 경우 JavaScript에선 조용히 오류가 발생합니다.

width 프로퍼티는 적합하고, color 프로퍼티는 없고, 추가 colour 프로퍼티는 중요하지 않기 때문에, 이 프로그램이 올바르게 작성되었다고 생각할 수 있습니다.

하지만, TypeScript는 이 코드에 버그가 있을 수 있다고 생각합니다. 객체 리터럴은 다른 변수에 할당할 때나 인수로 전달할 때, 특별한 처리를 받고, 초과 프로퍼티 검사 (excess property checking)를 받습니다. 만약 객체 리터럴이 "대상 타입 (target type)"이 갖고 있지 않은 프로퍼티를 갖고 있으면, 에러가 발생합니다.

```typescript
// error: Object literal may only specify known properties, but 'colour' does not exist in type 'SquareConfig'. Did you mean to write 'color'?
let mySquare = createSquare({ colour: "red", width: 100 });
```

이 검사를 피하는 방법은 정말 간단합니다. 가장 간단한 방법은 타입 단언을 사용하는 것입니다.

```typescript
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
```

하지만 특별한 경우에, 추가 프로퍼티가 있음을 확신한다면, 문자열 인덱스 서명(string index signatuer)을 추가하는 것이 더 나은 방법입니다. 만약 SquareConfig color와 width 프로퍼티를 위와 같은 타입으로 갖고 있고, 또한 다른 프로퍼티를 가질 수 있다면, 다음과 같이 정의할 수 있습니다.

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
  [propName: string]: any;
}
```

### 함수 타입 (Function Types)

인터페이스는 JavaScript 객체가 가질 수 있는 넓은 범위의 형태를 기술할 수 있습니다. 프로퍼티로 객체를 기술하는 것 외에, 인터페이스는 함수 타입을 설명할 수 있습니다.

인터페이스로 함수 타입을 기술하기 위해, 인터페이스에 호출 서명 (call signature)를 전달합니다. 이는 매개변수 목록과 반환 타입만 주어진 함수 선언과 비슷합니다. 각 매개변수는 이름과 타입이 모두 필요합니다.

```typescript
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function (source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
};
// 이런 방식으로 사용될 수 있다.
```

### 인덱서블 타입 (Indexable Types)

인터페이스로 함수 타입을 설명하는 방법과 유사하게, a[10] 이나 ageMap["daniel"] 처럼 타입을 "인덱스로" 기술할 수 있습니다. 인덱서블 타입은 인덱싱 할때 해당 반환 유형과 함께 객체를 인덱싱하는 데 사용할 수 있는 타입을 기술하는 인덱스 시그니처 (index signature)를 가지고 있습니다.

```typescript
interface StringArray {
  [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];
```

인덱스 서명을 지원하는 타입에는 두 가지가 있습니다: 문자열과 숫자.

두 타입의 인덱서(indexer)를 모두 지원하는 것은 가능하지만, 숫자 인덱서에서 반환된 타입은 반드시 문자열 인덱서에서 반환된 타입의 하위 타입(subtype)이어야 합니다.

```typescript
interface NumberDictionary {
  [index: string]: number;
  length: number; // 성공, length는 숫자입니다
  name: string; // 오류, `name`의 타입은 인덱서의 하위타입이 아닙니다
}
```

아래처럼 변경하면 오류가 해결된다.

```typescript
interface NumberOrStringDictionary {
  [index: string]: number | string;
  length: number; // 성공, length는 숫자입니다
  name: string; // 성공, name은 문자열입니다
}
```

### 클래스 타입 (Class Types)

클래스가 특정 계약(contract)을 충족시키도록 명시적으로 강제하는 C#과 Java와 같은 언어에서 인터페이스를 사용하는 가장 일반적인 방법은 TypeScript에서도 가능합니다.

```typescript
interface ClockInterface {
  currentTime: Date;
}

class Clock implements ClockInterface {
  currentTime: Date = new Date();
  constructor(h: number, m: number) {}
}
```

아래 예제의 setTime 처럼 클래스에 구현된 메서드를 인터페이스 안에서도 기술할 수 있습니다.

```typescript
interface ClockInterface {
  currentTime: Date;
  setTime(d: Date): void;
}

class Clock implements ClockInterface {
  currentTime: Date = new Date();
  setTime(d: Date) {
    this.currentTime = d;
  }
  constructor(h: number, m: number) {}
}
```

### 인터페이스 확장하기 (Extending Interfaces)

클래스처럼, 인터페이스들도 확장(extend)이 가능합니다. 이는 한 인터페이스의 멤버를 다른 인터페이스에 복사하는 것을 가능하게 해주는데, 인터페이스를 재사용성 높은 컴포넌트로 쪼갤 때, 유연함을 제공해줍니다.

```typescript
interface Shape {
  color: string;
}

interface Square extends Shape {
  sideLength: number;
}

let square = {} as Square;
square.color = "blue";
square.sideLength = 10;
```

```typescript
interface Shape {
  color: string;
}

interface PenStroke {
  penWidth: number;
}

interface Square extends Shape, PenStroke {
  sideLength: number;
}

let square = {} as Square;
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```
