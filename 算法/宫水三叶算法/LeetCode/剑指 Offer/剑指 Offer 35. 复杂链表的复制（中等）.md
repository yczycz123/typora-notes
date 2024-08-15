### 题目描述

这是 LeetCode 上的 **[剑指 Offer 35. 复杂链表的复制](https://leetcode.cn/problems/fu-za-lian-biao-de-fu-zhi-lcof/solution/by-ac_oier-6atv/)** ，难度为 **中等**。

Tag : 「哈希表」、「链表」



给你一个长度为 `n` 的链表，每个节点包含一个额外增加的随机指针 `random`，该指针可以指向链表中的任何节点或空节点。

构造这个链表的 深拷贝。 深拷贝应该正好由 `n` 个 全新 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 `next` 指针和 `random` 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。复制链表中的指针都不应指向原链表中的节点 。

例如，如果原链表中有 `X` 和 `Y` 两个节点，其中 `X.random --> Y` 。那么在复制链表中对应的两个节点 `x` 和 `y` ，同样有 `x.random --> y` 。

返回复制链表的头节点。

用一个由 `n` 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 `[val, random_index]` 表示：

* `val`：一个表示 `Node.val` 的整数。
* `random_index`：随机指针指向的节点索引（范围从 $0$ 到 $n-1$）；如果不指向任何节点，则为  `null` 。

你的代码 只 接受原链表的头节点 `head` 作为传入参数。



示例 1：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png)

```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]

输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```
示例 2：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e2.png)
```
输入：head = [[1,1],[2,1]]

输出：[[1,1],[2,1]]
```
示例 3：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e3.png)
```
输入：head = [[3,null],[3,0],[3,null]]

输出：[[3,null],[3,0],[3,null]]
```
示例 4：
```
输入：head = []

输出：[]

解释：给定的链表为空（空指针），因此返回 null。
```

提示：
* $0 <= n <= 1000$
* $-10000 <= Node.val <= 10000$

---

### 模拟 + 哈希表

如果不考虑 `random` 指针的话，对一条链表进行拷贝，我们只需要使用两个指针：一个用于遍历原链表，一个用于构造新链表（始终指向新链表的尾部）即可。这一步操作可看做是「创建节点 + 构建 `next` 指针关系」。

现在在此基础上增加一个 `random` 指针，我们可以将 `next` 指针和 `random` 指针关系的构建拆开进行：

1. 先不考虑 `random` 指针，和原本的链表复制一样，创建新新节点，并构造 `next` 指针关系，同时使用「哈希表」记录原节点和新节点的映射关系；
2. 对原链表和新链表进行同时遍历，对于原链表的每个节点上的 `random` 都通过「哈希表」找到对应的新 `random` 节点，并在新链表上构造 `random` 关系。

代码：
```Java
class Solution {
    public Node copyRandomList(Node head) {
        Node t = head;
        Node dummy = new Node(-10010), cur = dummy;
        Map<Node, Node> map = new HashMap<>();
        while (head != null) {
            Node node = new Node(head.val);
            map.put(head, node);
            cur.next = node;
            cur = cur.next; head = head.next;
        }
        cur = dummy.next; head = t;
        while (head != null) {
            cur.random = map.get(head.random);
            cur = cur.next; head = head.next;
        }
        return dummy.next;
    }
}
```
* 时间复杂度：$O(n)$
* 空间复杂度：$O(n)$

---

### 模拟（原地算法）

显然时间复杂度上无法优化，考虑如何降低空间（不使用「哈希表」）。

我们使用「哈希表」的目的为了实现原节点和新节点的映射关系，更进一步的是为了快速找到某个节点 `random` 在新链表的位置。

那么我们可以利用原链表的 `next` 做一个临时中转，从而实现映射。

具体的，我们可以按照如下流程进行：

1. 对原链表的每个节点节点进行复制，并追加到原节点的后面；
2. 完成 $1$ 操作之后，链表的奇数位置代表了原链表节点，链表的偶数位置代表了新链表节点，且每个原节点的 `next` 指针执行了对应的新节点。这时候，我们需要构造新链表的 `random` 指针关系，可以利用 `link[i + 1].random = link[i].random.next`，$i$ 为奇数下标，含义为 **新链表节点的 `random` 指针指向旧链表对应节点的 `random` 指针的下一个值**；
3. 对链表进行拆分操作。

![image.png](https://pic.leetcode-cn.com/1626919165-GuGmGo-image.png)

代码：
```Java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return head;
        Node t = head;
        while (head != null) {
            Node node = new Node(head.val);
            node.next = head.next;
            head.next = node;
            head = node.next;
        }
        head = t;
        while (head != null) {
            if (head.random != null) head.next.random = head.random.next;
            head = head.next.next;
        }
        head = t;
        Node ans = head.next;
        while (head != null) {
            Node ne = head.next;
            if (ne != null) head.next = ne.next;
            head = ne;
        }
        return ans;
    }
}
```
* 时间复杂度：$O(n)$
* 空间复杂度：$O(1)$

---

### 最后

这是我们「刷穿 LeetCode」系列文章的第 `No.剑指 Offer 35` 篇，系列开始于 2021/01/01，截止于起始日 LeetCode 上共有 1916 道题目，部分是有锁题，我们将先把所有不带锁的题目刷完。

在这个系列文章里面，除了讲解解题思路以外，还会尽可能给出最为简洁的代码。如果涉及通解还会相应的代码模板。

为了方便各位同学能够电脑上进行调试和提交代码，我建立了相关的仓库：https://github.com/SharingSource/LogicStack-LeetCode 。

在仓库地址里，你可以看到系列文章的题解链接、系列文章的相应代码、LeetCode 原题链接和其他优选题解。

