# 콜백 함수

[이전 글 보러가기 -> 브라우저 비동기 처리](./Asynchronous.md)

### 콜백이 뭔가요?

콜백 함수는 자바스크립트 비동기 처리에서 가장 많이 쓰이는 패턴입니다.<br>

지난 번 그림을 인용해서 설명해보겠습니다.<br>

![Frontend 05](../../../Image/frontend-05.gif)

setTimeout()함수는 2개의 인자를 받는데 첫번째 인자가 timer함수이고 두번째 인자가 timer함수가 얼마나 기다렸다 실행될 것인지 시간을 알려주는 값입니다.<br>

여기서 첫번째 인자로 받는 timer함수를 콜백함수라고 부르는데 왜 콜백함수인지 지금부터 이해과정을 통해 알아봅시다.<br>

setTimeout()함수는 콜 스택에서 실행되자마자 timer 콜백함수를 Web APIs로 넘겨주게 됩니다. Web APIs는 콜백함수를 받는 순간부터 두번째 인자로 받은 시간을 기다리게 됩니다.<br>

타임아웃이 되면 태스크 큐로 콜백함수를 주고 태스크 큐는 계속해서 콜 스택이 비어있는지 확인한 후에 콜 스택으로 콜백함수를 돌려주게 됩니다.<br>

이름에서부터 느껴지듯이 콜백함수는 콜스택에서 실행되자마자 어딘가(Web APIs)로 끌려가게 되는 대신 모든 작업을 마치고 콜 스택이 비게 된 후에서야 나중에 다시 돌려받게 되는 것이 바로 **콜백함수** 라고 합니다.<br>

의미가 어렵게 느껴진다면 간단하게 이야기해서 `함수 인자로 받는 함수` 정도로 생각하면 될 것 같습니다.<br>

### 중첩 콜백

콜백 함수가 비동기 처리에서 매우 유용한 패턴이지만 콜백 함수도 장점만 가지고 있는 것은 아닙니다.<br>

아래 코드를 봅시다.<br>

```
listen('click', function (e){
    setTimeout(function(){
        ajax('https://api.example.com/endpoint', function (text){
            if (text == "hello") {
	        doSomething();
	    }
	    else if (text == "world") {
	        doSomethingElse();
            }
        });
    }, 500);
});
```

listen함수는 하나의 이벤트 콜백함수를 가지고 있으며 그 콜백함수는 setTimeout함수를 포함하고 있습니다.<br>

또한 setTimeout함수는 하나의 콜백함수를 받게 되어있는데 이 콜백함수는 ajax()함수로 ajax()함수 역시 하나의 콜백함수를 가지고 있습니다.<br>

즉, 이 코드는 3단계의 중첩 콜백이 되어있으며 코드 해석을 하는데 많은 혼란과 어려움을 줍니다.<br>

비동기 처리를 하는데 있어 콜백 함수는 필연적입니다. 그렇다면 우리는 비동기 처리를 하기 위해 이렇게 헷갈리는 코드를 작성해야 하는 것일까?<br>

이 코드가 만약 동기식으로 처리하게 된다면 어떻게 바뀔 수 있을까요?<br>

첫번째 실행함수<br>

```
listen('click', function (e) {
	// ..
});
```

그 다음 함수<br>

```
setTimeout(function(){
    // ..
}, 500);
```

그 그 다음 함수<br>

```
ajax('https://api.example.com/endpoint', function (text){
    // ..
});
```

마지막으로 실행되는 코드<br>

```
if (text == "hello") {
    doSomething();
}
else if (text == "world") {
    doSomethingElse();
}
```

보다시피 비동기 코드를 동기식으로 바꾸게 되니 코드를 보는 것이 명확해졌습니다.<br>

비동기 처리를 동기식으로 작성하듯이 하는 방법이 있다면 굉장히 편리할 것임을 확신했습니다.<br>

그 방법을 다음 글에서 다뤄보겠습니다.<br>

참고 -> [이벤트 루프와 비동기 프로그래밍의 부상](https://engineering.huiseoul.com/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%9E%91%EB%8F%99%ED%95%98%EB%8A%94%EA%B0%80-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84%EC%99%80-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%98-%EB%B6%80%EC%83%81-async-await%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%BD%94%EB%94%A9-%ED%8C%81-%EB%8B%A4%EC%84%AF-%EA%B0%80%EC%A7%80-df65ffb4e7e)<br>
