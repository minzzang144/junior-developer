# This

[이전 글로 이동하기 -> Module](../Module/Module.md)

### This란 무엇인가?

자바스크립트는 렉시컬 스코프와 동적 스코프의 사이에서 렉시컬 스코프를 채택했다.<br>

렉시컬 스코프는 [Lexical Scope](../Scope/LexicalScope.md) 글에서 다뤘듯이 함수가 어디에서 호출되었는지 상관없이 함수가 선언된 위치에 따라 스코프가 결정되는 것을 말한다.<br>

반대로 동적 스코프는 렉시컬 스코프의 반대되는 개념으로 `함수가 어디서 호출되었는지에 따라 스코프가 결정되는 것`이라 할 수 있다.<br>

자바스크립트는 주로 렉시컬 스코프를 활용하지만 때때로 동적 스코프를 이용해야 하는 경우도 있다.<br>

동적 스코프의 대표적인 예로 **this** 바인딩 객체가 있으며 this는 작성 시점이 아닌 런타임 시점에 스코프가 정해진다.<br>

때문에 this는 함수 선언 위치에 상관없이 어떻게 함수를 호출했는지에 따라 this 바인딩이 결정된다.<br>

요약 -> This는 바인딩 객체로 어디서 선언되었는지에 상관없이 어떻게 호출되었는지에 따라 바인딩 되는 객체이다.<br>

### 호출부

`this`의 바인딩 개념을 이해하기 위해서는 함수가 호출된 부분(호출부)를 확인해봐야 하며 `this가 가리키는 것` 이 무엇인지 확인해야 한다.<br>

호출부는 함수가 호출된 부분을 찾기만 하면 금방 확인할 수 있을 것처럼 보이나 코딩 패턴에 따라 '진짜' 호출부가 어디인지 모호할 때가 많아 확인하기 쉽지 않다.<br>

this는 호출부를 확인하고 다음에 열거할 4가지 규칙 중에 어느 것에 해당하는지 찾게 된다.<br>

- 기본 바인딩

  평범한 함수 호출로 인해 일어나는 바인딩으로 나머지 규칙에 해당하지 않을 경우 적용되는 this의 기본 규칙이다.

  ```
  function foo() {
    console.log(this.a);
  }

  var a = 2;
  foo(); // 2
  ```

  위 코드를 this에 대해 아무것도 모르는 사람이 볼 때 결과값은 undefined가 출력될 것이라고 생각한다.

  결론적으로 결과값은 2가 출력되는 것이 맞다.

  아마도 독자는 foo() 함수의 this는 자기 자신을 가리키기 때문에 foo()함수를 호출하면 this.a(foo.a)는 존재하지 않기 때문에 undefined를 예상하겠지만 실제로는 foo()함수가 호출되었을 당시 전역 객체 window가 this에 바인딩 되었기 때문에 this.a(window.a)를 통해 결과값이 2가 출력이 된다.

  **단독 함수 실행** 은 이와 같이 this를 전역 객체 window에 바인딩한다.

  만약 'use strict(엄격 모드)'가 명시되어 있다면 전역 객체 window가 기본 바인딩 대상에서 제외되기 때문에 this는 undefined가 된다.

  따라서 아래의 결과값은 undefined이다.

  ```
  function foo() {
    'use strict';
    console.log(this.a);
  }

  var a = 2;
  foo(); // 2
  ```

- 암시적 바인딩

  두번째 규칙은 호출부에 컨텍스트 객체가 있는지 확인하는 것이다. 만약 존재한다면 this는 그 객체에 바인딩 된다.

  ```
  function foo() {
    console.log(this.a);
  }

  var obj = {
    a: 2,
    foo
  }

  obj.foo(); // 2
  ```

  기본 바인딩에서는 foo() 자체로 호출되었지만 위 코드에서는 obj.foo()와 같이 호출되었다.

  이와 같이 컨텍스트 객체가 함수를 호출할 시, 바로 이 컨텍스트 객체가 this에 `암시(말하지 않아도)적으로 바인딩` 된다.

  따라서 위 코드는 this는 obj가 되고 결과값은 2가 출력이 된다.

  참고) obj1.obj2.foo() 처럼 객체 프로퍼티 참조가 체이닝 된 형태라면 최상위/최하위 수준의 정보만 호출부와 연관된다(여기서는 obj1이 this에 암시적 바인딩)<br><br>

  `암시적 소실`

  암시적으로 바인딩된 함수에서 바인딩이 소실되는 경우가 있다.

  ```
  function foo() {
    console.log(this.a);
  }

  var obj = {
    a: 2,
    foo
  }

  var bar = obj.foo;
  var a = '전역입니다.';
  bar(); // 전역입니다.
  ```

  위의 결과값은 `2`가 출력될 것처럼 보였지만 `전역입니다.`가 출력 됩니다.

  foo함수는 obj객체를 this에 암시적으로 바인딩 했지만 새로운 변수 bar가 참조하고 호출하면서 암시적인 바인딩을 버리고 기본 바인딩을 따르게 된다. 그래서 결과값은 window.bar()에 의해 `전역입니다.`가 출력되는 것이다.

  위와 같이 호출부가 연속해서 바뀌는 경우가 있기 때문에 this 바인딩의 행방이 묘연해지는 경우가 많다.

  때문에 this 바인딩을 어느 값으로 고정하여 this가 현재 무엇을 바인딩하고 있는지 알 수 있다면 굉장히 편리할 것이다.

  그 방법은 다음 규칙이다.

- 명시적 바인딩

  암시적 바인딩에서는 함수를 객체에 넣고 객체 프로퍼티를 이용해 this를 간접적으로 바인딩했다.

  이렇게 암시적으로 바인딩하기 위해 함수를 객체 안에 넣어서 사용하는 것은 위에서 봤다시피 반드시 그 객체에 바인딩이 된다는 보장이 없을뿐더러 바인딩이 소실되는 경우도 종종 있다.

  따라서 어떤 객체를 this에 바인딩하겠다는 것을 명확히 밝힐 방법이 필요한데 그 예로 Function객체 Prototype 메서드로 가지고 있는 `call / apply / bind` 메서드이다.

  call과 apply 메서드는 this에 바인딩할 객체를 첫번째 파라미터로 받고, 함수 호출 시 첫번째 파라미터로 받은 값을 this로 바인딩할 수 있다.

  이처럼 this로 지정한 객체를 직접 바인딩하는 것을 `명시적 바인딩`이라고 한다.

  ```
  function foo() {
    console.log(this.a);
  }

  var obj = {
    a: 2
  }

  foo.call(obj); // 2
  ```

  문제는 이렇게 명시적으로 바인딩해도 this 바인딩이 도중에 소실되거나 임의로 다시 덮어씌여지는 것을 명시적 바인딩으로도 막을 수 없다는 것이다.

  때문에 명시적 바인딩을 한 단계 감싸 절대적으로 바인딩되게 하는 `하드 바인딩`이 존재한다.

  ```
  function foo() {
    console.log(this.a);
  }

  var obj = {
    a:2
  }

  var bar function() {
    foo.call(obj);
  }

  bar();
  setTimeout(bar, 100);

  bar.call(window); // 2
  ```

  위와 같이 함수 bar 내부에서 foo.call(obj)로 바인딩하면서 bar가 호출될 때마다 obj를 this에 강제 바인딩 하는 것이다.

  이런 바인딩을 강력하고 명시적이어서 `하드 바인딩`이라고 하며 하드 바인딩을 하게 되면 재바인딩도 무시하게 된다.

  재사용 가능한 헬퍼 함수를 사용하는 것도 동일한 패턴이다.

  ```
  function foo() {
    console.log(this.a, something);
    return this.a + something;
  }

  // 간단한 bind 헬퍼
  function apply(fn, obj) {
    return function() {
      return fn.apply(obj, arguments);
    }
  }

  var obj = {
    a: 2
  }

  var bar = apply(foo, obj);
  var b = bar(3); // 2 3
  console.log(b); // 5
  ```

  현재는 헬퍼 함수를 구현하지 않아도 하드 바인딩을 가능케 하는 `Function.prototype.bind` 메서드가 존재한다.

  즉, 위 코드와 아래 코드는 동일하다.

  ```
  function foo() {
    console.log(this.a, something);
    return this.a + something;
  }

  var obj = {
    a: 2
  }

  var bar = foo.bind(obj);
  var b = bar(3); // 2 3
  console.log(b); // 5
  ```

- new 바인딩

  함수 앞에 new를 붙여 생성자 호출을 하면 다음과 같은 일들이 저절로 일어난다.

  1. 새 객체가 툭 만들어진다.
  2. 새로 생성된 객체의 [[Prototype]]이 연결된다.
  3. 새로 생성된 객체는 해당 함수 호출 시 this로 바인딩된다.
  4. 이 함수가 자신의 또 다른 객체를 반환하지 않는 한 new와 함께 호출된 함수는 자동으로 새로 생성된 객체를 반환한다.

  이와 같이 new 생성자와 함께 함수 호출을 하면 자동적으로 this가 새 객체에 바인딩 되게 된다.

  아래 예제를 보자.

  ```
  function foo(a) {
    this.a = a;
  }

  var bar = new foo(2);
  console.log(bar.a);
  ```

  new 생성자를 붙여 foo()함수를 호출했고, 새로운 객체가 어딘가에 생성되었다.

  또한 새로 생성된 객체는 foo 호출과 동시에 this에 자동적으로 바인딩 되었다. 따라서 bar.a는 새로운 객체의 a값을 가리키며 결과값은 2가 된다.

  결국 new 생성자는 함수 호출 시 this를 새 객체에 바인딩하는 방법이며 이것을 `new 바인딩` 이라고 한다.

### 바인딩 순서

현재까지 함수를 호출할 때의 4가지 this 바인딩에 대해 알아보았다.<br>

만약 여러개의 바인딩 규칙이 중복되어 있다면 어떻게 될까? 이러한 경우에는 우선순위가 존재한다.<br>

1. new로 함수를 호출했는가? -> 맞으면 새로운 객체가 this다.

2. call / apply / bind로 함수를 호출했는가? 또한 하드 바인딩 형태로 호출되었는가? -> 맞으면 명시적으로 지정된 객체가 this이다.

3. 함수를 객체가 소유하는 형태로 호출했는가? -> 맞으면 그 컨텍스트 객체가 this이다.

4. 그 외의 경우에 this는 기본값(엄격 모드는 undefined / 비엄격 모드는 전역 객체)로 세팅된다.(기본 바인딩)

1번부터 4번까지 순번이 낮을수록 우선순위를 가지며 순서대로 따져본 후 그중 맞아떨어지는 최초의 규칙을 적용한다.<br>

중요한 것은 하드 바인딩조차 new 바인딩이 오버라이드 할 수 있다는 점이다.<br>

굳이 new 바인딩으로 오버라이드 하는 경우는 많이 없지만 함수 파라미터를 일부 또는 전부 미리 세팅해야하는 경우 유용하게 사용된다.<br>

```
function foo(p1, p2) {
  this.val = p1 + p2;
}

var bar = foo.bind(null, "p1"); // null 바인딩은 new 바인딩에 의해 오버라이드 되므로 의미가 없다는 것을 보여준다.
var baz = new bar("p2"); // p1이 미리 세팅되었기 때문에 p2값만 넘겨준다.
console.log(baz.val); // p1p2
```

### 바인딩 예외

- this 무시

  call, apply, bind 메서드의 첫번째 파라미터로 null 또는 undefined를 넘기면 바인딩이 무시되고 기본 바인딩 규칙이 적용된다.

  하지만 굳이 null값을 명시적으로 바인딩 하는 것은 추후에 많은 에러를 야기시킬 수 있다.(프로그래밍 하다보면 바인딩이 예상한 값과 다르게 될 수 있기 때문)

  바인딩을 굳이 할 필요가 없다면 텅빈 객체에라도 바인딩을 해두는 것이 좋다.

  ```
  function foo(a,b) {
    console.log(a, b);
  }

  var nullObj = Object.create(null);

  foo.apply(nullObj, [2, 3]);

  var bar = foo.bind(nullObj, 2);
  bar(3);
  ```

  이와 최소한 텅빈 객체에 바인딩하게 되면 어차피 빈 객체로 바인딩하게 되므로 전역 객체를 건드리는 부작용은 방지할 수 있다.

- soft binding

  앞에서 다뤄본 하드 바인딩은 함수의 유연성을 크게 떨어뜨리기 때문에 나중에 this를 암시적으로 바인딩하거나 명시적으로 바인딩하는 식으로 수동으로 오버라이드 하는 것은 불가능하다.

  `softBind` 메서드는 this를 체크하는 부분에서 전역 객체나 undefined일 경우 미리 준비한 바인딩 객체로 세팅을 하며, 그 외의 경우 this는 손대지 않는다.

  ```
  function foo() {
    console.log("name" + this.name);
  }

  var obj = {name: "obj"},
    obj2 = {name: "obj2"},
    obj3 = {name: "obj3"};

  var fooObj = foo.softBind(obj);

  fooObj(); // name: obj

  obj2.foo = foo.softBind(obj);
  obj2.foo(); // name: obj2

  fooObj.call(obj3); // name: obj3

  setTimeout(obj2.foo, 100); // name: obj <-- 소프트 바인딩이 적용됨
  ```

  소프트 바인딩이 탑재된 foo()함수는 this를 obj2나 obj3로 수동 바인딩을 할 수 있을 뿐만 아니라 undefined나 글로벌 객체로 바인딩되게 되면 기본 바인딩 규칙이 적용된다.

[다음 글 보러가기 -> Arrow Function](./ArrowFunction.md)
