# 커스텀 이벤트

### 이벤트 이름

컴포넌트 및 props와는 달리, 이벤트는 자동 대소문자 변환을 제공하지 않습니다. emit할 이벤트의 이름은 자동 대소문자 변환을 사용하는 것 대신 정확한 이름을 사용하여야 합니다. 예를 들어, 아래와 같이 camelCase로 작성된 이벤트를 emit하는 경우

```js
this.$emit("myEvent");
```

아래와 같이 kebab-case로 이벤트를 청취하는 경우 아무런 일도 일어나지 않습니다.

```html
<!-- 작동안함 -->
<my-component @my-event="doSomething"></my-component>
```

컴포넌트 및 props와는 다르게 이벤트 이름은 자바스크립트 변수나 속성의 이름으로 사용되는 경우가 없으며, 따라서 camelCase나 PascalCase를 사용할 필요가 없습니다. 또한, (HTML이 대소문자를 구분하지 않는 특성 때문에) DOM 템플릿의 v-on 이벤트리스너는 항상 자동으로 소문자 변환되기 때문에 @myEvent 는 자동으로 @myevent로 변환됩니다. -- 즉, myEvent 이벤트를 들을 수 없습니다.

**이러한 이유 때문에, 이벤트 이름에는 항상 kebab-case를 사용하는것이 권장됩니다.**

### 커스텀 이벤트 정의하기

Emit된 이벤트는 컴포넌트 상에서 emits 옵션을 통해 정의될 수 있습니다.

```js
app.component("custom-form", {
  emits: ["in-focus", "submit"],
});
```

만약 emits 옵션을 이용해 네이티브 이벤트(e.g., click)가 재정의 된 경우, 컴포넌트에 정의된 커스텀 이벤트가 네이티브 이벤트를 덮어씁니다.

### Emit 된 이벤트 검사하기

prop 타입 검사와 비슷하게, emit된 이벤트 또한 배열 형식 대신 오브젝트 형식으로 선언함으로써 검사를 추가할 수 있습니다.

검사를 추가하기 위해서는 $emit의 전달인자를 받아 이벤트가 유효한지를 검증하여 boolean을 반환하는 함수를 이벤트에 할당합니다.

```js
app.component("custom-form", {
  emits: {
    // 검사 절차 없음
    click: null,

    // submit 이벤트 검사
    submit: ({ email, password }) => {
      if (email && password) {
        return true;
      } else {
        console.warn("잘못된 이벤트 페이로드입니다!");
        return false;
      }
    },
  },
  methods: {
    submitForm() {
      this.$emit("submit", { email, password });
    },
  },
});
```

### v-model 인자

기본적으로 컴포넌트 상의 `v-model`는 modelValue를 `props`처럼, `update:modelValue`를 `이벤트`처럼 사용합니다. 이 때, v-model에 전달인자를 넘겨줌으로써 이 이름(modalValue)을 변경할 수 있습니다.

```html
<my-component v-model:foo="bar"></my-component>
```

이 경우, 자식 컴포넌트는 foo를 prop으로, 동기화 이벤트에 대해서는 update:foo를 emit하도록 상정합니다.

```js
const app = Vue.createApp({});

app.component("my-component", {
  props: {
    foo: String,
  },
  template: `
    <input
      type="text"
      :value="foo"
      @input="$emit('update:foo', $event.target.value)">
  `,
});
```

### 다중 v-model 바인딩

v-model arguments 단락에서 다루었던 개별 prop과 이벤트를 타겟하는 능력을 극대화하기 위해, 단일 컴포넌트 인스턴스에 대해 다중 v-model 바인딩을 만들 수 있습니다.

각 v-model은 추가 컴포넌트 옵션 없이도 다른 prop을 동기화합니다.

```html
<user-name
  v-model:first-name="firstName"
  v-model:last-name="lastName"
></user-name>
```

```js
const app = Vue.createApp({});

app.component("user-name", {
  props: {
    firstName: String,
    lastName: String,
  },
  template: `
    <input
      type="text"
      :value="firstName"
      @input="$emit('update:firstName', $event.target.value)">

    <input
      type="text"
      :value="lastName"
      @input="$emit('update:lastName', $event.target.value)">
  `,
});
```

**이 글을 [Vue.js](https://v3.ko.vuejs.org/) 공식문서를 참고하여 작성되었습니다.**
