# 큐(Queue)

**FIFO(First In First Out, 선입선출)의 원리를 따르는 선형 자료구조의 일종이다.**<br>

즉, 스택과는 반대로 먼저 들어간 요소가 먼저 나오게 되는 구조이다.<br>

큐는 순서대로 처리해야 하는 작업을 임시로 저장해두는 버퍼로서 사용된다. 아직 배우지 못했지만 **BFS** 상황에서 사용한다고 한다.<br><br>

큐의 가장 맨 앞에 있는 요소를 Front, 끝에 있는 요소를 Rear라고 부른다.<br>

큐는 들어올 때 Rear로 들어오며 나갈 때는 Front로 빠지는 특성을 가진다.<br>

## 메서드

- enQueue(데이터를 넣음)
- deQueue(데이터를 뺌)
- isEmpty(비어있는지 확인)
- isFront(꽉차있는지 확인)

또한 넣고 뺀 후의 해당 값의 위치를 기억해야 한다.<br>

- front(deQueue할 위치 기억)
- rear(enQueue할 위치 기억)

## 큐 구현

```
class Queue {
  constructor() {
    this._arr = [];
  }

  enqueue(item) {
    this._arr.push(item);
  }

  dequeue() {
    return this._arr.shift();
  }

  front() {
    return this._arr[0]
  }

  rear() {
    return this._arr[this._arr.length - 1]
  }

  isEmpty() {
     if (this._arr.length === 0) {
        return true;
    } else {
        return false;
    }
  }

  toString() {
    var str = "";
    for (var i = 0;i < this._arr.length; ++i )    {
        str += this._arr[i] + "\n";
    }
    return str;
  }

  size() {
    return this._arr.length
  }
}

const queue = new Queue();
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);
queue.dequeue(); // 1
```
