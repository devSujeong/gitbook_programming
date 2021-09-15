# Queue

## Queue란?

가장 먼저 넣은 데이터를 가장 먼저 꺼낼 수 있는 구조  
FIFO\(First-In, First-Out\) 또는 LILO\(Last-In, Last-Out\) 방식으로 스택과 꺼내는 순서가 반대  
[https://visualgo.net/en/list](https://visualgo.net/en/list)

* Enqueue: 큐에 데이터를 넣는 기능
* Dequeue: 큐에서 데이터를 꺼내는 기능

## 장단점

딱히 없음

## 활용

멀티 태스킹을 위한 프로세스 스케쥴링 방식을 구현하기 위해 많이 사용됨 \(운영체제 참조\)

## 구현

```javascript
let list = [1, 2, 3];

function enqueue(data) {
    list.push(data);
}

function dequeue() {
    return list.shift();
}
```

