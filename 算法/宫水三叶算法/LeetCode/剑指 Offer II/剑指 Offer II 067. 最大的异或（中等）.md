### 题目描述

这是 LeetCode 上的 **[剑指 Offer II 067. 最大的异或](https://leetcode.cn/problems/ms70jA/solution/by-ac_oier-d9kx/)** ，难度为 **中等**。

Tag : 「贪心」、「字典树」




给定一个整数数组 `nums`，返回 `nums[i] XOR nums[j]` 的最大运算结果，其中 $0 ≤ i ≤ j < n$ 。

示例 1：
```
输入：nums = [3,10,5,25,2,8]

输出：28

解释：最大运算结果是 5 XOR 25 = 28.
```
示例 2：
```
输入：nums = [0]

输出：0
```
示例 3：
```
输入：nums = [2,4]

输出：6
```
示例 4：
```
输入：nums = [8,10,2]

输出：10
```
示例 5：
```
输入：nums = [14,70,53,83,49,91,36,80,92,51,66,70]

输出：127
```

提示：
* $1 <= nums.length <= 2 \times 10^4$
* $0 <= nums[i] <= 2^{31} - 1$


进阶：你可以在 $O(n)$ 的时间解决这个问题吗？

---

### 基本分析

要求得数组 `nums` 中的「最大异或结果」，假定 $nums[i]$ 与 $nums[j]$ 异或可以取得最终结果。

**由于异或计算「每位相互独立」（又称为不进位加法），同时具有「相同值异或结果为 $0$，不同值异或结果为 $1$」的特性。**

因此对于 $nums[j]$ 而言，可以从其二进制表示中的最高位开始往低位找，尽量让每一位的异或结果为 $1$，这样找到的 $nums[i]$ 与 $nums[j]$ 的异或结果才是最大的。

具体的，我们需要先将 `nums` 中下标范围为 $[0, j]$ 的数（二进制表示）加入 $Trie$ 中，然后每次贪心的匹配每一位（优先匹配与之不同的二进制位）。

---

### 证明

由于我们会从前往后扫描 `nums` 数组，因此 $nums[j]$ 必然会被处理到，所以我们只需要证明，在选定 $nums[j]$ 的情况下，我们的算法能够在 $[0, j]$ 范围内找到 $nums[i]$ 即可。

假定我们算法找出来的数值与 $nums[j]$ 的异或结果为 $x$，而真实的最优异或结果为 $y$。

接下来需要证得 $x$ 和 $y$ 相等。

由于找的是「最大异或结果」， 而 $x$ 是一个合法值，因此我们天然有 $x \leq y$。

然后利用反证法证明 $x \geq y$，假设 $x \geq y$ 不成立，即有 $x < y$，那么从两者的二进制表示的高位开始找，必然能找到第一位不同：$y$ 的「不同位」的值为 $1$，而 $x$ 的「不同位」的值为 $0$。

那么对应到选择这一个「不同位」的逻辑：能够选择与 $nums[j]$ 该位不同的值，使得该位的异或结果为 $1$，但是我们的算法选择了与 $nums[j]$ 该位相同的值，使得该位的异或结果为 $0$。

这与我们的算法逻辑冲突，因此必然不存在这样的「不同位」。即 $x < y$ 不成立，反证 $x \geq y$ 成立。

得证 $x$ 与 $y$ 相等。

---

### Trie 数组实现

可以使用数组来实现 $Trie$，但由于 OJ 每跑一个样例都会创建一个新的对象，因此使用数组实现，相当于每跑一个数据都需要 `new` 一个百万级别的数组，会 TLE 。

因此这里使用数组实现必须要做的一个优化是：**使用 `static` 来修饰 $Trie$ 数组，然后在初始化时做相应的清理工作。**

担心有不熟 Java 的同学，在代码里添加了相应注释说明。

代码：
```Java
class Solution {
    // static 成员整个类独一份，只有在类首次加载时才会创建，因此只会被 new 一次
    static int N = (int)1e6;
    static int[][] trie = new int[N][2];
    static int idx = 0;
    // 每跑一个数据，会被实例化一次，每次实例化的时候被调用，做清理工作
    public Solution() {
        for (int i = 0; i <= idx; i++) {
            Arrays.fill(trie[i], 0);
        }
        idx = 0;
    }
    void add(int x) {
        int p = 0;
        for (int i = 31; i >= 0; i--) {
            int u = (x >> i) & 1;
            if (trie[p][u] == 0) trie[p][u] = ++idx;
            p = trie[p][u];
        }
    }
    int getVal(int x) {
        int ans = 0;
        int p = 0;
        for (int i = 31; i >= 0; i--) {
            int a = (x >> i) & 1, b = 1 - a;
            if (trie[p][b] != 0) {
                ans |= (b << i);
                p = trie[p][b];
            } else {
                ans |= (a << i);
                p = trie[p][a];
            }
        }
        return ans;
    }
    public int findMaximumXOR(int[] nums) {
        int ans = 0;
        for (int i : nums) {
            add(i);
            int j = getVal(i);
            ans = Math.max(ans, i ^ j);
        }
        return ans;
    }
}
```
* 时间复杂度：$O(n)$
* 空间复杂度：$O(1e6)$

---

### Trie 类实现

相比于使用 `static` 来优化，一个更好的做法是使用类来实现 $Trie$，这样可以真正做到「按需分配」内存，缺点是会发生不确定次数的 `new`。

代码：
```Java
class Solution {
    class Node {
        Node[] ns = new Node[2];
    }
    Node root = new Node();
    void add(int x) {
        Node p = root;
        for (int i = 31; i >= 0; i--) {
            int u = (x >> i) & 1;
            if (p.ns[u] == null) p.ns[u] = new Node();
            p = p.ns[u];
        }
    }
    int getVal(int x) {
        int ans = 0;
        Node p = root;
        for (int i = 31; i >= 0; i--) {
            int a = (x >> i) & 1, b = 1 - a;
            if (p.ns[b] != null) {
                ans |= (b << i);
                p = p.ns[b];
            } else {
                ans |= (a << i);
                p = p.ns[a];
            }
        }
        return ans;
    }
    public int findMaximumXOR(int[] nums) {
        int ans = 0;
        for (int i : nums) {
            add(i);
            int j = getVal(i);
            ans = Math.max(ans, i ^ j);
        }
        return ans;
    }
}
```
* 时间复杂度：$O(n)$
* 空间复杂度：$O(n)$

---

### 最后

这是我们「刷穿 LeetCode」系列文章的第 `No.剑指 Offer II 067` 篇，系列开始于 2021/01/01，截止于起始日 LeetCode 上共有 1916 道题目，部分是有锁题，我们将先把所有不带锁的题目刷完。

在这个系列文章里面，除了讲解解题思路以外，还会尽可能给出最为简洁的代码。如果涉及通解还会相应的代码模板。

为了方便各位同学能够电脑上进行调试和提交代码，我建立了相关的仓库：https://github.com/SharingSource/LogicStack-LeetCode 。

在仓库地址里，你可以看到系列文章的题解链接、系列文章的相应代码、LeetCode 原题链接和其他优选题解。

