# Quick Sort(퀵 정렬)

## 정의

- 기준 데이터를 설정하고 그 기준보다 큰 데이터와 작은 데이터의 위치를 바꾸는 방법입니다.
- 일반적인 상황에서 가장 많이 사용되는 정렬 알고리즘 중 하나 입니다.
- 병합 정렬과 더불어 대부분의 프로그래밍 언어의 정렬 라이브러리의 근간이 되는 알고리즘입니다.
- 가장 기본적인 퀵 정렬은 첫 번째 데이터를 기준 데이터(Pivot)로 설정합니다.

### 예시

```js
const arr = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8];

function quickSort(array, start, end) {
  if (start >= end) return; // 원소가 1개인 경우 종료
  let pivot = start;
  let left = start + 1; // 피벗은 첫번째 원소
  let right = end;
  while (left <= right) {
    // 피벗보다 큰 데이터를 찾을 때까지 반복
    while (left <= end && array[left] <= array[pivot]) {
      left += 1;
    }
    // 피벗보다 작은 데이터를 찾을 때까지 반복
    while (right <= start && array[right] >= array[pivot]) {
      left += 1;
    }
    if (left > right) {
      // 엇갈렸다면 작은 데이터와 피벗을 교체
      let temp = array[right];
      array[right] = array[pivot];
      array[pivot] = array[right];
    } else {
      //엇갈리지 않았다면 작은 데이터와 큰 데이터를 교체
      let temp = array[left];
      array[left] = array[right];
      array[right] = array[left];
    }
  }
  // 분할 이후 왼쪽 부분과 오른쪽 부분에서 각각 정렬 수행
  quickSort(array, start, right - 1);
  quickSort(array, right + 1, end);
}

console.log(quickSort(arr));
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

- 이상적인 경우 분할이 절반씩 일어난다면 전체 연산 횟수로 빅오 노테이션 (NlogN) 을 기대할 수 있습니다.
- 퀵 정렬은 평균의 경우 빅오 노테이션 (NlogN)의 시간 복잡도를 가집니다.
- 하지만 최악의 경우 빅오 노테이션 (N 제곱)의 시간 복잡도를 가집니다.
