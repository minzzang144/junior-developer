# Hook

### useEffect

useEffect 훅은 클래스형 컴포넌트에서만 사용할 수 있었던 Lifecycle을 함수형 컴포넌트에서도 어느정도 사용 가능하게 도와주는 함수이다.

```
useEffect(<function>, <Array>);
```

`function`은 컴포넌트가 render 또는 re-render 되었을 때 실행하고 싶은 함수를 의미하고 `Array`에는 어떤 state가 변화되었을 때 컴포넌트를 re-render할지 그 state를 담는 Array형태의 파라미터입니다.

- 화면이 처음 떴을때 실행

  deps에 [] 빈배열을 넣을 때, life cycle중 componentDidmount처럼 실행

- 화면이 사라질때 실행(clean up함수)

  componentWillUnmount처럼 실행

- deps에 넣은 파라미터값이 업데이트 됬을때 실행

  componentDidUpdate처럼 실행

### useRef

JavaScript 를 사용 할 때에는, 우리가 특정 DOM 을 선택해야 하는 상황에 getElementById, querySelector 같은 DOM Selector 함수를 사용해서 DOM 을 선택합니다.

리액트를 사용하는 프로젝트에서도 가끔씩 DOM 을 직접 선택해야 하는 상황이 발생 할 때도 있습니다. 예를 들어서 특정 엘리먼트의 크기를 가져와야 한다던지, 스크롤바 위치를 가져오거나 설정해야된다던지, 또는 포커스를 설정해줘야된다던지 등 정말 다양한 상황이 있겠죠. 추가적으로 Video.js, JWPlayer 같은 HTML5 Video 관련 라이브러리, 또는 D3, chart.js 같은 그래프 관련 라이브러리 등의 외부 라이브러리를 사용해야 할 때에도 특정 DOM 에다 적용하기 때문에 DOM 을 선택해야 하는 상황이 발생 할 수 있습니다.

그럴 땐, 리액트에서 ref 라는 것을 사용합니다. 클래스형 컴포넌트에서는 `React.createRef` 라는 함수를 사용하고 함수형 컴포넌트에서는 `useRef`라는 Hook 함수를 사용합니다.

useRef() 함수는 Ref 객체를 만들고, 이 객체를 우리가 선택하고 싶은 DOM 에 ref 값으로 설정해주어야 합니다. 그러면, Ref 객체의 .current 값은 우리가 원하는 DOM 을 가르키게 됩니다.
