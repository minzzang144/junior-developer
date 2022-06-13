# 폼 입력 바인딩

### 기본 사용법

v-model 디렉티브를 사용하여 input, textarea, select 요소들에 양방향 데이터 바인딩을 생성할 수 있습니다. v-model 디렉티브는 input type 요소를 변경하는 올바른 방법을 자동으로 선택합니다. 약간 이상하지만, v-model은 기본적으로 사용자 입력 이벤트에 대한 데이터를 업데이트하는 “syntax sugar”이며, 일부 경우에는 특별한 주의를 해야합니다.

```
주의

v-model은 모든 form 엘리먼트의 초기 value와 checked 그리고 selected 속성을 무시합니다. 항상 Vue 인스턴스 데이터를 원본 소스로 취급합니다. 컴포넌트의 data 옵션 안에 있는 JavaScript에서 초기값을 선언해야합니다.
```

v-model은 내부적으로 서로 다른 속성을 사용하고 서로 다른 입력 요소에 대해 서로 다른 이벤트를 전송합니다.

- text 와 textarea 태그는 value 속성과 input 이벤트를 사용합니다.
- 체크박스들과 라디오 버튼들은 checked 속성과 change 이벤트를 사용합니다.
- Select 태그는 value 를 prop으로, change를 이벤트로 사용합니다.

### 수식어

- .lazy

  기본적으로, v-model은 각 input 이벤트 후 입력과 데이터를 동기화 합니다. (단, 앞에서 설명한 IME 구성은 제외됩니다.). lazy 수식어를 추가하여 change이벤트 이후에 동기화 할 수 있습니다.

  ```html
  <!-- "input" 대신 "change" 이후에 동기화 됩니다. -->
  <input v-model.lazy="msg" />
  ```

- .number

  .number
  사용자 입력이 자동으로 숫자로 형변환 되기를 원하면, v-model이 관리하는 input에 number 수식어를 추가하면 됩니다.

  ```html
  <input v-model.number="age" type="number" />
  ```

  type="number"를 사용하는 경우에 HTML 입력 요소의 값은 항상 문자열을 반영하기 때문에 종종 유용합니다. 만약, 값이 parseFloat()에 의해서 분석할 수 없는 경우, 원래의 값이 리턴됩니다.

-.trim

사용자가 입력한 내용에서 자동으로 앞뒤 공백을 제거하는 트림처리가 되길 바란다면, v-model이 관리하는 input에 trim 수식어를 추가하면 됩니다.

```html
<input v-model.trim="msg" />
```
