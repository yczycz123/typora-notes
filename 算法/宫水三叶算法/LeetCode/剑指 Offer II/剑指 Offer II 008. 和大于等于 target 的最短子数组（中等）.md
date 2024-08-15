### 题目描述

这是 LeetCode 上的 **[剑指 Offer II 008. 和大于等于 target 的最短子数组](https://leetcode.cn/problems/2VG8Kg/solution/by-ac_oier-vw5r/)** ，难度为 **中等**。

Tag : 「前缀和」、「二分」



给定一个含有 `n` 个正整数的数组和一个正整数 `target`。

找出该数组中满足其和 `≥ target` 的长度最小的 连续子数组 $[nums_{l}, nums_{l+1}, ..., nums_{r-1}, nums_{r}]$ ，并返回其长度。如果不存在符合条件的子数组，返回 $0$ 。

示例 1：
```
输入：target = 7, nums = [2,3,1,2,4,3]

输出：2

解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```
示例 2：
```
输入：target = 4, nums = [1,4,4]

输出：1
```
示例 3：
```
输入：target = 11, nums = [1,1,1,1,1,1,1,1]

输出：0
```

提示：
* $1 <= target <= 10^9$
* $1 <= nums.length <= 10^5$
* $1 <= nums[i] <= 10^5$

---

### 前缀和 + 二分

利用 $nums[i]$ 的数据范围为 $[1, 10^5]$，可知前缀和数组满足「单调递增」。

我们先预处理出前缀和数组 `sum`（前缀和数组下标默认从 $1$ 开始），对于每个 $nums[i]$ 而言，假设其对应的前缀和值为 $s = sum[i + 1]$，我们将 $nums[i]$ 视为子数组的右端点，问题转换为：在前缀和数组下标 $[0, i]$ 范围内找到满足「**值小于等于 $s - t$**」的最大下标，充当子数组左端点的前一个值。

利用前缀和数组的「单调递增」（即具有二段性），该操作可使用「二分」来做。

代码：
```Java
class Solution {
    public int minSubArrayLen(int t, int[] nums) {
        int n = nums.length, ans = n + 10;
        int[] sum = new int[n + 10];
        for (int i = 1; i <= n; i++) sum[i] = sum[i - 1] + nums[i - 1];
        for (int i = 1; i <= n; i++) {
            int s = sum[i], d = s - t;
            int l = 0, r = i;
            while (l < r) {
                int mid = l + r + 1 >> 1;
                if (sum[mid] <= d) l = mid;
                else r = mid - 1;
            }
            if (sum[r] <= d) ans = Math.min(ans, i - r);
        }
        return ans == n + 10 ? 0 : ans;
    }
}
```
* 时间复杂度：预处理前缀和数组的复杂度为 $O(n)$，遍历前缀和数组统计答案复杂度为 $O(n\log{n})$。整体复杂度为 $O(n\log{n})$
* 空间复杂度：$O(n)$

---

### 最后

这是我们「刷穿 LeetCode」系列文章的第 `剑指 Offer II 008` 篇，系列开始于 2021/01/01，截止于起始日 LeetCode 上共有 1916 道题目，部分是有锁题，我们将先把所有不带锁的题目刷完。

在这个系列文章里面，除了讲解解题思路以外，还会尽可能给出最为简洁的代码。如果涉及通解还会相应的代码模板。

为了方便各位同学能够电脑上进行调试和提交代码，我建立了相关的仓库：https://github.com/SharingSource/LogicStack-LeetCode 。

在仓库地址里，你可以看到系列文章的题解链接、系列文章的相应代码、LeetCode 原题链接和其他优选题解。

