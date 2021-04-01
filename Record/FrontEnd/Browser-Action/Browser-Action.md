# 브라우저 동작 방식

[이전 글로 이동하기 -> 브라우저 렌더링 방식](../Browser-Render/Browser-Render.md)

### 자바스크립트는 단일 스레드인가? 멀티 스레드인가?

자바스크립트는 단일 스레드 기반 언어이다. 때문에 자바스크립트 엔진도 단일 콜 스택을 가집니다.<br>

이 말은 곧 하나의 작업(tast)만을 처리한다는 것을 의미합니다. 하지만 실제로 동작하는 웹 어플리케이션에서는 많은 작업들이 동시에 처리되는 것처럼 느껴진다.<br>

이것이 가능한 이유는 브라우저의 환경이 가지고 있는 **이벤트 루프(Event Loop)** 덕분이다.<br>

### 브라우저 환경

자바스크립트 엔진 중 유명한 것이 구글의 `V8`엔진입니다. V8엔진은 크롬과 노드JS에서 사용이 되며 다음과 같이 구성되어 있습니다.<br>

![FrontEnd 02](../../../Image/javascript-02.png)<br>

V8엔진은 크게 두 부분으로 이루어진다.<br>

- 메모리힙(Memory Heap)

  메모리 할당이 이루어지는 곳이다.

- 콜스택(Call Stack)

  코드 실행이 이루어지면서 스택 프레임이 쌓이는 곳이다.

보다시피 자바스크립트 엔진은 하나의 콜 스택을 가지는데 이것만으로는 절대 동시에 여러 작업들이 이루어질 수 없습니다.<br>

콜 스택은 동기식으로 이루어지기 때문에 한번에 단 하나의 작업만을 처리할 수 있기 때문입니다.<br>

그렇다면 어떻게 브라우저가 동시성 작업을 지원할까요?<br>

브라우저 구성 환경은 자바스크립트 엔진 이외에 많은 것들을 구성하고 있습니다.<br>

![FrontEnd 03](../../../Image/javascript-03.png)<br>

사진을 보면 처음 언급한 자바스크립트 엔진 외에 브라우저는 **Web APIs** & **Task Queue** & **Microtask Queue** 그리고 **Event Loop** 등으로 구성되어 있습니다.<br>

각각 무슨 일을 하는지 개념을 간단하게 정리해 봅시다.<br>

- Web APIs

  DOM 이벤트, AJAX, Timeout등 비동기 처리 실행문이 대부분 담기게 됩니다.

- Task Queue(Callback Queue / Event Queue 등 명칭이 다양하다.)

  Event Handler, 비동기 처리 함수의 콜백 함수, Timer함수(setTimeout(), setInterval())의 콜백 함수가 보관되는 영역으로 이벤트 루프에 의해 특정 시점에 순차적으로 콜백으로 이동되어 실행됩니다.

- Microtask Queue

  Promise, MutationObserver 등의 콜백함수가 보관되는 영역

  Task Queue보다 Microtask Queue가 우선순위를 가지기 때문에 Microtask Queue의 콜백함수를 전부 실행하고 나서 Task Queue의 콜백함수들을 실행합니다.

- Event Loop

  Call Stack내에서 현재 실행중인 task가 있는지 그리고 Task Queue에 task가 있는지 반복하여 확인한다.

  만약 Call Stack이 비어있다면 Event Queue 내의 task가 Call Stack으로 이동하고 실행한다.

개념 설명을 이해하기 쉽게 하나의 예시를 살펴봅시다.<br>

```
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  setTimeout(function () {
    console.log('func2');
  }, 0);

  func3();
}

function func3() {
  console.log('func3');
}

func1();
```

브라우저는 위의 자바스크립트 코드를 읽고 대략 다음과 같은 순서를 거칩니다.<br>

![FrontEnd 04](../../../Image/javascript-04.gif)<br>

1. func1이 호출되면 Call Stack에 쌓인다.

2. func1은 func2를 호출하기 때문에 func2를 Call Stack에 쌓습니다.

3. func2함수에서 setTimeout을 호출하기 때문에 setTimeout이 Call Stack에 쌓습니다. 그 후 브라우저가 setTimeout을 Call Stack에서 Web APIs로 이동시키고 타이머를 동작시킵니다.<br>
   타이머가 끝나면 setTimeout의 콜백함수를 Task Queue에 넣는다.

4. func2함수에서 func3함수를 호출하기 때문에 func3을 Call Stack에 쌓습니다.

5. func3, func2, func1이 차례대로 Call Stack에서 빠져나가고 Event Loop는 Call Stack이 비어있는지 계속 확인하다 Call Stack이 비어있을 경우 Task Queue에 있는 Task들을 하나하나씩 가져와 마지막으로 작업하게 됩니다.

이는 setTimeout뿐만이 아니라 모든 비동기 작업이 수행되는 과정입니다.<br>

중요한 것은 setTimeout은 1번째 인자로 수행할 콜백함수를 받고 2번째 인자로 콜백함수를 언제 실행할지 정하는 시간을 받는데 정확히 2번째 인자로 받은 시간 뒤에 setTimeout의 콜백함수가 실행되는 것이 아니고 위와 같이 Web APIs 및 Task Queue로 이동 후 Call Stack이 비어져있을 때 비로소 setTimeout의 콜백함수가 실행되는 것입니다.<br>

때문에 setTimeout 콜백함수를 3초 뒤에 실행하도록 코드를 썼더라도 만약 Call Stack이 비어있지 않다면 3초 뒤에도 실행하지 못할 수 있다는 것입니다.<br>

자바스크립트 엔진이 굉장히 빠르게 동작하기 때문에 Call Stack이 꽉차있을 경우는 거의 없지만 만약 자바스크립트에서 서버에 용량이 큰 데이터를 요청한다던가 복잡한 이미지를 변형해야하는 작업을 해야한다고 가정해봅시다.<br>

이러한 Task가 Call Stack에 쌓이게 된다면 어떻게 될까요? 자바스크립트 엔진은 앞의 복잡한 Task를 처리할 때까지 아무것도 할 수 없게 됩니다.<br>

만약 이러한 상황이 발생한다면 사용자가 브라우저를 사용할 때 어떠한 상호작용도 바랄 수 없습니다. 클릭을 하거나 스크롤을 내리고 싶어도 브라우저가 Freeze상태가 되어버리는 것 입니다.<br>

단일 Call Stack을 가진 브라우저 환경 때문에 우리는 이러한 문제를 해결하기 위해 사용하는 방법이 바로 **비동기 처리** 라고 하는 유명한 처리방식 입니다.<br>

비동기 처리는 다음에 다뤄보도록 하겠습니다.<br>

참고 -> [자바스크립트는 어떻게 작동하는가?](https://engineering.huiseoul.com/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%9E%91%EB%8F%99%ED%95%98%EB%8A%94%EA%B0%80-%EC%97%94%EC%A7%84-%EB%9F%B0%ED%83%80%EC%9E%84-%EC%BD%9C%EC%8A%A4%ED%83%9D-%EA%B0%9C%EA%B4%80-ea47917c8442)<br>
참고 -> [이벤트 루프](https://poiemaweb.com/js-event)<br>
