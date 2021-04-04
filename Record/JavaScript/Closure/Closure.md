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

그렇기 때문에 일반적으로는 foo()함수가 더 이상 사용되지 않기 때문에 사라지는 것이 당연하다. 따라서 bar(var baz = foo()에서 baz는 foo가 반환한 함수 bar이다. 여기서는 baz를 bar라고 명칭하겠다.)가 호출되었을 때 a라는 변수는 존재하지 않기 때문에 undefined가 출력이 되어야 하는 것이 사실이다.<br>

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

### 반복문과 클로저

클로저를 설명하는 가장 대표적인 예는 `for 반복문`이다.<br>

아래 코드를 보자.<br>

```
for(var i=1; i<=5; i++) {
  setTimeout(function timer() {
    console.log(i);
  }, i*1000);
}
```

아마도 이 코드의 결과를 1, 2, 3, ... 5를 일초에 하나씩 출력하는 것을 예상할 것이다.<br>

하지만 실제 코드의 결과는 6만 5번 출력이 된다. 어떻게 6이 나오게 되는 것일까?<br>

6이 나올 수 있는 경우를 생각해 본다면 반복문이 끝났을 때의 i값뿐이다.<br>

사실 당연한 것이 setTimeout함수는 비동기로 작동한다. 즉, timer 콜백함수는 반복문이 다 끝나고 나서야 실행 되는 것이다. 그렇기 때문에 반복문이 끝나고 동작한 timer함수는 6을 5번 출력한다.<br>
([브라우저의 작동 방식](../../FrontEnd/Browser-Action/Browser-Action.md)에서 비동기 처리가 어떻게 되는지 안다면 이해하기 수월할 것이다.)<br>

만약 우리가 원했던 결과값을 출력하기 위해서는 어떻게 해야할까? 필요한 것은 반복마다 각자의 i 복제본을 잡아두는 것이다.<br>

그러나 반복문 안 총 5개의 함수들은 반복마다 따로 정의되었음에도 불구하고 모두 같이 글로벌 스코프를 공유하여 해당 스코프 내에는 오직 하나의 i만이 존재한다.<br>

따라서 필요한 것은 닫힌(closured) 스코프다. 구체적으로 반복마다 하나의 새로운 닫힌 스코프가 필요하다.<br>

아래가 그 코드이다.<br>

```
for(var i=1; i<=5; i++) {
  (function() {
    var j = i;
    setTimeout(function timer() {
      console.log(j);
    }, j*1000);
  })
}
```

var로 호이스팅 된 변수 i를 닫힌 스코프 내에 j라는 변수에 담아둠으로써 이제 반복문마다 모두 i를 공유하는 것이 아닌 j라는 변수를 초마다 출력할 수 있게 되었다.<br>

timer 콜백함수는 반복문이 다 끝난 후에도 닫힌 스코프 내에서 각각 j라는 변수가 다른 값을 가지고 있기 때문에 우리가 원하는 1, 2 ... 5값을 초마다 출력하는 결과를 가져온다.<br>

우리는 이 코드를 `let` 키워드를 사용해 더 간결하게 만들 수 있다.<br>

```
for(let i=1; i<=5; i++) {
  setTimeout(function timer() {
    console.log(i);
  }, i*1000);
}
```

let 키워드는 해당 블록 스코프에 변수를 선언하는 것이다.<br>

특히 let은 반복문에서 더 큰 효과를 가지는데 반복문 시작 부분에 let으로 선언된 변수는 한번만 선언되는 것이 아니고 반복할 때마다 선언된다.<br>

따라서 해당 변수는 편리하게도 반복마다 이전 반복이 끝난 이후의 값으로 초기화된다.<br>

즉, 우리가 지금까지 var와 let에 따라 코드를 다르게 작성한 이유는 다음과 같다.<br>

var는 hoisting이 일어나 글로벌 스코프에 선언됨으로써 반복할 때마다 함수가 같은 i를 공유함으로써 6이 5번 출력되는 반면 let은 호이스팅 없이 블록 스코프에 고정이 되면서 블록 스코프 내의 클로저를 사용해 1부터 5까지 출력을 할 수 있는 것이다.<br>

요약 -> 반복문 내에서는 let 키워드를 사용하여 호이스팅이 일어나지 않게 해주는 것이 좋다.<br>

참고 -> You don't know JS
참고 -> PoiemaWeb
