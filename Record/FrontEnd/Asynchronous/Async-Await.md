# Async Await

[이전 글 보러가기 -> Promise](./Promise.md)

### Async Await는 무엇인가?

자바스크립트 ES8에서는 프로미스 사용을 쉽게 해주는 async/await를 도입했습니다. 먼저, 작동 방법부터 확인해봅시다.<br>

- async

  `async`키워드를 통해 함수 선언을 하게 되면 비동기 함수임을 정의합니다.

  이렇게 생성된 함수는 AsyncFunction 객체를 반환하게 되는데 이 객체는 해당 함수 내에 포함되어 있는 코드를 수행하는 비동기 함수를 나타냅니다.

  즉, async를 통해 만들어진 함수가 호출되면 하나의 프로미스를 반환하게 됩니다.

  만약 async함수가 프로미스가 아닌 값을 반환해도 프로미스는 자동으로 생성되며 해당함수로부터 반환받은 값을 resolve 하게 됩니다.

  반대로 async함수가 에러를 발생시키면 프로미스가 생성되고 예외와 함께 reject 하게 됩니다.

  따라서 아래 코드는 같은 코드임을 나타냅니다.

  ```
  // Just a standard JavaScript function
  function getNumber1() {
      return Promise.resolve('374');
  }
  // This function does the same as getNumber1
  async function getNumber2() {
      return 374;
  }
  ```

  ```
  function f1() {
      return Promise.reject('Some error');
  }
  async function f2() {
      throw 'Some error';
  }
  ```

- await

  `await`키워드는 async 함수 내에서만 사용할 수 있는데 동기적으로 프로미스를 기다릴 수 있게 해줍니다.<br>

  ```
  async function loadData() {
      // `rp` is a request-promise function.
      var promise1 = rp('https://api.example.com/endpoint1');
      var promise2 = rp('https://api.example.com/endpoint2');

      // Currently, both requests are fired, concurrently and
      // now we'll have to wait for them to finish
      var response1 = await promise1;
      var response2 = await promise2;
      return response1 + ' ' + response2;
  }
  // Since, we're not in an `async function` anymore
  // we have to use `then`.
  loadData().then(() => console.log('Done'));
  ```

  보다시피 async 함수 밖에서 프로미스를 사용한다면 여전히 then 후속 처리 메서드를 사용해야 합니다.

  그리고 비유를 하자면 async는 Promise의 producer 역할을 하고 await는 Promise의 consumer로 후속 처리를 해주는 역할이라고 볼 수 있습니다.

### Async Await를 사용하는 이유

- async & await를 사용하면 훨씬 더 적은 코드로 작성할 수 있습니다.

  Promise를 사용하면 후속처리 메서드를 붙이고 응답을 처리하기 위한 콜백함수와 그 콜백에서 응답을 받기 위한 작업을 해야하는 반면 await는 await 키워드를 사용해 단 한 줄로 해결할 수 있습니다.<br>

- 에러처리하기 더 좋다.

  async & await를 사용하면 동일한 코드 구조로 비동기 코드와 동기 코드의 에러를 처리하는 것이 가능합니다.

  이것은 try / catch와 함께 async await를 사용하기 때문에 가능합니다.

  한번 둘을 비교해봅시다.<br><br>

  Promise

  ```
  function loadData() {
      try { // Catches synchronous errors.
          getJSON().then(function(response) {
              var parsed = JSON.parse(response);
              console.log(parsed);
          }).catch(function(e) { // Catches asynchronous errors
              console.log(e);
          });
      } catch(e) {
          console.log(e);
      }
  }
  ```

  Async / Await

  ```
  async function loadData() {
      try {
          var data = JSON.parse(await getJSON());
          console.log(data);
      } catch(e) {
          console.log(e);
      }
  }
  ```

  한 번에 비동기 프로미스와 동기 코드에서 일어나는 에러를 잡아낼 수 있다.

- 프로미스 체인은 async await와 달리 어디에서 에러가 발생했는지에 대한 정보를 주지 않는다.

- 프로미스를 사용해 디버깅하는 것보다 async await를 사용해 디버깅하는 것이 더 쉽다.

  프로미스 내의 then메서드에서 브레이크 포인트를 잡아도 스텝 오버와 같은 명령을 사용해도 디버거는 다음으로 이동하지 않는데 이것은 디버거가 오직 동기 코드만을 지나가기 때문이다.

  async await를 사용하면 await 호출이 마치 일반적인 동기 함수인 것처럼 지나가기 때문에 디버깅할 수 있습니다.

요약 -> aysnc함수는 프로미스 함수를 반환하지 않아도 새로운 하나의 프로미스를 반환하고 await는 프로미스를 동기적으로 기다릴 수 있게 해준다. 또한 async await는 try/catch와 함께 사용하면 가독성이 좋으며 일반 프로미스를 처리하는 것보다 간편하다.<br>
