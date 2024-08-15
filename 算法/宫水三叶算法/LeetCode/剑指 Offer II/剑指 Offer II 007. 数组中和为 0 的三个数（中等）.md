### 题目描述

这是 LeetCode 上的 **[剑指 Offer II 007. 数组中和为 0 的三个数](https://leetcode.cn/problems/1fGaJU/solution/by-ac_oier-6mfb/)** ，难度为 **中等**。

Tag : 「双指针」、「排序」、「n 数之和」



给你一个包含 $n$ 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 `a`，`b`，`c` ，使得 `a + b + c = 0` ？

请你找出所有和为 $0$ 且不重复的三元组。

注意：答案中不可以包含重复的三元组。


示例 1：
```
输入：nums = [-1,0,1,2,-1,-4]

输出：[[-1,-1,2],[-1,0,1]]
```
示例 2：
```
输入：nums = []

输出：[]
```
示例 3：
```
输入：nums = [0]

输出：[]
```

提示：
* $0 <= nums.length <= 3000$
* $-10^5 <= nums[i] <= 10^5$

---

### 排序 + 双指针

对数组进行排序，使用三个指针 `i`、`j` 和 `k` 分别代表要找的三个数。

1. 通过枚举 `i` 确定第一个数，另外两个指针 `j`，`k` 分别从左边 `i + 1` 和右边 `n - 1` 往中间移动，找到满足 `nums[i] + nums[j] + nums[k] == 0` 的所有组合。

2. `j` 和 `k` 指针的移动逻辑，分情况讨论 `sum = nums[i] + nums[j] + nums[k]` ：
    * `sum` > 0：`k` 左移，使 `sum` 变小
    * `sum` < 0：`j` 右移，使 `sum` 变大
    * `sum` = 0：找到符合要求的答案，存起来

由于题目要求答案不能包含重复的三元组，所以在确定第一个数和第二个数的时候，要跳过数值一样的下标（在三数之和确定的情况下，确保第一个数和第二个数不会重复，即可保证三元组不重复）。

代码：
```Java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            int j = i + 1, k = n - 1;
            while (j < k) {
                while (j > i + 1 && j < n && nums[j] == nums[j - 1]) j++;
                if (j >= k) break;
                int sum = nums[i] + nums[j] + nums[k];
                if (sum == 0) {
                    ans.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    j++;
                } else if (sum > 0) {
                    k--;
                } else if (sum < 0) {
                    j++;
                }
            }
        }
        return ans;
    }
}
```
* 时间复杂度：排序的复杂度为 $O(n\log{n})$，对于每个 `i` 而言，最坏的情况 `j` 和 `k` 都要扫描一遍数组的剩余部分，复杂度为 $O(n^2)$。整体复杂度为 $O(n^2)$
* 空间复杂度：$O(\log{n})$

---
### 最后

这是我们「刷穿 LeetCode」系列文章的第 `剑指 Offer II 007` 篇，系列开始于 2021/01/01，截止于起始日 LeetCode 上共有 1916 道题目，部分是有锁题，我们将先把所有不带锁的题目刷完。

在这个系列文章里面，除了讲解解题思路以外，还会尽可能给出最为简洁的代码。如果涉及通解还会相应的代码模板。

为了方便各位同学能够电脑上进行调试和提交代码，我建立了相关的仓库：https://github.com/SharingSource/LogicStack-LeetCode 。

在仓库地址里，你可以看到系列文章的题解链接、系列文章的相应代码、LeetCode 原题链接和其他优选题解。

