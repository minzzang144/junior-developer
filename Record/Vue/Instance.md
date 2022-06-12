# 어플리케이션 & 컴포넌트 인스턴스

### 최상위(Root) 컴포넌트

createApp에 전달된 옵션은 루트 컴포넌트를 구성하는데 사용됩니다. 이 컴포넌트는 어플리케이션을 mount할 때, 렌더링의 시작점으로 사용됩니다.

어플리케이션을 DOM 요소에 마운트되어야합니다. 예를들어, Vue 어플리케이션을 <div id="app"></div>에 마운트하려면 #app을 전달해야 합니다.

```javascript
const RootComponent = {
  /* options */
};
const app = Vue.createApp(RootComponent);
const vm = app.mount("#app");
```

대부분의 어플리케이션 메소드와 달리, mount는 어플리케이션을 반환하지 않습니다. 대신 루트 컴포넌트 인스턴스를 반환합니다.

MVVM 패턴 (opens new window)과 밀접하게 관련있지는 않지만, Vue의 디자인은 MVVM 패턴에 의해 부분적으로 영감을 받았습니다. 종종 인스턴스를 참조하기 위해서 변수 vm (ViewModel의 줄임말)를 종종 사용합니다. 각 컴포넌트에는 고유한 컴포넌트 인스턴스 vm이 있습니다.

### 컴포넌트 인스턴스 속성들

data에 정의된 속성은 컴포넌트 인스턴스를 통해 노출됩니다

```javascript
const app = Vue.createApp({
  data() {
    return { count: 4 };
  },
});

const vm = app.mount("#app");

console.log(vm.count); // => 4
```

`methods, props, computed, inject 및 setup`과 같은 사용자 정의 속성을 컴포넌트 인스턴스에 추가하는 다양한 다른 컴포넌트 옵션들이 있습니다. 추후에 각각에 대해 자세히 설명하겠습니다. 컴포넌트 인스턴스의 모든 속성은 정의된 방법에 관계없이 컴포넌트의 템플릿에서 접근할 수 있습니다.

Vue는 또한 컴포넌트 인스턴스를 통해 $attrs와 $emit과 같은 몇몇 빌트인(built-in) 속성을 노출합니다. 이러한 속성을 모두 사용자 정의 속성명과 충돌하지 않도록 $ 접두사(prefix)를 가지고 있습니다.

### 라이프사이클 훅

각 컴포넌트는 생성될 때 일련의 초기화 단계를 거칩니다. 예를들어 데이터 관찰, 템플릿 컴파일, 인스턴스를 DOM에 마운트, 데이터 변경 시 DOM을 업데이트해야합니다. 그 과정에서 라이프사이클 훅이라 불리우는 함수도 실행하여, 사용자가 특정 단계에서 자신의 코드를 추가할 수 있는 기회를 제공합니다.

예를들어, created 훅은 인스턴스가 생성된 후에 코드를 실행하는데 사용됩니다.

```javascript
Vue.createApp({
  data() {
    return { count: 1 };
  },
  created() {
    // `this` points to the vm instance
    console.log("count is: " + this.count); // => "count is: 1"
  },
});
```

인스턴스 라이프사이클에는 `mounted`, `updated`, `unmounted` 과 같은 다른 훅도 존재합니다. 모든 라이프사이클 훅에서는 Vue인스턴스를 가리키는 this 컨텍스트와 함께 호출됩니다.

### 라이프사이클 다이어그램

아래는 인스턴스 라이프사이클에 대한 다이어그램입니다. 지금 당장 모든 것을 완전히 이해할 필요는 없지만 다이어그램은 앞으로 도움이 될 것입니다.

![Life Cycle](/Image/lifecycle.svg)
