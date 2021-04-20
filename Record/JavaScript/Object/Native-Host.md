# 네이티브 객체 VS 호스트 객체

### 네이티브 객체

ECMAScript 사양에 정의된 객체들로 자바스크립트 언어의 일부인 객체들이다.<br>

EX)

- Object

- Function

- RegExp

- String

- Number

- Math

- Date

등등...

### 호스트 객체

자바스크립트를 실행하는 환경에 종속된 객체들로 그 환경에서만 찾아볼 수 있는 객체이다.<br>

예를 들어 브라우저라면 다음과 같은 호스트 객체를 가지고 있다.<br>

- window

- BOM

  - window

  - document

  - location

  ...

- DOM

  - html

  - head

  - body

  ...

- XMLHttpRequest

등등...

노드 환경이라면 다음과 같은 호스트 객체를 가지고 있다.<br>

- http

- fs

- url

- os

등등...
