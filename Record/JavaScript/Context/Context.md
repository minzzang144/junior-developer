# Execution Context(실행 컨텍스트)

[이전 글로 이동하기 -> Arrow Function](../This/ArrowFunction.md)

### Execution Context(실행 컨텍스트)의 정의와 종류

**실행 컨텍스트**는 코드의 실행환경에 대한 여러가지 정보를 담고있는 것으로 좀 더 알기쉽게 표현하자면 `실행 컨텍스트는 실행 가능한 코드가 실행되기 위해 필요한 환경`이다.<br>

먼저 자바스크립트의 코드는 총 3가지로 이루어지는데 글로벌 스코프에 존재하는 `글로벌 코드`, eval함수 내에서 실행되는 `eval 코드`, 그리고 함수 스코프 내에 존재하는 `함수 코드`가 있다.<br>
(일반적으로 eval 코드는 컴파일최적화를 방해하기 때문에 일반적으로 많이 사용하는 글로벌 코드와 함수 코드를 다룬다.)<br>

그리고 자바스크립트 엔진은 위 3가지 코드를 실행하기 위해 각각의 실행 컨텍스트를 생성하게 된다.<br>

- 글로벌 실행 컨텍스트(**G**lobal **E**xecution **C**ontext)

- 함수 실행 컨텍스트(**F**untion **E**xecution **C**ontext)

- eval 실행 컨텍스트(일반적으로 사용x)

기억해야 하는 것은 자바스크립트 엔진이 코드를 실행하기 전에 `글로벌 실행 컨텍스트`를 생성한다는 것과 함수를 호출할 때마다 `함수 실행 컨텍스트`가 생성이 된다는 것이다.<br>

즉, 글로벌 실행 컨텍스트는 실행 이전에 생성이 되고 함수 실행 컨텍스트는 호출될 때 생성된다.<br>

```
var x = 'context1';

function foo () {
  var y = 'context2';

  function bar () {
    var z = 'context2';
    console.log(x + y + z);
  }
  bar();
}
foo();
```

위 코드를 실행하면 다음과 같이 실행 컨텍스트가 생성되고 소멸된다.<br>

![JavaScript-02](../../../Image/javascript-02.png)

코드가 실행되기 전에 글로벌 실행 컨텍스트가 쌓이고 함수가 호출되면 함수의 실행 컨텍스트가 쌓인다.<br>

함수 실행이 끝나면 함수 실행 컨텍스트가 제거되며 마지막으로 코드 실행이 종료되면 글로벌 실행 컨텍스트도 제거된다.<br>

이는 **LIFO(후입선출)** 의 구조를 따른다.<br>

### 실행 컨텍스트의 구성요소

실행 컨텍스트는 모두 다음과 같은 구성요소를 가집니다.<br>

**Lexical Environment**<br>

**Variable Environment**<br><br>

그리고 GEC와 FEC의 구성요소는 조금씩 다른데 어떻게 다른지 하나씩 비교해보겠습니다.<br>

- Lexical Environment

  Lexical Environment는 총 3가지로 구성되어 있습니다.

  1. Environment Record(V/O)

     실행 컨텍스트가 생성되면 자바스크립트 엔진은 실행에 필요한 여러 정보들을 담을 객체를 생성한다.

     이를 **Environment Record** 또는 **Varibale Object(V/O)** 라고 한다.

     Environment Record는 다음과 같은 정보를 담고 있다.

     - 변수

     - parameter or argument 정보

     - 함수 선언(함수 표현식 X)

     GEC의 Environment Record와 FEC의 Environment Record는 각각 전역 객체 / 활성 객체를 가르키는데 이는 GEC는 전역 코드와 함수 코드의 내용이 다르기 때문이다.

     그림으로 비교해보면 다음과 같다.

     - GEC의 E/R(V/O)

       ![JavaScript-03](../../../Image/javascript-03.png)

     - FEC의 E/R(V/O)

       ![JavaScript-04](../../../Image/javascript-04.png)

       그림과 같이 GEC는 parameter와 argument를 받을 수 있는 수단이 없이 때문에 제외되었다.

  2. Outer(Scope Chain)

     Outer(Scope Chain)는 외부 Lexical Environment를 참조하는 포인터로 전역 객체 또는 활성 객체의 리스트를 가리킨다.

     ![JavaScript-05](../../../Image/javascript-05.png)

     [Scope](../Scope/Scope.md) 글에서 Scope란 변수를 저장하고 찾는 규칙의 집합이라고 설명했었다.

     실제로 자바스크립트 엔진은 변수를 찾을 때 현재 스코프에서 찾는 것을 시도하고 실패할 시, 스코프 체인에 담겨있는 리스트 순서대로 점차 외부 스코프를 검색하게 되는데 바로 이것이 스코프 체인이라 불리는 이유이다.

  3. This Object

     This Object에는 동적으로 바인딩 되는 `this`객체의 정보가 담기게 된다.
