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
