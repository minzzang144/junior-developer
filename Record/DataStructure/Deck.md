# 덱(Deck)

덱(Deck)은 단방향으로만 요소를 제어할 수 있는 스택과 달리 앞과 뒤 모든 방향에서 요소를 제어할 수 있다.<br>

즉, 앞과 뒤 어느쪽에서도 삽입과 삭제가 가능하며 이는 사실상 스택과 큐의 연산을 합쳐놓은 것이라고도 할 수 있다.<br>

## 메서드

- push_front(덱의 앞에 자료를 넣는 연산)
- push_back(덱의 뒤에 자료를 넣는 연산)
- pop_front(덱의 앞에서 자료를 빼는 연산)
- pop_back(덱의 뒤에서 자료를 빼는 연산)

또한 넣고 뺀 후의 해당 값의 위치를 기억해야 한다.<br>

- front(덱의 가장 앞에 있는 자료를 보는 연산)
- back(덱의 가장 뒤에 있는 자료를 보는 연산)

## 큐 구현

```
class Deque {
  constructor() {
    this.data = [];
    this.rear = 0;
  }

  push_front(element) {
    this.data.unshift(element);
    this.rear = this.rear + 1;
  }

  push_back(element) {
    this.data[this.rear] = element;
    this.rear = this.rear + 1;
  }

  pop_front() {
    if (this.isEmpty() === false) {
      this.rear = this.rear - 1;
      return this.data.shift();
    }

    return false;
  }

  pop_back() {
    if (this.isEmpty() === false) {
      this.rear = this.rear - 1;
      return this.data.pop();
    }

    return false;
  }

  length() {
    return this.rear;
  }

  isEmpty() {
    return this.rear === 0;
  }

  getFront() {
    if (this.isEmpty() === false) {
      return this.data[0];
    }

    return false;
  }

  getLast() {
    if (this.isEmpty() === false) {
      return this.data[this.rear - 1];
    }

    return false;
  }

  print() {
    for (let i = 0; i < this.rear; i++) {
      console.log(this.data[i]);
    }
  }
}
```
