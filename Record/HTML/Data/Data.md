# Data 속성

### 정의

JavaScript 프레임워크가 인기있기 전에, 프론트엔드 개발자는 비표준 속성, DOM 추가 프로퍼티 등의 조작없이, DOM 자체에 추가적인 데이터를 저장하기 위해 data-속성을 사용했었습니다. 이는 적절한 속성이나 요소가 없는 페이지나 애플리케이션에 사용자정의 데이터를 비공개로 저장하기 위한 것입니다.

### 사용법

문법은 간단합니다. 어느 엘리멘트에나 data-로 시작하는 속성은 무엇이든 사용할 수 있습니다. 화면에 안 보이게 글이나 추가 정보를 엘리멘트에 담아 놓을 수 있어요.

```
<article
  id="electriccars"
  data-columns="3"
  data-index-number="12314"
  data-parent="cars">
...
</article>
```

JavaScript 에서 이 속성 값들을 읽는 방법은 너무 간단합니다. dataset 객체를 통해 data 속성을 가져오기 위해서는 속성 이름의 data- 뒷 부분을 사용합니다.

```
var article = document.getElementById('electriccars');

article.dataset.columns // "3"
article.dataset.indexNumber // "12314"
article.dataset.parent // "cars"
```

데이터 속성은 순 HTML 속성이기 때문에 CSS에서도 접근할 수 있다는 것에 주목하세요.

```
article::before {
  content: attr(data-parent);
}
```

```
article[data-columns='3'] {
  width: 400px;
}
article[data-columns='4'] {
  width: 600px;
}
```

요즘에는, data-속성을 사용하는 것을 권장하지 않습니다. 그 이유 중 하나는 사용자가 브라우저의 inspect 기능를 사용하여 데이터 속성을 쉽게 수정할 수 있다는 것입니다. 데이터 모델은 JavaScript 자체에 더 잘 저장되며, 라이브러리나 프레임워크의 데이터 바인딩을 통해 DOM을 업데이트된 상태로 유지하는 것이 더 낫습니다.
