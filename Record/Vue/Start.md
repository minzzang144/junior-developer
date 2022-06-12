# Start

### Vue.js가 무엇인가요?

Vue(/vjuː/ 로 발음, view 와 발음이 같습니다.)는 사용자 인터페이스를 만들기 위한 프로그레시브 프레임워크 입니다. 다른 단일형 프레임워크와 달리 Vue는 점진적으로 채택할 수 있도록 설계하였습니다.

### 선언적 렌더링(Declarative Rendering)

Vue.js의 핵심에는 간단한 템플릿 구문을 사용하여 DOM에서 데이터를 선언적으로 렌더링 할 수있는 시스템이 있습니다.

```html
<div id="counter">Counter: {{ counter }}</div>
```

```javascript
const Counter = {
  data() {
    return {
      counter: 0,
    };
  },
};

Vue.createApp(Counter).mount("#counter");
```

첫 Vue app을 만들었습니다! 이것은 문자열 템플릿을 렌더링하는 것과 매우 유사하지만, Vue.JS 내부에서는 더 많은 작업을 하고 있습니다. 이제 데이터와 DOM이 연결되었으며 모든 것이 반응형(reactive) 이 되었습니다. 우리는 그것을 어떻게 확인할 수 있을까요? 아래 예제 코드에서 counter 속성은 매 초마다 증가하고 우리는 DOM이 변경될때 어떻게 렌더 되는지 볼 수 있습니다.

```javascript
const CounterApp = {
  data() {
    return {
      counter: 0,
    };
  },
  mounted() {
    setInterval(() => {
      this.counter++;
    }, 1000);
  },
};
```

텍스트 보간 이외에도 다음과 같은 엘리먼트 속성을 바인딩할 수 있습니다.

```html
<div id="bind-attribute">
  <span v-bind:title="message">
    여기에 마우스를 올려두고 잠시 기다리면 제목이 동적으로 바뀝니다!
  </span>
</div>
```

```javascript
const AttributeBinding = {
  data() {
    return {
      message:
        "이 페이지를 다음 시간에 열었습니다. " + new Date().toLocaleString(),
    };
  },
};

Vue.createApp(AttributeBinding).mount("#bind-attribute");
```

여기서 우리는 새로운 것을 만났습니다. v-bind 속성은 `디렉티브` 이라고 합니다. **디렉티브는 Vue에서 제공하는 특수 속성임을 나타내는 `v- 접두어`가 붙어있으며 사용자가 짐작할 수 있듯 렌더링 된 DOM에 특수한 반응형 동작을 합니다**. 기본적으로 “이 요소의 title 속성을 Vue 인스턴스의 message 프로퍼티로 최신 상태를 유지 합니다.”

### 사용자 입력 핸들링

사용자가 앱과 상호 작용할 수 있게 하기 위해 우리는 `v-on` 디렉티브를 사용하여 Vue 인스턴스에서 메소드를 호출하는 이벤트 리스너를 추가 할 수 있습니다.

```html
<div id="event-handling">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```

```javascript
const EventHandling = {
  data() {
    return {
      message: "Hello Vue.js!",
    };
  },
  methods: {
    reverseMessage() {
      this.message = this.message.split("").reverse().join("");
    },
  },
};

Vue.createApp(EventHandling).mount("#event-handling");
```

Vue는 또한 양식에 대한 입력과 앱 상태를 양방향으로 바인딩하는 `v-model` 디렉티브를 제공합니다.

```html
<div id="two-way-binding">
  <p>{{ message }}</p>
  <input v-model="message" />
</div>
```

```javascript
const TwoWayBinding = {
  data() {
    return {
      message: "Hello Vue!",
    };
  },
};

Vue.createApp(TwoWayBinding).mount("#two-way-binding");
```

### 조건문과 반복문

엘리먼트가 표시되는지에 대한 여부를 제어하는 것은 아주 간단합니다.

```html
<div id="conditional-rendering">
  <span v-if="seen">이제 나를 볼수 있어요</span>
</div>
```

```javascript
const ConditionalRendering = {
  data() {
    return {
      seen: true,
    };
  },
};

Vue.createApp(ConditionalRendering).mount("#conditional-rendering");
```

Vue에서 제공하는 특별한 기능을 제공하는 디렉티브들이 있습니다. 예를 들어 v-for 디렉티브는 배열에서 데이터를 가져와 아이템 목록을 표시하는데 사용할수 있습니다.

```html
<div id="list-rendering">
  <ol>
    <li v-for="todo in todos" :key="todo.id">{{ todo.text }}</li>
  </ol>
</div>
```

```javascript
const ListRendering = {
  data() {
    return {
      todos: [
        { text: "Learn JavaScript" },
        { text: "Learn Vue" },
        { text: "Build something awesome" },
      ],
    };
  },
};

Vue.createApp(ListRendering).mount("#list-rendering");
```

**이 글을 [Vue.js](https://v3.ko.vuejs.org/) 공식문서를 참고하여 작성되었습니다.**
