`ArrayDeque` 和 `LinkedList` 都是 Java 中常用的双端队列实现类，它们有一些相似之处，但底层实现和性能表现不同。

### 1. 底层实现

- **ArrayDeque**：基于 **动态数组** 实现，因此其内部存储结构是一个数组。数组满时会自动扩容，但需要进行元素拷贝，因此扩容开销较大。
- **LinkedList**：基于 **双向链表** 实现。元素在链表中是通过节点连接的，因此插入和删除的开销较低，但每个节点额外存储了前后指针，占用更多的内存。



虽然数组的形式快，但是当创建的时候有时候需要很大的空间，有很大的内存消耗，以后写队列的时候可以用ArrayDeque的实现类

详细代码说明见二叉树文件夹中的513题