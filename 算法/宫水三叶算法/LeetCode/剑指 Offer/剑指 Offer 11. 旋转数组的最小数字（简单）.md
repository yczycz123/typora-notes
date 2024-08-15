### 题目描述

这是 LeetCode 上的 **[剑指 Offer 11. 旋转数组的最小数字](https://leetcode.cn/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/solution/by-ac_oier-p751/)** ，难度为 **简单**。

Tag : 「二分」



把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。

给你一个可能存在 重复 元素值的数组 `numbers`，它原来是一个升序排列的数组，并按上述情形进行了一次旋转。请返回旋转数组的最小元素。例如，数组 `[3,4,5,1,2]` 为 `[1,2,3,4,5]` 的一次旋转，该数组的最小值为 $1$。  

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` 旋转一次 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`。

示例 1：
```
输入：numbers = [3,4,5,1,2]

输出：1
```
示例 2：
```
输入：numbers = [2,2,2,0,1]

输出：0
```

提示：
* $n = numbers.length$
* $1 <= n <= 5000$
* $-5000 <= numbers[i] <= 5000$
* `numbers` 原来是一个升序排序的数组，并进行了 $1$ 至 $n$ 次旋转

---

### 二分

根据题意，我们知道，所谓的旋转其实就是「将某个下标前面的所有数整体移到后面，使得数组从整体有序变为分段有序」。

但和 [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/solution/gong-shui-san-xie-yan-ge-olognyi-qi-kan-6d969/) 不同的是，本题元素并不唯一。

**这意味着我们无法直接根据与** $nums[0]$ **的大小关系，将数组划分为两段，即无法通过「二分」来找到旋转点。**

**因为「二分」的本质是二段性，并非单调性。只要一段满足某个性质，另外一段不满足某个性质，就可以用「二分」。**

如果你有看过我 [严格 O(logN)，一起看清二分的本质](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/solution/shua-chuan-lc-yan-ge-ologn100yi-qi-kan-q-xifo/) 这篇题解，你应该很容易就理解上句话的意思。如果没有也没关系，我们可以先解决本题，在理解后你再去做 [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)，我认为这两题都是一样的，不存在先后关系。

举个🌰，我们使用数据 `[0,1,2,2,2,3,4,5]` 来理解为什么不同的旋转点会导致「二段性丢失」：

![image.png](https://pic.leetcode-cn.com/1617852745-LoBNPK-image.png)

java 代码：
```Java
class Solution {
    public int minArray(int[] nums) {
        int n = nums.length;
        int l = 0, r = n - 1;
        while (l < r && nums[0] == nums[r]) r--;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] >= nums[0]) l = mid;                
            else r = mid - 1;
        }
        return r + 1 < n ? nums[r + 1] : nums[0];
    }
}
```
TypeScript 代码：
```TypeScript
function minArray(nums: number[]): number {
    const n = nums.length
    let l = 0, r = n - 1
    while (l < r && nums[0] == nums[r]) r--
    while (l < r) {
        const mid = l + r + 1 >> 1
        if (nums[mid] >= nums[0]) l = mid
        else r = mid - 1
    }
    return r + 1 < n ? nums[r + 1] : nums[0]
};
```
* 时间复杂度：恢复二段性处理中，最坏的情况下（考虑整个数组都是同一个数）复杂度是 $O(n)$，而之后的找旋转点是「二分」，复杂度为 $O(\log{n})$。整体复杂度为 $O(n)$
* 空间复杂度：$O(1)$

---

### 最后

这是我们「刷穿 LeetCode」系列文章的第 `剑指 Offer 11` 篇，系列开始于 2021/01/01，截止于起始日 LeetCode 上共有 1916 道题目，部分是有锁题，我们将先把所有不带锁的题目刷完。

在这个系列文章里面，除了讲解解题思路以外，还会尽可能给出最为简洁的代码。如果涉及通解还会相应的代码模板。

为了方便各位同学能够电脑上进行调试和提交代码，我建立了相关的仓库：https://github.com/SharingSource/LogicStack-LeetCode 。

在仓库地址里，你可以看到系列文章的题解链接、系列文章的相应代码、LeetCode 原题链接和其他优选题解。

