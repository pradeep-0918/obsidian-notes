
## Stack

### Definition
Stack is a linear data structure that follows the LIFO principle.

LIFO = Last In First Out
The element inserted last will be removed first.

Example:

Push 1
Push 2
Push 3

| 3 | ← Top
| 2 |
| 1 |

Pop → 3 removed

| 2 |
| 1 |

### Stack Operation

### Core Operations

Push(x)  → insert element
Pop()    → remove element
Peek()   → see top element
isEmpty()
size()
## Example:
push(10)
push(20)
push(30)

`stack = [10,20,30]`

pop()

stack = [10,20].
### Time Complexity

Push  → O(1)
Pop   → O(1)
Peek  → O(1)

### Real Life Examples

1. Browser Back Button
2. Undo / Redo operations
3. Expression evaluation
4. Function Call Stack
5. DFS traversal in graphs
### Browser History
Visit A
Visit B
Visit C

Back → C removed
Back → B removed

####  Stack Implementation in Java
class StackExample {

    int top = -1;
    int arr[] = new int[100];

    void push(int x){
        arr[++top] = x;
    }

    int pop(){
        return arr[top--];
    }

    int peek(){
        return arr[top];
    }
}

## Queue Concept

Queue is a linear data structure that follows FIFO.

FIFO = First In First Out
First inserted element will be removed first.

### insert Operation 

enqueue(10)
enqueue(20)
enqueue(30)

### Queue operation

enqueue(10)
enqueue(20)
enqueue(30)

## Stack Implementations

1. Array Implementation
2. Dynamic Array (ArrayList)
3. Linked List Implementation
4. Java Built-in Stack
5. Deque (Recommended for Interviews)

## Queue Implementations

1. Array Queue
2. Circular Queue
3. Linked List Queue
4. Java Queue Interface
5. Deque Implementation
6. Priority Queue

## Stack 

## Using Arrays Method

```
int[] stack = new int[10];
int top = -1;
```
### Push Operation 

*Remember that push - we want to check stack is full or not* 

```
top++;
stack[top] = x;
```

### Pop Operation

*Remember that pop - we need to check Stack is empty or not*

```
int value = stack[top];
top--;
```

### Peek operation
```
int value = stack[top];
```

## Using Array List Method


*Declaration*- `ArrayList<Integer> stack = new ArrayList<>();`
*push Operaion* - `stack.add(x);`
*Pop operation*-  `stack.remove()stack.size()-1;`
*peek opeartion* - `stack.get(stack.size() - 1);`



