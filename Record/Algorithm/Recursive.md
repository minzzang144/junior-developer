# Recursive Function(재귀 함수)

## 정의

- 재귀 함수(Recursive Function)란 자기 자신을 호출하는 함수를 의미합니다.

### 예시

- 재귀 함수를 문제 풀이에서 사용할 때는 재귀 함수의 `종료 조건`을 반드시 명시해야 합니다.
- 종료 조건을 제대로 명시하지 않으면 **함수가 무한히 호출**될 수 있습니다.

```js
function recursiveFunc(i) {
  if (i === 100) return;
  console.log(`${i}번째 재귀함수에서 ${i + 1}번째 재귀함수를 호출합니다.`);
  recursiveFunc(i + 1);
}

recursiveFunc(1);
```

#### 팩토리얼

- 팩토리얼은 1부터 n 사이의 모든 수를 곱한 값을 말합니다.
- 주의해야 할 점은 `0!`과 `1!`의 값은 1입니다.

```js
function factorialIterative(n) {
  let result = 1;
  for (let i = 1; i <= n; i++) {
    result *= i;
  }
  return result;
}

function factorialRecursive(n) {
  if (n <= 1) return 1;
  return n * factorialRecursive(n - 1);
}
```

두 함수의 결과값은 동일하다.

#### 최대공약수

- 유클리드 호제법에 의해 다음과 같이 해결할 수 있습니다.
  - 두 자연수 A, B에 대하여(A > B) A를 B로 나눈 나머지를 R이라고 합시다.
  - 이때 A와 B의 최대공약수는 B와 R의 최대공약수와 같습니다.
  - 재귀함수를 사용해서 나머지가 0이 나올때까지 호출하고 0이 나오면 그 값이 최대공약수이다.

```js
function gcd(a, b) {
  if (a % b === 0) return b;
  else gcd(b, a % b);
}

console.log(gcd(192, 162));
```
