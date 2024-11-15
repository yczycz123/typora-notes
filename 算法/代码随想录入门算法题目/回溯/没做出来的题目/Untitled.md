[93. 复原 IP 地址 - 力扣（LeetCode）](https://leetcode.cn/problems/restore-ip-addresses/description/)



# 为什么错？

这里如果用path.delete(path.length() - str.length() - 1, path.length());直接报溢出错误

如果用path.setLength(preLength);结果几乎完全错误

这里的count是记录有几段数字

```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> ans = new ArrayList<>();
        StringBuilder path = new StringBuilder();
        dfs(s, path, ans, 0, 0);
        return ans;
    }

    public void dfs(String s, StringBuilder path, List<String> ans, int startIndex, int count) {
        if (count == 4 && startIndex == s.length()) {
            //到这里，path在最后多了一个.，删去
            path.deleteCharAt(path.length() - 1);
            ans.add(path.toString());
            return;
        }
        for (int i = startIndex; i < Math.min(startIndex + 3, s.length()); i++) {  //这里最多取3个字符作为ip地址的某个字段
            if (count > 4) {
                break;
            }
            String str = s.substring(startIndex, i + 1);
            if (isAns(str)) {
                int preLength=str.length();
                path.append(str);
                path.append(".");
                dfs(s, path, ans, i + 1, count + 1);
                //path.delete(path.length() - str.length() - 1, path.length()); //回溯
                path.setLength(preLength);
            }
        }
    }

    public boolean isAns(String str) {  //判断该段字符串是否可以作为ip地址的一部分（每个整数位于 0 到 255 之间组成，且不能含有前导 0
        if (str.length() >= 2 && str.charAt(0) == '0') {
            return false;
        }
        int val = Integer.valueOf(str);
        if (val < 0 || val > 255) {
            return false;
        }
        return true;
    }
}
```