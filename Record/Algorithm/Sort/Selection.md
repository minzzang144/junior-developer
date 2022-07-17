# Selection Sort(선택 정렬)

## 정의

- 정렬이란 데이터를 특정한 기준에 따라 순서대로 나열하는 것을 말합니다.
- 그 중에서도 선택 정렬은 처리되지 않은 데이터 중에서 가장 작은 데이터를 선택해 맨 앞에 있는 데이터와 바꾸는 것을 반복합니다.

### 예시

```js
const arr = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8];

function selectionSort(array) {
  for (let i = 0; i < array.length; i++) {
    let minIndex = i;
    for (let j = i + 1; j < array.length; j++) {
      if (array[minIndex] > array[j]) {
        minIndex = j;
      }
    }
    let temp = array[i];
    array[i] = array[minIndex];
    array[minIndex] = temp;
  }
}

console.log(selectionSort(arr));
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

맨 앞 인덱스부터 시작해 그 앞의 인덱스와 비교했을 때 더 작은 숫자를 발견하면 자리를 바꿉니다.
