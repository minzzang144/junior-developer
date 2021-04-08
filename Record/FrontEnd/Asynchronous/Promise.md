# Promise

[이전 글 보러가기 -> Callback](./Callback.md)

### Promise란?

지난 글에서 콜백 함수를 여러 번 남발하면 콜백지옥이 발생하게 되고 코드의 가독성이 좋지않아 유지보수가 어렵다는 것을 확인했습니다.<br>

그래서 만약 비동기 코드를 마치 동기식 코드처럼 작성할 수 있다면 비동기 처리를 하는데 쉽게 해결할 수 있지 않을까라는 것이 지난 글의 궁금한 부분이었습니다.<br>

ES6에서는 위의 문제점을 해결하기 위해 비동기 처리를 위한 또 다른 패턴으로 프로미스(Promise)를 도입했으며 이는 마치 비동기 코드를 동기식으로 처리할 수 있도록 도와줍니다.<br>

그리고 Promise는 콜백함수로 비동기를 처리할 때 에러를 캐치하지 못하는 문제점을 보완하는 역할도 해줍니다.<br>

아래 코드를 봅시다.<br>

```
try {
  setTimeout(() => { throw new Error('Error.'); }, 1000);
} catch (e) {
  // 에러를 캐치할 수 없다.
  console.log(e);
}
```

setTimeout함수에서 콜백함수로 에러를 발생시켰지만 catch문에서 에러를 캐치하지 못합니다.<br>

자바스크립트는 비동기 코드를 보면 일단 실행하고 지나가기 때문에 실행결과를 받을 때까지 기다리지 않아 에러를 캐치하지 못합니다.<br>

반면, `Promise`는 에러를 캐치하는 것이 가능할 뿐 아니라 비동기 코드를 동기 코드로 작성할 수 있습니다.<br>

Promise를 사용하는 예시를 봅시다. Promise는 Producer과 Consumer 측면으로 나뉘어 있으며 Producer측 부터 보겠습니다.<br>

```
// Promise 객체의 생성
const promise = new Promise((resolve, reject) => {
  // 비동기 작업을 수행한다.

  if (/* 비동기 작업 수행 성공 */) {
    resolve('result');
  }
  else { /* 비동기 작업 수행 실패 */
    reject('failure reason');
  }
});
```

Promise는 생성자 함수(new)를 통해 인스턴스화합니다.<br>

그리고 Promise는 하나의 콜백함수를 받으며 콜백함수 인자로 `resolve`, `reject`를 받습니다.<br>

콜백함수 내부에서 비동기 작업을 처리하고 비동기 처리가 성공했을 때 콜백함수 인자인 resolve함수를 호출해 결과값을 넘겨줄 것이고 비동기 처리가 실패했을 때 콜백함수 인자인 reject함수를 호출해 실패했다는 정보를 알려줄 것이다.<br>

이것이 Promise함수의 기본적인 형태이며 **producer** 측 입니다. 또한 Promise는 위와 같이 비동기 처리를 한 후 비동기 처리가 성공했는지 실패했는지 정보를 가지고 있습니다.<br>

|   상태    |                 의미                  |                        구현                        |
| :-------: | :-----------------------------------: | :------------------------------------------------: |
|  pending  | 비동기 처리가 아직 수행되지 않은 상태 | resolve 또는 reject 함수가 아직 호출되지 않은 상태 |
| fulfilled |    비동기 처리가 수행된 상태(성공)    |             resolve함수가 호출된 상태              |
| rejected  |    비동기 처리가 수행된 상태(실패)    |              reject함수가 호출된 상태              |

### Promise의 후속 처리 메서드

지금까지 한 것은 Promise의 `producer` 측면 입니다. 이제 Promise로 구현된 비동기 함수를 호출하는 측(**consumer**)을 알아보겠습니다.<br>

Promise Consumer는 Promise Producer에서 반환한 resolve와 reject의 결과값을 받아오기 위해 `후속 처리 메서드`를 사용합니다.<br>

후속 처리 메서드는 then, catch, finally 등이 있습니다.<br>

- then

  then메서드는 두 개의 콜백함수를 전달 받으며 첫번째 콜백함수는 fulfilled(성공)상태 시 호출되고 두번째 콜백함수는 rejected(실패)상태 시 호출된다.

  중요한 것은 then 메서드는 Promise를 반환한다는 것이다. 이것이 왜 중요한지 밑에서 설명하겠습니다.

- catch

  비동기 처리에서 발생한 에러와 then메서드에서 에러가 발생하면 호출됩니다.

  마찬가지로 catch 메서드도 Promise를 반환합니다.

```
var promise = new Promise((resolve, reject) => {
  if(true))) {
    resolve("Promise 성공!");
  } else {
    reject("Promise 실패..");
  }
})

promise()
  .then(res => console.log(res), err => console.log(err))
```

위에서 어떤 promise가 호출되었을 때 resolve메서드가 호출되었는지 reject메서드가 호출되었는지에 따라 후속 처리 메서드인 then 메서드에서 그에 따른 결과값이 반환될 것입니다.<br>

여기서는 resolve함수가 호출되었으므로 then 메서드의 첫번째 콜백함수가 실행이 됩니다.<br>

문제는 then메서드에서 두번째 콜백함수는 첫번째 콜백함수에서 발생한 에러를 캐치할 수 없다는 것입니다.<br>

이것을 해결하기 위해 보통 catch메서드를 사용합니다.<br>

```
promise()
  .then(res => console.log(res))
  .catch(err => console.log(err));
```

catch메서드는 Promise 비동기 함수에서 발생한 에러(reject)뿐만 아니라 then 내부 메서드에서 발생한 에러도 잡아주기 때문에 일반적으로 then에서 두개의 콜백함수를 받기보다 then과 catch 메서드를 같이 사용하는 것이 좋습니다.<br>
