# Slots

### Slot 컨텐츠

slot을 사용하면 다음과 같이 컨텐츠를 구성할 수 있습니다.

```html
<todo-button> Add todo </todo-button>

<!-- todo-button 컴포넌트 템플릿 -->
<button class="btn-primary">
  <slot></slot>
</button>

<!-- HTML 렌더링 -->
<button class="btn-primary">Add todo</button>
```

문자열은 시작일 뿐입니다! 슬롯은 HTML을 포함한 어떠한 템플릿 코드라도 포함할 수 있습니다.

```html
<todo-button>
  <!-- 폰트어썸 아이콘 추가 -->
  <i class="fas fa-plus"></i>
  Add todo
</todo-button>

<!-- 심지어는 컴포넌트를 포함시킬 수도 있습니다. -->
<todo-button>
  <!-- 아이콘을 추가하기 위해 컴포넌트를 사용 -->
  <font-awesome-icon name="plus"></font-awesome-icon>
  Add todo
</todo-button>
```

만약 todo-button 컴포넌트의 템플릿이 slot 코드를 가지고 있지 않다면, 태그 내부에 위치한 모든 컨텐츠는 무시됩니다.

```html
<todo-button>
  <!-- 아래의 텍스트는 렌더링되지 않습니다 -->
  Add todo
</todo-button>

<!-- todo-button 컴포넌트 템플릿 -->

<button class="btn-primary">Create a new item</button>
```

### 렌더 스코프

slot 내부에서 데이터를 사용할 수도 있습니다.

```html
<todo-button> Delete a {{ item.name }} </todo-button>
```

해당 slot은 나머지 템플릿과 같은 인스턴스의 속성과 같은 접근 권한을 갖습니다. (즉, 같은 "스코프"를 갖습니다.)

반대로 아래 이 slot은 todo-button 의 스코프에 접근할 수 없습니다. 예를 들어, 아래와 같이 action에 접근하고자 하는 경우, 코드가 정상적으로 동작하지 않습니다.

```html
<todo-button action="delete">
  Clicking here will {{ action }} an item
  <!--
  The `action` will be undefined, because this content is passed
  _to_ <todo-button>, rather than defined _inside_ the
  <todo-button> component.
  -->
</todo-button>
```

아래와 같은 규칙을 기억하세요.

> 부모 템플릿에 있는 모든 요소는 부모 스코프에서 컴파일되고, 자식 템플릿에 있는 모든 요소는 자식 스코프에서 컴파일됩니다.

### Fallback 컨텐츠

Slot에 컨텐츠가 주어지지 않는 경우, (default 같은) fallback을 특정하는 것이 유용한 경우가 있습니다.

예를 들어, submit-button 라는 컴포넌트가 있을 때

```html
<button type="submit">
  <slot></slot>
</button>
```

일반적으로 "Submit"이라는 텍스트가 button 내부에 있기를 원할 수 있습니다. "Submit" 텍스트를 fallback 컨텐츠로 만들기 위해서 slot 태그 내부에 텍스트를 위치할 수 있습니다.

```html
<button type="submit">
  <slot>Submit</slot>
</button>
```

이제 submit-button 내부의 slot에 아무런 컨텐츠도 제공하지 않는 경우에

```html
<submit-button></submit-button>
```

아래와 같이 fallback 컨텐츠로써 "Submit"을 렌더합니다.

```html
<button type="submit">Submit</button>
```

### 이름을 갖는 Slot

여러 개의 슬롯을 사용하는 것이 유용한 경우가 있습니다. 예를 들어, 아래와 같은 템플릿을 가진 base-layout 컴포넌트가 있다고 해 봅시다.

```html
<div class="container">
  <header>
    <!-- header 컨텐츠를 두고 싶은 곳 -->
  </header>
  <main>
    <!-- main 컨텐츠를 두고 싶은 곳 -->
  </main>
  <footer>
    <!-- footer 컨텐츠를 두고 싶은 곳 -->
  </footer>
</div>
```

이 경우, slot 엘리먼트가 특별한 attribute인 name을 갖도록 할 수 있습니다. 이는 각 slot의 특정한 ID로써, 각각의 컨텐츠가 원하는 곳에 렌더링되도록 하는데 사용될 수 있습니다.

```html
<div class="container">
  <header>
    <slot name="header">
  </header>
  <main>
    <slot>
  </main>
  <footer>
    <slot name="footer">
  </footer>
</div>
```

name 을 갖지 않는 slot 은 묵시적으로 `"default"`라는 name을 갖습니다.

이름을 가진 slot에 컨텐츠를 제공하기 위해서는 `<template>` 엘리먼트에 v-slot 지시자를 사용합니다.
v-slot의 매개변수로는 각 slot의 이름을 전달합니다.

```html
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <template v-slot:default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

렌더링된 HTML은 다음과 같습니다.

```html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

### 스코프를 갖는 Slot

간혹 슬롯 컨텐츠가 자식 컴포넌트에서만 접근할 수 있는 데이터에 접근하는 것이 유용한 경우가 있습니다. 이러한 경우는 컴포넌트가 array의 요소들을 렌더링하고 각 요소들이 렌더링되는 방식을 커스터마이징 하는 경우에 주로 발생합니다.

예를 들어, todo-item들을 갖는 컴포넌트를 생각해 봅시다.

```javascript
app.component("todo-list", {
  data() {
    return {
      items: ["Feed a cat", "Buy milk"],
    };
  },
  template: `
    <ul>
      <li v-for="(item, index) in items">
        {{ item }}
      </li>
    </ul>
  `,
});
```

이 때, 부모 컴포넌트에서 slot을 변경하길 원할 수 있습니다.

```html
<todo-list>
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>
```

하지만 todo-list 컴포넌트만이item 요소에 대한 접근이 가능하며 부모요소에는 slot만을 제공하므로 위 코드는 정상적으로 동작하지 않습니다.

이 경우, item을 부모 요소로부터 제공받는 slot 컨텐츠에 사용 가능하도록 하기 위해서 slot 요소에 매개변수로써 bind 시킬 수 있습니다.

```html
<ul>
  <li v-for="( item, index ) in items">
    <slot :item="item"></slot>
  </li>
</ul>
```

slot 엘리먼트에 연결된 매개변수를 `slot props`라고 부릅니다. 이제 부모 스코프에서 v-slot를 정의한 이름을 이용해 사용할 수 있습니다.

```html
<todo-list>
  <template v-slot:default="slotProps">
    <i class="fas fa-check"></i>
    <span class="green">{{ slotProps.item }}</span>
  </template>
</todo-list>
```

**이 글을 [Vue.js](https://v3.ko.vuejs.org/) 공식문서를 참고하여 작성되었습니다.**
