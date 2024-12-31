



`Stack`、`Deque` 和 `Queue` 是 Java 中常用的三种数据结构。它们在功能、用途和实现上都有所不同。以下是详细的区别：

------

### **1. 定义与特点**

| 数据结构  | 定义                                         | 特点                                         |
| --------- | -------------------------------------------- | -------------------------------------------- |
| **Stack** | 栈，后进先出（LIFO, Last In First Out）。    | 只能在栈顶插入和删除元素。                   |
| **Deque** | 双端队列，可以两端插入和删除元素。           | 可以用作 **栈**（LIFO）或 **队列**（FIFO）。 |
| **Queue** | 队列，先进先出（FIFO, First In First Out）。 | 元素只能从队列尾部插入，从队列头部移除。     |

------

### **2. 用途**

| 数据结构  | 用途                                                         |
| --------- | ------------------------------------------------------------ |
| **Stack** | 用于需要后进先出逻辑的场景，例如括号匹配、递归调用模拟、逆序等。 |
| **Deque** | 通用的数据结构，可以同时作为栈或队列使用。适合双向操作的场景，例如滑动窗口问题等。 |
| **Queue** | 用于先进先出逻辑的场景，例如任务调度、消息队列、广度优先搜索（BFS）等。 |

------

### **3. 常见实现**

| 数据结构  | 常见实现方式                                                 |
| --------- | ------------------------------------------------------------ |
| **Stack** | Java 中的 `Stack` 类（继承自 `Vector`）。现代开发更推荐使用 `Deque` 代替 `Stack`。 |
| **Deque** | 常见实现有 `ArrayDeque` 和 `LinkedList`。                    |
| **Queue** | 常见实现有 `LinkedList` 和 `PriorityQueue`（优先队列）。     |

------

### **4. 方法与操作**

| 数据结构  | 操作                            | 方法示例（以 `Deque` 为例）         |
| --------- | ------------------------------- | ----------------------------------- |
| **Stack** | 压栈（Push）：向栈顶添加元素    | `push(E e)`                         |
|           | 弹栈（Pop）：移除栈顶元素       | `pop()`                             |
|           | 查看栈顶元素（Peek）            | `peek()`                            |
| **Deque** | 入队列头：在队首插入元素        | `addFirst(E e)` / `offerFirst(E e)` |
|           | 入队列尾：在队尾插入元素        | `addLast(E e)` / `offerLast(E e)`   |
|           | 出队列头：移除队首元素          | `removeFirst()` / `pollFirst()`     |
|           | 出队列尾：移除队尾元素          | `removeLast()` / `pollLast()`       |
| **Queue** | 入队（Enqueue）：在队尾插入元素 | `add(E e)` / `offer(E e)`           |
|           | 出队（Dequeue）：移除队首元素   | `remove()` / `poll()`               |
|           | 查看队首元素                    | `element()` / `peek()`              |

------

### **5. 线程安全性**

| 数据结构  | 线程安全性             | 说明                                                         |
| --------- | ---------------------- | ------------------------------------------------------------ |
| **Stack** | 线程安全               | `Stack` 类继承自 `Vector`，其方法是同步的，但同步机制使性能较低。 |
| **Deque** | 非线程安全             | 使用 `ArrayDeque` 和 `LinkedList` 时需要手动同步。若需要线程安全，可以使用 `ConcurrentLinkedDeque`。 |
| **Queue** | 非线程安全（一般实现） | 可以使用线程安全的队列，如 `ConcurrentLinkedQueue` 或 `BlockingQueue`。 |

------

### **6. 性能对比**

| 数据结构  | 性能 | 说明                                                         |
| --------- | ---- | ------------------------------------------------------------ |
| **Stack** | 较低 | 继承自 `Vector`，线程安全导致性能不高。现代开发中建议使用 `ArrayDeque` 替代。 |
| **Deque** | 较高 | 比 `Stack` 性能更高，特别是 `ArrayDeque` 基于数组实现，效率较高。 |
| **Queue** | 较高 | `LinkedList` 的性能适合一般队列操作，`PriorityQueue` 适合需要排序的队列。 |

------

### **7. 使用方式示例**

#### **Stack**

```
java复制代码Stack<Integer> stack = new Stack<>();
stack.push(1);  // 压栈
stack.push(2);
System.out.println(stack.pop());  // 弹栈，输出 2
System.out.println(stack.peek()); // 查看栈顶，输出 1
```

#### **Deque 用作栈**

```
java复制代码Deque<Integer> deque = new ArrayDeque<>();
deque.push(1);  // 压栈
deque.push(2);
System.out.println(deque.pop());  // 弹栈，输出 2
System.out.println(deque.peek()); // 查看栈顶，输出 1
```

#### **Deque 用作队列**

```
java复制代码Deque<Integer> deque = new ArrayDeque<>();
deque.addLast(1);  // 入队
deque.addLast(2);
System.out.println(deque.pollFirst()); // 出队，输出 1
System.out.println(deque.peekFirst()); // 查看队首，输出 2
```

#### **Queue**

```
java复制代码Queue<Integer> queue = new LinkedList<>();
queue.offer(1);  // 入队
queue.offer(2);
System.out.println(queue.poll());  // 出队，输出 1
System.out.println(queue.peek());  // 查看队首，输出 2
```

------

### **8. 总结与推荐**

| 数据结构  | 场景                                     | 推荐使用                               |
| --------- | ---------------------------------------- | -------------------------------------- |
| **Stack** | 后进先出，简单的栈操作场景。             | 推荐使用 `Deque` 替代。                |
| **Deque** | 需要灵活操作（既当栈，又当队列）。       | 使用 `ArrayDeque`，性能优越。          |
| **Queue** | 先进先出场景，如任务队列或广度优先搜索。 | 使用 `LinkedList` 或 `PriorityQueue`。 |

总结来说，`Deque` 是一个更加通用和高效的选择，现代开发中更倾向于用 `ArrayDeque` 替代传统的 `Stack` 或简单的 `Queue`。