# Design Circular Queue

Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Implementation the  `MyCircularQueue`  class:

-   `MyCircularQueue(k)`  Initializes the object with the size of the queue to be  `k`.
-   `int Front()`  Gets the front item from the queue. If the queue is empty, return  `-1`.
-   `int Rear()`  Gets the last item from the queue. If the queue is empty, return  `-1`.
-   `boolean enQueue(int value)`  Inserts an element into the circular queue. Return  `true`  if the operation is successful.
-   `boolean deQueue()`  Deletes an element from the circular queue. Return  `true`  if the operation is successful.
-   `boolean isEmpty()`  Checks whether the circular queue is empty or not.
-   `boolean isFull()`  Checks whether the circular queue is full or not.

**Example 1:**

    Input
    ["MyCircularQueue", "enQueue", "enQueue", "enQueue", "enQueue", "Rear", "isFull", "deQueue", "enQueue", "Rear"]
    [[3], [1], [2], [3], [4], [], [], [], [4], []]
    Output
    [null, true, true, true, false, 3, true, true, true, 4]

    Explanation
    MyCircularQueue myCircularQueue = new MyCircularQueue(3);
    myCircularQueue.enQueue(1); // return True
    myCircularQueue.enQueue(2); // return True
    myCircularQueue.enQueue(3); // return True
    myCircularQueue.enQueue(4); // return False
    myCircularQueue.Rear();     // return 3
    myCircularQueue.isFull();   // return True
    myCircularQueue.deQueue();  // return True
    myCircularQueue.enQueue(4); // return True
    myCircularQueue.Rear();     // return 4

**Constraints:**

-   `1 <= k <= 1000`
-   `0 <= value <= 1000`
-   At most  `3000`  calls will be made to `enQueue`,  `deQueue`, `Front`, `Rear`, `isEmpty`, and `isFull`.

**Follow up:** Could you solve the problem without using the built-in queue?

---

**Solutions:**

At first, I thought if I would use Array to represent the Queue, I would need to shift the entire Array on Dequeue operation, which has O(N) time complexity. As this is costly, I chose DoublyLinkedList node to represent items in the Queue

```go
type DLNode struct {
    Val int
    Prev,Next *DLNode
}

type MyCircularQueue struct {
    front,back *DLNode
    size int
    count int
}


func Constructor(k int) MyCircularQueue {
    return MyCircularQueue{size: k, count: 0}
}


func (this *MyCircularQueue) EnQueue(value int) bool {
    if this.IsFull { return false }
    
    node := &DLNode{Val: value}
    if this.front == nil {
        this.front = node // this.back is this.front.Next = nil
    }else {
        node.Prev = this.back
        this.back.Next = node
        this.back = node
    }
    return true
}


func (this *MyCircularQueue) DeQueue() bool {
    if this.IsEmpty() { return false }
    
    node := this.back
    this.back = this.back.prev
    return true
}


func (this *MyCircularQueue) Front() int {
    if this.front == nil { return -1 }
    return this.front
}


func (this *MyCircularQueue) Rear() int {
    if this.back == nil { return -1 }
    return this.back
}


func (this *MyCircularQueue) IsEmpty() bool {
    return this.count == 0
}


func (this *MyCircularQueue) IsFull() bool {
    return this.count == this.size
}


/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * obj := Constructor(k);
 * param_1 := obj.EnQueue(value);
 * param_2 := obj.DeQueue();
 * param_3 := obj.Front();
 * param_4 := obj.Rear();
 * param_5 := obj.IsEmpty();
 * param_6 := obj.IsFull();
 */
```

Turn out that if I use mod operation on the indexes of the Array, I don't need to move the item around. I only need to move the indexes of front and back around

```go
type MyCircularQueue struct {
    capacity,size int
    frontIdx,backIdx int
    queue []int
}


func Constructor(k int) MyCircularQueue {
    return MyCircularQueue{
        capacity: k,
        size: 0,
        frontIdx: 0,
        backIdx: 0,
        queue: make([]int,k),
    }
}


func (this *MyCircularQueue) EnQueue(value int) bool {
    if this.IsFull() { return false }
    
    this.queue[this.backIdx] = value
    this.size += 1
    this.backIdx += 1
    this.backIdx %= this.capacity
    
    return true
}


func (this *MyCircularQueue) DeQueue() bool {
    if this.IsEmpty() { return false }
    
    this.size -= 1
    this.frontIdx += 1
    this.frontIdx %= this.capacity
    
    return true
}


func (this *MyCircularQueue) Front() int {
    if this.IsEmpty() { return -1 }
    return this.queue[this.frontIdx]
}


func (this *MyCircularQueue) Rear() int {
    if this.IsEmpty() { return -1 }
    if this.backIdx == 0 { return this.queue[this.capacity-1]}
    return this.queue[this.backIdx-1]
}


func (this *MyCircularQueue) IsEmpty() bool {
    return this.size == 0
}


func (this *MyCircularQueue) IsFull() bool {
    return this.size == this.capacity
}


/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * obj := Constructor(k);
 * param_1 := obj.EnQueue(value);
 * param_2 := obj.DeQueue();
 * param_3 := obj.Front();
 * param_4 := obj.Rear();
 * param_5 := obj.IsEmpty();
 * param_6 := obj.IsFull();
 */
```