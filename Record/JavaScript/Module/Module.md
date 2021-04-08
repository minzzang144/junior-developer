# Module

[이전 글 보러가기 -> Closure](../Closure/Closure.md)<br>

### 모듈

**모듈(Module)** 의 정의는 어플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각을 의미한다.<br>

일반적으로 모듈은 파일 단위로 분리되어 있으며 명시적으로 모듈을 로드하여 재사용하게 된다. 즉, 모듈은 어플리케이션에 분리되어 개별적으로 존재하다가 로드에 의해 비로소 어플리케이션의 일원이 되는 것이다.<br>

때문에 모듈은 기능별로 분리되어 작성되는 경우가 대부분이며 재사용성이 좋아서 개발 효율성과 유지보수성을 높힐 수 있으므로 꼭 필요하다.<br><br>

모듈은 자바스크립트에 한정된 것이 아닌 모든 언어에서 사용되는 개념이다. 하지만 자바스크립트에서의 모듈은 문제점이 존재한다.<br>

자바스크립트에서는 script태그를 사용해 외부의 script파일을 가져올 수 있다.<br>

문제는 sciript 태그를 사용해 외부의 스크립트 파일을 가져올 수 있지만, 자바스크립트가 로드 될 때 파일마다 독립적인 파일 스코프를 가지지 않고 하나의 전역 객체를 공유하게 되는 것이다.<br>

전부터 강조해왔지만 전역적으로 무언가를 존재하는 것은 굉장히 충돌이 발생하기 쉽고 때때론 예상치 못한 문제점을 발생할 수도 있다는 것이다.<br>

이러한 문제점 때문에 모듈화는 반드시 해결해야 하는 과제가 되었고 자바스크립트에서 모듈화는 크게 2가지로 나뉘게 되었다.<br>

- CommonJS

  서버 사이드 렌더링인 NodeJS는 모듈 시스템의 표준인 CommonJS를 채택하였고 현재도 100%표준은 아니지만 기본적으로 CommonJS의 방식을 준수하고 있다.

  따라서 NodeJS 환경에서는 모듈 별로 독립적인 스코프를 갖는다.

- AMD

  Asynchronous Module Definition으로 비동기적 모듈 선언이란 뜻입니다.

  가장 유명한 스크립트로 RequireJS가 있다.

현재는 클라이언트 사이드 에서도 동작하는 모듈 기능이 추가되었으며 ES6 모듈을 사용할 수 있다.<br>

script태그에 `type="module"` 속성을 추가하면 로드된 자바스크립트는 모듈로서 동작하게 된다.<br>

하지만 몇 가지 이유로 아직까지는 브라우저가 지원하는 ES6 모듈 기능보다는 Webpack같은 모듈 번들러를 사용한다.<br>

- IE를 포함한 구형 브라우저는 ES6 모듈을 지원하지 않는다.

- 브라우저의 ES6 모듈 기능을 사용하더라도 아직 지원하지 않는 기능이 있어 트랜스파일링이나 번들링이 필요하다.

### 대표적인 모듈

클로저의 능력을 가장 많이 활용하는 강력한 패턴, 모듈을 살펴보자.<br>

아래 코드와 같은 패턴을 `모듈`이라고 한다.<br>

```
function CoolMoudle() {
  var something = "cool";
  var another = [1, 2, 3];
  function doSomething() {
    console.log(something);
  }
  function doAnother() {
    console.log(another.join("!"));
  }

  return {
    doSomething,
    doAnother
  }
}

var foo = CoolModule();
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

다른 패턴의 모듈도 있다.<br>

```
var foo = (function CoolModule(id) {
  function change() {
    // modifying the public API
    publicAPI.identify = identify2;
  }
  function identify() {
    console.log(id);
  }
  function identify2() {
    console.log(id.toUpperCase());
  }
  var publicAPI = {
    change,
    identify
  }

  return publicAPI;
})("foo module");

foo.identify(); // foo module
foo.change();
foo.identify(); // FOO MODULE
```

위 코드의 특징을 알아보자.<br>

- CoolModule은 그저 하나의 함수이지만 모듈 인스턴스를 생성하기 위해서는 반드시 호출해야 한다. 최외각 함수가 실행되지 않으면 내부 스코프와 클로저는 생성되지 않는다.

- CoolModule 함수는 객체를 반환한다. 해당 객체는 내장 함수들에 대한 참조(클로저)를 가지지만 내장 데이터 변수에 대한 참조는 가지지 않는다. 이 객체는 본질적으로 모듈의 공개 API라고 할 수 있다.

즉, 이 모듈 패턴을 사용하기 위해서는 두 가지 조건이 있다.<br>

1. 하나의 최외각 함수가 존재하고, 이 함수가 최소 한번은 호출되어야 한다.

2. 최외각 함수는 하나의 내부함수를 반환해야 한다. 그래야 해당 내부함수가 비공개 스코프에 대한 클로저를 가지고 비공개 상태(여기서는 숨겨져 있는 변수-> somthing, another등을 말함)에 접근하고 수정할 수 있다.

이와 같이 모듈은 세부사항을 캡슐화하고 공개가 필요한 API들만 외부에 노출시키는 최소 권한의 원칙을 지킨다.<br><br>

실제로 코드를 작성할 때는 위와 같이 모든 코드를 모듈화해서 작성하는 것이 매우 번거롭고 브라우저가 모듈을 100% 이해할 지 확신할 수 없으므로 `모듈 번들러`를 사용하게 된다.<br>

대표적으로 웹팩(Webpack) / 걸프(Gulp) / 파셀(Parcel) 등이 있고 그 외에도 많이 존재한다.<br>

모듈 번들러를 사용함으로써 모든 코드를 작성할 때 일일히 모듈화하지 않아도 번들러가 하나의 모듈로 바라보게 해주며 클로저를 사용해 독립적인 스코프를 가지고 있기 때문에 전역적으로 충돌이 발생하지 않을 수 있다.<br>

모듈 번들러를 통한 모듈들은 어떻게 서로 모듈임을 알고 사용할 수 있을까?<br>

아래에서 확인해보자.<br>

### 모듈의 공개 및 로드

모듈은 inline형식을 지원하지 않고 반드시 개별 파일(모듈당 파일 하나)에 정의되어야 한다.<br>

또한 모듈을 불러올 때는 동기적으로 모듈 파일을 불러온다.<br><br>

- export 키워드

  모듈은 독자적인 스코프를 가지기 때문에 모듈 안에 선언한 식별자는 기본적으로 모듈 내에서만 참조할 수 있다.

  **export** 키워드는 모듈 안에 선언한 식별자를 외부에 공개하는 역할을 하며 다른 모듈에서 사용하게 하고 싶다면 export 키워드를 사용한다.

  선언된 변수, 함수, 클래스 모두 모듈의 공개 API로 export 가능하다.

  ```
  // 변수의 공개
  export const pi = Math.PI;

  // 함수의 공개
  export function square(x) {
    return x * x;
  }

  // 클래스의 공개
  export class Person {
    constructor(name) {
      this.name = name;
    }
  }
  ```

  선언문 앞에 매번 export 키워드를 붙이는 것이 번거롭다면 하나의 객체로 구성하여 export 하는 것도 가능하다.

  ```
  // 변수, 함수 클래스를 하나의 객체로 구성하여 공개
  export { pi, square, Person };
  ```

  또한 모듈에서 하나만을 export 하는 경우 default 키워드를 사용할 수 있다.

  다만 default를 사용하는 경우 var, let, const등은 사용할 수 없다.

  ```
  // 가능
  export default function (x) {
    return x * x;
  }

  // => SyntaxError: Unexpected token 'const'
  export default const foo = () => {};
  ```

- import 키워드

  모듈에서 공개한 대상을 로드하기 위해서는 **import** 키워드를 사용한다.<br>

  ```
  // 모듈에서 공개한 API를 하나하나씩 로드하기 위해선 {}안에 하나씩 import한다.
  import { pi, square, Person } from './lib.mjs';

  // 하나하나씩 로드하는 것이 번거롭다면 as 키워드로 하나의 이름으로 한꺼번에 import하는 것도 가능하다.
  import * as lib from './lib.mjs';
  // 이후 객체 속성을 사용하듯이 사용할 수 있다.
  console.log(lib.pi, lib.square(10), lib.Person('tsuel'));

  // 이름을 변경하여 import 하는 것도 가능하다.
  import { pi as PI, square as sq, Person as P } from './lib.mjs';

  // default 키워드를 사용해 export한 모듈은 {}없이 임의의 이름으로 import한다.
  import square from './lib.mjs';
  ```
