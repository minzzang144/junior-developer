# Script

### <script>, <script async>, <script defer> 사이의 차이점

1. 첫번째, head안에 script를 포함하는 경우

```
<!DOCTYPE html>
<html lang="en">
<head>
  <script src="main.js"></script>
</head>
<body>

</body>
</html>
```

사용자가 html파일을 다운로드 받았을 때, 브라우저가 한줄한줄씩 분석(파싱)한다. 이렇게 한줄한줄씩 읽다가, script태그를 만나게 되고, main.js를 다운받아야한다는 것을 알게된다.

html파싱을 잠시 멈추고, 필요한 자바스크립트 파일을 서버에서 다운로드 받아 이것을 실행한 다음 다시 html파싱을 시작한다.

단점은, 만약 js 파일이 매우 크고, 인터넷이 매우 느리다면 사용자가 웹사이트를 보는데까지 많은 시간이 걸린다. 그래서 스크립트를 그냥 헤드에 포함하는 방법은 좋지 않다.

2. 두번째, body의 제일 끝 부분에 script 배치

```
<!DOCTYPE html>
<html lang="en">
<head>

</head>
<body>
  <div></div>
  <script src="main.js"></script>
</body>
</html>
```

이 방법은 사용자가 기본적인 HTML컨텐츠를 빨리 볼 수 있다는 장점이 있지만 만약 웹사이트가 자바스크립트에 굉장히 의존적인 사이트라면, 즉 사용자가 의미있는 컨텐츠를 보기 위해 자바스크립트를 이용해 서버에 있는 데이터를 받아온다던지, 아니면 DOM요소를 더 예쁘게 꾸며준다던지... 이런 식의 웹페이지라면 사용자는 fetching 하는 시간도 기다려야 하고, 실행하는 시간도 기다려야 하는 단점이 있다.

3. 세번째, head + async

```
<head>
  <script async src="main.js"></script>
</head>
```

async 옵션을 사용하는 경우, 브라우저가 html을 다운받아 파싱을 하다가 async를 발견하게 되고, html파싱과 동시에 병렬로 js파일을 다운로드 받는다.

js가 다운로드가 완료되었다면 파싱하는 것을 멈추고 js를 실행하게 된다. 실행이 모두 끝난 후 나머지 html 파일을 파싱한다.

이 방법은 body끝에 사용하는 것보다는 fetching이 병렬적으로 일어나기 때문에 다운로드받는 시간을 절약할 수 있다. 하지만 js가 html이 파싱되기도 전에 실행되기 때문에 만약 js 파일에서 쿼리 셀렉터로 돔 요소를 조작하려 할 때, 조작하는 시점에 원하는 요소가 아직 정의되지 않아 문제가 발생할 수 있다. 또한 js 가 실행되는 동안에 block되므로, 사용자가 웹 페이지를 보는데까지 시간이 걸린다.

4. 네번째, head + defer

```
<head>
	<script defer src="main.js"></script>
</head>
```

파싱을 쭉- 하다가, defer를 만나게 되면 'js파일을 다운로드 받아!' 이렇게 명령만 시켜놓고 나머지 html을 끝까지 파싱한다. html파싱이 모두 끝난 후에야 js를 실행한다.

### async 와 defer 의 차이

```
<head>
  <script async src="a.js"></script>
  <script async src="b.js"></script>
  <script async src="c.js"></script>
</head>
```

async 옵션으로 다수의 스크립트를 다운받게 되면, 먼저 다운로드 받은 스크립트를 실행하게 된다. 정의된 스크립트의 순서에는 상관없이, 먼저 다운로드 받은 것을 실행하기 때문에 만약 내가 작성한 자바스크립트가 순서에 의존적인 것이라면, 예를들어 a.js 가 먼저 선행이 되어야 한다면 async옵션은 조금 위험할 수 있다.

```
<head>
  <script defer src="a.js"></script>
  <script defer src="b.js"></script>
  <script defer src="c.js"></script>
</head>
```

defer옵션을 사용하면, 스크립트 파일을 원하는 순서대로 실행할 수 있다.(a -> b -> c)

### 요약

- <script> - HTML 파싱이 중단되고, 스크립트를 즉시 가져오고 실행되며, 스크립트 실행 후 HTML 파싱이 다시 시작됩니다.


- <script async> - 이 스크립트는 HTML 파싱과 병렬적으로 가져오며, 가능할 때 즉시 실행됩니다(아마 HTML 파싱이 끝나기 전). 스크립트가 페이지의 다른 스크립트들과 독립적인 경우 async를 사용하세요.


- <script defer> - 이 스크립트는 HTML 파싱과 병렬적으로 가져오지만, 페이지 파싱이 끝나면 실행됩니다. 이 것이 여러개 있는 경우, 각 스크립트는 페이지에 등장한 순서대로 실행됩니다.
