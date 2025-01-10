

**关于比较器的知识，在一些常见用法的比较器文档中有讲解**



堆里面存整数，并按照字典序排序



```java
// 小根堆，按字典序排序数字
PriorityQueue<Integer> heap = new PriorityQueue<>((a, b) -> a - b);
// 或者直接使用默认构造函数
PriorityQueue<Integer> heap = new PriorityQueue<>();


// 大根堆，按字典序从大到小排序数字
PriorityQueue<Integer> heap = new PriorityQueue<>((a, b) -> b - a);
```







堆里面存字符，并按照字典序排序



```java
// 小根堆，按字典序排序字符
PriorityQueue<Character> heap = new PriorityQueue<>((a, b) -> a - b);

// 或者直接使用默认构造函数，因为 Character 的自然排序是字典序
PriorityQueue<Character> heap = new PriorityQueue<>();





// 大根堆，按字典序从大到小排序字符
PriorityQueue<Character> heap = new PriorityQueue<>((a, b) -> b - a);

```







堆里面存字符串，并按照字典序排序

```java
// 小根堆，按字典序排序字符串
PriorityQueue<String> heap = new PriorityQueue<>((a, b) -> a.compareTo(b));

// 或者直接使用默认构造函数，因为 String 的自然排序是字典序
PriorityQueue<String> heap = new PriorityQueue<>();




// 大根堆，按字典序从大到小排序字符串
PriorityQueue<String> heap = new PriorityQueue<>((a, b) -> b.compareTo(a));

```

