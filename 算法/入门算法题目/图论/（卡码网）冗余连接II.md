

知识点：并查集



[代码随想录](https://www.programmercarl.com/kamacoder/0109.冗余连接II.html)



题目对应力扣：[685. 冗余连接 II - 力扣（LeetCode）](https://leetcode.cn/problems/redundant-connection-ii/description/)



此题目考虑的是，在构建图的过程中，找到导致冲突的点（即一个节点出现两个父节点，也就是入度为2）和是否会出现环路



有三种情况：

有环有冲突，此时环和冲突一定发生在同一个地方，要删除形成环的那条边

无环有冲突，此时就删除输入顺序靠下面的边

有环无冲突。此时也是删除输入顺序靠下面的边





```java
import java.io.*;
import java.util.*;

//规定数据量的模板
public class Main {

    // 定义一个最大节点数为1010
    public static int maxN = 1010;
    // 初始化一个父节点数组，用于并查集
    public static int[] father = new int[maxN];

    public static void main(String[] args) throws IOException {
        // 创建输入输出流
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StreamTokenizer in = new StreamTokenizer(br);
        PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));

        // 读取节点和边的数量
        in.nextToken();
        int n = (int) in.nval;  // 节点和边的个数
        int[] inDegree = new int[n + 1];  // 用于存储各节点的入度

        // 创建边的数组，存储每一条边
        int[][] edges = new int[n][2];

        // 输入边的顺序，i从0到n
        for (int i = 0; i < n; i++) {
            in.nextToken();
            edges[i][0] = (int) in.nval;  // 边的起点
            in.nextToken();
            edges[i][1] = (int) in.nval;  // 边的终点
            inDegree[edges[i][1]]++;  // 更新终点的入度
        }

        // 创建一个列表，用于存储入度为2的冲突边
        List<Integer> conflict = new ArrayList<>();
        // 遍历边，查找入度为2的节点
        for (int i = 0; i < n; i++) {
            // 如果当前节点的入度为2，则加入到冲突列表中
            if (inDegree[edges[i][1]] == 2) {
                conflict.add(i);   // 如果出现入度为2的点，那么conflict中一定存放两条边
                // 也就是说，要么conflict为空，要么conflict中有两条边
                // 这里存放的是输入的顺序
            }
        }

        // 如果存在入度为2的点
        if (!conflict.isEmpty()) {
            // 由于是顺序进行遍历的，所以优先删除conflict[1]这条边
            if (isTreeAfterRemoveEdge(edges, conflict.get(1), n)) {
                // 如果删除conflict[1]后形成树，则输出这条边
                out.println(edges[conflict.get(1)][0] + " " + edges[conflict.get(1)][1]);
                out.flush();
                out.close();
                br.close();
            } else {
                // 否则输出conflict[0]这条边
                out.println(edges[conflict.get(0)][0] + " " + edges[conflict.get(0)][1]);
                out.flush();
                out.close();
                br.close();
            }
        } else {  // 如果没有入度为2的点，说明图中有环
            // 获取删除的环上的边
            int[] removeEdge = getRemoveEdge(edges, n);
            out.println(removeEdge[0] + " " + removeEdge[1]);  // 输出要删除的边
            out.flush();
            out.close();
            br.close();
        }
    }

    // 判断删除一条边后图是否成树
    public static boolean isTreeAfterRemoveEdge(int[][] edges, int i, int n) {
        init(n);  // 初始化并查集
        for (int j = 0; j < edges.length; j++) {
            int u = edges[j][0];  // 当前边的起点
            int v = edges[j][1];  // 当前边的终点
            if (j == i) {  // 如果当前是要删除的边，跳过
                continue;
            }
            if (isSameSet(u, v)) {  // 如果起点和终点已经在同一集合中，说明有环
                return false;
            } else {
                union(u, v);  // 否则合并两个节点
            }
        }
        return true;  // 如果遍历完所有边都没有问题，则是树
    }

    // 获取删除环上的一条边
    public static int[] getRemoveEdge(int[][] edges, int n) {
        init(n);  // 初始化并查集
        for (int i = 0; i < n; i++) {
            int u = edges[i][0];  // 当前边的起点
            int v = edges[i][1];  // 当前边的终点
            if (isSameSet(u, v)) {  // 如果起点和终点已经在同一集合中，说明有环
                return new int[]{u, v};  // 返回这条边
            } else {
                union(u, v);  // 否则合并两个节点
            }
        }
        return new int[]{0, 0};  // 如果没有环，返回一个无效的边，这个地方不可能执行到
    }

    // 初始化并查集
    public static void init(int n) {
        for (int i = 1; i < n + 1; i++) {
            father[i] = i;  // 每个节点的父节点指向自己
        }
    }

    // 查找根节点
    public static int find(int a) {
        if (a != father[a]) {  // 如果当前节点不是根节点
            father[a] = find(father[a]);  // 路径压缩，直接将父节点指向根节点
        }
        return father[a];  // 返回根节点
    }

    // 判断两个节点是否在同一个集合中
    public static boolean isSameSet(int a, int b) {
        return find(a) == find(b);  // 判断根节点是否相同
    }

    // 合并两个集合
    public static void union(int a, int b) {
        int fa = find(a);  // 找到a的根节点
        int fb = find(b);  // 找到b的根节点
        father[fa] = fb;  // 将a的根节点指向b的根节点
    }
}

```

