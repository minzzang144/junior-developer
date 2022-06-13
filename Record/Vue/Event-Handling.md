# 이벤트 핸들링

### 이벤트 청취

v-on디렉티브는 @기호로, DOM 이벤트를 듣고 트리거 될 때와 JavaScript를 실행할 때 사용합니다.
`v-on:click="methodName"` 나 줄여서 `@click="methodName"`으로 사용합니다.

### 메소드 이벤트 핸들러

많은 이벤트 핸들러의 로직은 더 복잡할 것이므로, JavaScript를 v-on 속성 값으로 보관하는 것은 간단하지 않습니다. 그게 v-on이 호출하고자 하는 메소드의 이름을 받는 이유입니다.

```html
<div id="event-with-method">
  <!-- `greet`는 메소드 이름으로 아래에 정의되어 있습니다 -->
  <button @click="greet">Greet</button>
</div>
```

```javascript
Vue.createApp({
  data() {
    return {
      name: "Vue.js",
    };
  },
  methods: {
    greet(event) {
      // 메소드 안에서 사용하는 `this` 는 Vue 인스턴스를 가리킵니다.
      alert("Hello " + this.name + "!");
      // `event` 는 네이티브 DOM 이벤트입니다
      if (event) {
        alert(event.target.tagName);
      }
    },
  },
}).mount("#event-with-method");
```

### 인라인 메소드 핸들러

메소드 이름을 직접 바인딩 하는 대신에, 인라인 JavaScript 구문에 메소드를 사용할 수도 있습니다.

```html
<div id="inline-handler">
  <button @click="say('hi')">Say hi</button>
  <button @click="say('what')">Say what</button>
</div>
```

```javascript
Vue.createApp({
  methods: {
    say(message) {
      alert(message);
    },
  },
}).mount("#inline-handler");
```

때로 인라인 명령문 핸들러에서 원본 DOM 이벤트에 액세스 해야할 수도 있습니다. 특별한 `$event`를 사용해 메소드에 전달할 수 있습니다.

```html
<button @click="warn('Form cannot be submitted yet.', $event)">Submit</button>
```

```javascript
// ...
methods: {
  warn(message, event) {
    // 네이티브 이벤트에 접근 할 수 있습니다.
    if (event) {
      event.preventDefault()
    }
    alert(message)
  }
}
```

### 복합 이벤트 핸들러

산자를 사용하여 이벤트 핸들러 안에서 복합 메소드를 지정할 수 있습니다.

```html
<!-- one()과 two() 둘다 버튼 클릭 이벤트를 실행할 수 있습니다.-->
<button @click="one($event), two($event)">Submit</button>
```

```javascript
// ...
methods: {
  one(event) {
    // 첫번째 핸들러 로직...
  },
  two(event) {
    // 두번째 핸들러 로직...
  }
}
```

### 이벤트 수식어

이벤트 핸들러 내부에서 event.preventDefault() 또는 event.stopPropagation()를 호출하는 것은 매우 보편적인 일입니다. 메소드 내에서 쉽게 이 작업을 할 수 있지만, DOM 이벤트 세부 사항을 처리하는 대신 데이터 로직에 대한 메소드만 사용할 수 있으면 더 좋습니다.

이 문제를 해결하기 위하여, Vue는 v-on이벤트에 이벤트 수식어를 제공합니다. 수식어는 점으로 된 접미사 입니다.

- .stop
- .prevent
- .capture
- .self
- .once
- .passive

관련 코드가 동일한 순서로 생성되므로 수식어를 사용할 때 순서를 지정하세요. 다시 말하여 v-on:click.prevent.self를 사용하면 모든 클릭을 막을 수 있으며 v-on:click.self.prevent는 엘리먼트 자체에 대한 클릭만 방지합니다.

### 왜 HTML로 된 리스너를 사용하는가?

이 모든 이벤트 청취 접근 방법이 관심사의 분리(“separation of concerns”)에 대한 오래된 규칙을 어긴다고 생각할 수 있습니다. 모든 뷰 핸들러 함수와 표현식은 현재 뷰 처리 하는 ViewModel에 엄격히 바인딩 되기 때문에 유지보수가 어렵지 않습니다. 실제로 v-on이나 @를 사용하면 몇가지 이점이 있습니다:

- HTML 템플릿을 간단히 하여 JavaScript 코드 내에서 핸들러 함수 구현을 찾는 것이 더 쉽습니다.

- JavaScript에서 이벤트 리스너를 수동으로 연결할 필요가 없으므로 ViewModel 코드는 순수 로직과 DOM이 필요하지 않습니다. 이렇게 하면 테스트가 쉬워집니다.

- ViewModel이 파기되면 모든 이벤트 리스너가 자동으로 제거 됩니다. 이벤트 제거에 대한 걱정이 필요없어집니다.
