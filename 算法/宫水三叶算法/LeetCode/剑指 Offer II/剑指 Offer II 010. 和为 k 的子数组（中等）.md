### 题目描述

这是 LeetCode 上的 **[剑指 Offer II 010. 和为 k 的子数组](https://leetcode.cn/problems/QTMn0o/solution/by-ac_oier-hr6k/)** ，难度为 **中等**。

Tag : 「前缀和」、「哈希表」



给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回该数组中和为 `k` 的子数组的个数 。

示例 1：
```
输入：nums = [1,1,1], k = 2

输出：2
```
示例 2：
```
输入：nums = [1,2,3], k = 3

输出：2
```

提示：
* $1 <= nums.length <= 2 \times 10^4$
* $-1000 <= nums[i] <= 1000$
* $-10^7 <= k <= 10^7$

---

### 前缀和 + 哈希表

这是一道经典的前缀和运用题。

统计以每一个 $nums[i]$ 为结尾，和为 $k$ 的子数组数量即是答案。

我们可以预处理前缀和数组 `sum`（前缀和数组下标默认从 $1$ 开始），对于求解以某一个 $nums[i]$ 为结尾的，和为 $k$ 的子数组数量，本质上是求解在 $[0, i]$ 中，`sum` 数组中有多少个值为 $sum[i + 1] - k$ 的数，这可以在遍历过程中使用「哈希表」进行同步记录。

Java 代码：
```Java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length, ans = 0;
        int[] sum = new int[n + 10];
        for (int i = 1; i <= n; i++) sum[i] = sum[i - 1] + nums[i - 1];
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        for (int i = 1; i <= n; i++) {
            int t = sum[i], d = t - k;
            ans += map.getOrDefault(d, 0);
            map.put(t, map.getOrDefault(t, 0) + 1);
        }
        return ans;
    }
}
```
TypeScript 代码：
```TypeScript
function subarraySum(nums: number[], k: number): number {
    let n = nums.length, ans = 0
    const sum = new Array<number>(n + 10).fill(0)
    for (let i = 1; i <= n; i++) sum[i] = sum[i - 1] + nums[i - 1]
    const map = new Map<number, number>()
    map.set(0, 1)
    for (let i = 1; i <= n; i++) {
        const t = sum[i], d = t - k
        if (map.has(d)) ans += map.get(d)
        map.set(t, map.has(t) ? map.get(t) + 1 : 1)
    }
    return ans
};
```
* 时间复杂度：预处理前缀和的复杂度为 $O(n)$，统计答案的复杂度为 $O(n)$。整体复杂度为 $O(n)$
* 空间复杂度：$O(n)$

---

### 最后

这是我们「刷穿 LeetCode」系列文章的第 `剑指 Offer II 010` 篇，系列开始于 2021/01/01，截止于起始日 LeetCode 上共有 1916 道题目，部分是有锁题，我们将先把所有不带锁的题目刷完。

在这个系列文章里面，除了讲解解题思路以外，还会尽可能给出最为简洁的代码。如果涉及通解还会相应的代码模板。

为了方便各位同学能够电脑上进行调试和提交代码，我建立了相关的仓库：https://github.com/SharingSource/LogicStack-LeetCode 。

在仓库地址里，你可以看到系列文章的题解链接、系列文章的相应代码、LeetCode 原题链接和其他优选题解。

