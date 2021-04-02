# Closure

[이전 글 보러가기 -> Hoisting](../Hoisting/Hoisting.md)

### 스코프 클로저

클로저는 자바스크립트 모든 곳에 존재하며 렉시컬 스코프에 의존해 코드를 작성한 결과로 그냥 발생한다.<br>

이게 무슨 말인지 굉장히 난해하겠지만 그저 클로저는 우리도 모르게 자연스럽게 생성되고 이용될 것이다. 그저 인식하고 받아들이면 된다.<br><br>

**클로저**`는 함수가 속한 렉시컬 스코프를 기억하여 함수가 렉시컬 스코프 밖에서 실행될 때에도 이 스코프에 접근할 수 있게 하는 기능이다.`

이게 무슨 말인지 아래 코드를 보면서 이해해봅시다.<br>

```
function foo() {
  var a = 2;
  function bar() {
    console.log(a);
  }
  return bar;
}

var baz = foo();
baz(); // 2
```

함수 foo()가 호출되면서 bar()함수를 반환하고 생을 마감했다. 그 이유는 자바스크립트 엔진이 가비지 콜렉터를 사용해 더 이상 사용하지 않는 메모리(여기서는 foo()함수)를 해제 시키기 때문이다.<br>

그렇기 때문에 일반적으로는 foo()함수가 더 이상 사용되지 않기 때문에 사라지는 것이 당연하다. 따라서 bar가 호출되었을 때 a라는 변수는 존재하지 않기 때문에 undefined가 출력이 되어야 하는 것이 사실이다.<br>

하지만 실제 결과값은 2가 출력이 된다. 어떻게 된 것일까?<br>

또한 bar()함수는 원래 코드의 렉시컬 스코프에서 완전히 벗어난 영역에서 호출하고 있다. 이는 스코프 규칙에 어긋나는 행위이다. 그럼에도 불구하고 bar()함수를 실행할 수 있는 것은 무슨 이유가 있기 때문에 가능한 것일까?<br>

위 코드에서 foo()함수는 bar()함수를 반환하고 일반적으로 자바스크립트 엔진이 가비지 컬렉터를 호출해 더 이상 사용하지 않는 메모리로 간주하여 foo()함수를 해제하는 것을 피해갈 수 없다.<br>

하지만 foo()함수는 bar()함수를 반환하고도 foo()함수가 해제되지 않는데 이것은 foo함수(외부함수) 내에 있는 bar함수(내부함수)가 아직 사용 중이기 때문이다.(bar함수가 변수 baz에 할당되었음)<br>

foo()함수는 bar()함수가 나중에 참조할 수 있도록 foo()함수의 렉시컬 스코프를 살려두게 되는데 bar()함수는 해당 스코프에 대한 참조를 가지게 되며 이것을 **클로저** 라고 하는 것이다.<br>

(여기서 bar함수의 클로저는 bar가 선언되었을 당시의 렉시컬 스코프(여기서는 foo 렉시컬 스코프)를 기억하기 때문에 var a와 bar함수가 클로저 안에 살아있게 된다.)<br>

`즉, 클로저는 반환된 내부함수가 자신이 선언되었을 때의 환경인 스코프를 기억하여 자신이 선언됐을 때의 환경 밖에서 호출되어도 그 환경에 접근할 수 있는 함수를 뜻한다.`<br>

요약 -> 클로저는 자신이 생성됐을 때의 환경을 기억하는 함수다.<br>

어떤 방식이든 함수를 값으로 넘겨 다른 위치에서 호출하는 행위는 모두 클로저가 적용된 예이다.<br>

### 클로저를 왜 써야 하는가?

만약 클로저라는 기능이 없게 된다면 어떻게 될까?<br>

1번째 코드<br>

```
var incleaseBtn = document.getElementById('inclease');
var count = document.getElementById('count');

function increase() {
  // 카운트 상태를 유지하기 위한 지역 변수
  var counter = 0;
  return ++counter;
}

incleaseBtn.onclick = function () {
  count.innerHTML = increase();
};
```

클로저는 최신 상태 유지를 해야하는 상황에 매우 유리하다. 위 코드에서 incleaseBtn을 아무리 클릭해도 counter값은 항상 increase함수가 호출되어 1로 고정이 된다.<br>

클로저가 없다면 상태 관리를 하기 위해 counter값을 전역 변수로 지정해줘야 한다.<br>

2번째 코드<br>

```
var incleaseBtn = document.getElementById('inclease');
var count = document.getElementById('count');

// 카운트 상태를 유지하기 위한 전역 변수
var counter = 0;

function increase() {
  return ++counter;
}

incleaseBtn.onclick = function () {
  count.innerHTML = increase();
};
```

2번째 코드는 1번째 코드와 달리 increaseBtn을 클릭하면 counter값이 누를 때마다 1씩 증가할 것이다.<br>

하지만 알다시피 변수를 전역적으로 관리한다는 것은 충돌이 발생하기 쉽고 최소 권한의 원칙을 지킬 수 없게 된다. 함수 스코프 내에서 상태 관리를 할 때 클로저는 아주 유용하게 사용될 수 있다.<br>

3번째 코드

```
var incleaseBtn = document.getElementById('inclease');
var count = document.getElementById('count');

var increase = (function () {
  // 카운트 상태를 유지하기 위한 자유 변수
  var counter = 0;
  // 클로저를 반환
  return function () {
    return ++counter;
  };
}());

incleaseBtn.onclick = function () {
  count.innerHTML = increase();
};
```

3번째 코드는 increase함수 안에 counter 변수를 뒀음에도 불구하고 클로저를 사용해 버튼을 누를 때마다 최신 상태를 유지하는 것이 가능해졌다.<br>

클로저 덕분에 counter라는 변수를 전역적으로 노출시키지 않아도 되었고 increase함수 안에서만 상태를 관리할 수 있게 되었다.<br>

참고 -> You don't know JS
참고 -> PoiemaWeb
