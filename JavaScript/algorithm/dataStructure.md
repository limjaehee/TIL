# 자료구조

## 단방향 연결 리스트

<img src="../../assets/JavaScript/singly_linked_lists.png" width="400">

단방향 연결 리스트는 다음과 같은 특징을 가지고 있다

1. 인덱스가 없음
2. next 포인터가 있는 노드를 통해 연결됨
3. 임의 인덱스 접근이 허용되지 않음 (순서대로 따라가야 함)

이는 배열과 다른 특징을 가지고 있는데 배열은

1. 인덱스가 있음
2. 삽입 및 삭제 비용이 많이 들 수 있음
3. 특정 인덱스에서 빠르게 접근 가능

<br>

### ✅ push 메소드 구현

```jsx
class Node {
    constructor(val) {
        this.val = val;
        this.next = null;
    }
}

class SinglyLinkedList {
    constructor() {
        this.head = null;
        this.tail = null;
        this.length = 0;
    }
    push(val) {
        let newNode = new Node(val);
        if (!this.head) {
            this.head = newNode;
            this.tail = this.head;
        } else {
            this.tail.next = newNode;
            this.tail = newNode;
        }
        this.length++;
        return this;
    }
}

let list = new SinglyLinkedList();
list.push("HELLO");
list.push("GOODBYE");
list.push("END"); //SinglyLinkedList {head: Node, tail: Node, length: 3}
```

<br>

### ✅ pop 메소드 구현

```jsx
class SinglyLinkedList {
		...
		pop() {
		    if (!this.head) return undefined;
		    let current = this.head;
		    let newTail = current;
		    while (current.next) {
		        newTail = current;
		        current = current.next;
		    }
		    this.tail = newTail;
		    this.tail.next = null;
		    this.length--;
		    if (this.length === 0) {
		        this.head = null;
		        this.tail = null;
		    }
		    return current;
		}
		...
}

list.pop(); //Node {val: 'END', next: null}
```