### 题目描述

这是 LeetCode 上的 **[剑指 Offer 04. 二维数组中的查找](https://leetcode.cn/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/solution/by-ac_oier-7jo0/)** ，难度为 **中等**。

Tag : 「二叉树搜索树」、「BST」、「二分」



在一个 $n \times m$ 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

示例:
```
现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]

给定 target = 5，返回 true。
给定 target = 20，返回 false。
```

限制：
* $0 <= n <= 1000$
* $0 <= m <= 1000$

---

### 二分

与 [（题解）74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/solution/gong-shui-san-xie-yi-ti-shuang-jie-er-fe-l0pq/) 不同，本题没有确保「每行的第一个整数大于前一行的最后一个整数」，因此我们无法采取「两次二分」的做法。

只能退而求之，遍历行/列，然后再对列/行进行二分。

代码：
```Java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix.length == 0 || matrix[0].length == 0) return false;
        int m = matrix.length, n = matrix[0].length;
        for (int i = 0; i < m; i++) {
            int l = 0, r = n - 1;
            while (l < r) {
                int mid = l + r + 1 >> 1;
                if (matrix[i][mid] <= target) l = mid;
                else r = mid - 1;
            }
            if (matrix[i][r] == target) return true;
        }
        return false;
    }
}
```
-
```Java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix.length == 0 || matrix[0].length == 0) return false;
        int m = matrix.length, n = matrix[0].length;
        for (int i = 0; i < n; i++) {
            int l = 0, r = m - 1;
            while (l < r) {
                int mid = l + r + 1 >> 1;
                if (matrix[mid][i] <= target) l = mid;
                else r = mid - 1;
            }
            if (matrix[r][i] == target) return true;
        }
        return false;
    }
}
```
* 时间复杂度：$O(m\log{n})$ 或 $O(n\log{m})$
* 空间复杂度：$O(1)$

---

### 抽象 BST

该做法则与 [（题解）74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/solution/gong-shui-san-xie-yi-ti-shuang-jie-er-fe-l0pq/) 的「解法二」完全一致。

我们可以将二维矩阵抽象成「以右上角为根的 BST」：

![image.png](https://pic.leetcode-cn.com/1617066993-AyRIiF-image.png)

那么我们可以从根（右上角）开始搜索，如果当前的节点不等于目标值，可以按照树的搜索顺序进行：

1. 当前节点「大于」目标值，搜索当前节点的「左子树」，也就是当前矩阵位置的「左方格子」，即 $c$--
2. 当前节点「小于」目标值，搜索当前节点的「右子树」，也就是当前矩阵位置的「下方格子」，即 $r$++

代码：
```Java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix.length == 0 || matrix[0].length == 0) return false;
        int m = matrix.length, n = matrix[0].length;
        int r = 0, c = n - 1;
        while (r < m && c >= 0) {
            if (matrix[r][c] < target) r++;
            else if (matrix[r][c] > target) c--;
            else return true;
        }
        return false;
    }
}
```
* 时间复杂度：$O(m + n)$
* 空间复杂度：$O(1)$

---

### 最后

这是我们「刷穿 LeetCode」系列文章的第 `剑指 Offer 04` 篇，系列开始于 2021/01/01，截止于起始日 LeetCode 上共有 1916 道题目，部分是有锁题，我们将先把所有不带锁的题目刷完。

在这个系列文章里面，除了讲解解题思路以外，还会尽可能给出最为简洁的代码。如果涉及通解还会相应的代码模板。

为了方便各位同学能够电脑上进行调试和提交代码，我建立了相关的仓库：https://github.com/SharingSource/LogicStack-LeetCode 。

在仓库地址里，你可以看到系列文章的题解链接、系列文章的相应代码、LeetCode 原题链接和其他优选题解。

