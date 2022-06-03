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

### useMemo

useMemo 훅은 함수가 반환한 값을 메모이제이션 할 때 사용하는 함수이다.

예를 들어 a, b 2가지 상태를 가지고 a 상태와 관련된 값을 반환하는 c함수를 가진 컴포넌트가 있다고 가정해보자. a 상태가 변경될 때마다 리렌더링이 발생해서 c함수가 가진 값도 리렌더링이 된다. 문제는 b 상태가 변경될 때에도 전혀 관련 없는 c함수가 가진 값도 리렌더링 된다는 것이다. 여기서 useMemo 훅을 사용해 a 상태가 변경되었을 때만 c함수가 반환하는 값을 변경하도록 해서 최적화하게 도울 수 있다.

```
useMemo(<function>, <Array>);
```

useMemo 의 첫번째 파라미터에는 어떻게 연산할지 정의하는 함수를 넣어주면 되고 두번째 파라미터에는 deps 배열을 넣어주면 되는데, 이 배열 안에 넣은 내용이 바뀌면, 우리가 등록한 함수를 호출해서 값을 연산해주고, 만약에 내용이 바뀌지 않았다면 이전에 연산한 값을 재사용하게 됩니다.

결론 - useMemo는 함수의 연산량이 많을때 이전 결과값을 재사용하기 위해 사용한다.

### useCallback

useMemo 훅은 특정 결과값을 재사용 할 때 사용하는 반면, useCallback 훅은 특정 함수를 새로 만들지 않고 재사용하고 싶을때 사용합니다.

특히 props로 전달받은 함수는 props나 상태가 업데이트 될 때마다 새로 생성되는데 이 때 최적화할 때 사용하기 좋다.

```
useCallback(<function>, <Array>);
```

useCallback 의 첫번째 파라미터에는 어떻게 연산할지 정의하는 함수를 넣어주면 되고 두번째 파라미터에는 deps 배열을 넣어주면 되는데, 이 배열 안에 넣은 내용이 바뀌면, 우리가 등록한 함수를 새로 선언하고, 만약에 내용이 바뀌지 않았다면 이전에 생성한 함수를 재사용하게 됩니다.

### React.memo

React.memo는 훅이 아니지만 useMemo와 useCallback 훅과 같이 최적화 하는 데 많이 연관짓는 함수여서 넣어보았습니다.

React.memo는 컴포넌트의 props 가 바뀌지 않았다면, 리렌더링을 방지하여 컴포넌트의 리렌더링 성능 최적화를 해줄 수 있게 해줍니다.

- useMemo - 함수가 반환한 값을 메모하여 최적화 함
- useCallback - 함수 자체를 메모하여 재생성을 방지하는 식으로 최적화 함
- React.memo - 컴포넌트를 메모하여 props가 바뀌지 않는다면 리렌더링 하지 않게 하여 최적화 함
