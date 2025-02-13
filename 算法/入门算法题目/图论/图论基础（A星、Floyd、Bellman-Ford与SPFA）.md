



[算法讲解065【必备】A星、Floyd、Bellman-Ford与SPFA_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1t94y187zW/?spm_id_from=333.1387.upload.video_card.click&vd_source=96c1635797a0d7626fb60e973a29da38)



 [算法讲解065【必备】A星、Floyd、Bellman-Ford与SPFA.pptx](..\..\..\..\Java资料\数据结构与算法\zuoGod_algorithm-journey-main\ppt\算法讲解065【必备】A星、Floyd、Bellman-Ford与SPFA.pptx) 



# A*算法



[A * 算法精讲 （A star算法） | 代码随想录](https://www.programmercarl.com/kamacoder/0126.骑士的攻击astar.html#astar)



**Dijkstra算法是解决从某个源点到其他所有点的最短路径问题**



**A*算法解决的是从某个源点到另一个指定目标点的最短路径问题**



Dijkstra算法可以看作A*算法的一个特例





# Floyd算法



Floyd算法，得到图中任意两点之间的最短距离

时间复杂度O(n^3)，空间复杂度O(n^2)，常数时间小，容易实现

适用于任何图，不管有向无向、不管边权正负、但是不能有负环（保证最短路存在）





# Bellman-Ford算法



[代码随想录](https://www.programmercarl.com/kamacoder/0094.城市间货物运输I.html#思路)

**代码随想录讲的很清楚**，尤其是对为什么松弛n-1次讲的很清楚

解决可以有负权边（也可以没有）但是不能有负环（保证最短路存在）的图，单源最短路算法



**什么是负环？**

在图论中，**环**指的是一条从某个节点出发，经过若干个边，最后回到该节点的路径。如果这个环的总权重是负数（即路径上边的权重之和小于0），我们称这个环为**负环**。

例如，图中有三条边：

```
A --(-2)--> B
B --(2)--> C
C --(-1)--> A
```

这里形成了一个负环，路径 `A -> B -> C -> A` 的权重是 `-2 + 2 + (-1) = -1`，这是一个负环。





**松弛操作**

假设源点为A，从A到任意点F的最短距离为distance[F]

假设从点P出发某条边，去往点S，边权为W

如果发现，distance[P] + W < distance[S]，也就是通过该边可以让distance[S]变小

那么就说，P出发的这条边对点S进行了松弛操作







**算法步骤：**

Bellman-Ford过程

1，每一轮考察每条边，每条边都尝试进行松弛操作，那么若干点的distance会变小

2，当某一轮发现不再有松弛操作出现时，算法停止



## 算法模板





```java
import java.util.Arrays;

public class BellmanFord {

    // Bellman-Ford算法
    // graph是一个边的集合，每个边用一个三元组表示 (u, v, weight)，表示从u到v的边权重为weight
    // n 是图中节点的数量，src 是源节点
    public static boolean bellmanFord(int n, int[][] edges, int src) {
        // 初始化距离数组，将源节点的距离设为0，其他节点的距离设为无穷大
        int[] distance = new int[n];
        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[src] = 0;

        // 松弛操作：进行V-1次（节点数 - 1）遍历
        for (int i = 0; i < n - 1; i++) {
            for (int[] edge : edges) {
                int u = edge[0];
                int v = edge[1];
                int weight = edge[2];

                // 如果通过u到v的边能得到更短的路径，则更新v的距离
                if (distance[u] != Integer.MAX_VALUE && distance[u] + weight < distance[v]) {
                    distance[v] = distance[u] + weight;
                }
            }
        }

        // 检测负环：再进行一次松弛操作，如果能更新距离，则说明存在负环
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int weight = edge[2];

            // 如果找到还能更新的边，说明图中有负环
            if (distance[u] != Integer.MAX_VALUE && distance[u] + weight < distance[v]) {
                System.out.println("图中存在负权环！");
                return false; // 如果存在负环，返回false
            }
        }

        // 如果没有负环，打印所有节点的最短路径
        System.out.println("从源点 " + src + " 到其他节点的最短路径如下：");
        for (int i = 0; i < n; i++) {
            if (distance[i] == Integer.MAX_VALUE) {
                System.out.println("节点 " + i + " 不可达");
            } else {
                System.out.println("节点 " + i + ": 距离 = " + distance[i]);
            }
        }
        return true; // 没有负环，返回true
    }

    public static void main(String[] args) {
        // 示例图：6个节点，图的边用 (u, v, weight) 表示
        // 边的格式是 [起点, 终点, 权重]
        int[][] edges = {
            {0, 1, -1},  // 从0到1的边，权重为-1
            {0, 2, 4},   // 从0到2的边，权重为4
            {1, 2, 3},   // 从1到2的边，权重为3
            {1, 3, 2},   // 从1到3的边，权重为2
            {1, 4, 2},   // 从1到4的边，权重为2
            {3, 2, 5},   // 从3到2的边，权重为5
            {3, 1, 1},   // 从3到1的边，权重为1
            {4, 3, -3}   // 从4到3的边，权重为-3
        };

        int n = 5; // 图中的节点数（从0到4，共5个节点）
        int src = 0; // 源节点为0

        // 调用Bellman-Ford算法
        boolean result = bellmanFord(n, edges, src);

        // 如果存在负环，result会为false
        if (result) {
            System.out.println("最短路径计算完成。");
        }
    }
}

```







## 算法模板题目



[P3385 【模板】负环 - 洛谷 | 计算机科学教育新生态](https://www.luogu.com.cn/problem/P3385)



```java
import java.io.*;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;


//规定数据量的模板
public class Main {
    public static void main(String[] args) throws IOException {
        // 使用 BufferedReader 和 StreamTokenizer 来处理输入
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StreamTokenizer in = new StreamTokenizer(br);
        // 使用 PrintWriter 输出结果
        PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));

        // 读取测试数据组数 T
        in.nextToken();
        int T = (int) in.nval;  //测试数据组数

        // 对于每组测试数据进行处理
        for (int i = 0; i < T; i++) {
            // 初始化图，存储每个节点的邻接边
            List<List<int[]>> graph = new ArrayList<>();
            
            // 读取点的个数 n
            in.nextToken();
            int n = (int) in.nval;  //点的个数
            
            // 初始化图的邻接表，包含 n + 1 个空的列表
            // 注意：节点编号从 1 开始，因此索引是 n + 1
            for (int j = 0; j < n + 1; j++) {
                graph.add(new ArrayList<>());
            }

            // 读取边的个数 m
            in.nextToken();
            int m = (int) in.nval;  //边的个数

            // 读取每条边的信息，更新图的邻接表
            for (int j = 0; j < m; j++) {
                // 读取边的起点 from, 终点 to 和权重 w
                in.nextToken();
                int from = (int) in.nval;
                in.nextToken();
                int to = (int) in.nval;
                in.nextToken();
                int w = (int) in.nval;

                // 如果边的权重是非负，说明存在双向边，反向边也要存储
                if (w >= 0) {
                    // 存储从 from 到 to 的边
                    graph.get(from).add(new int[]{to, w});
                    // 存储从 to 到 from 的边（因为权重非负，反向边也存在）
                    graph.get(to).add(new int[]{from, w});
                } else {
                    // 如果是负权边，只需要存储一个方向
                    graph.get(from).add(new int[]{to, w});
                }
            }

            // 初始化距离数组，默认值为 Integer.MAX_VALUE（表示不可达）
            int[] distance = new int[n + 1];
            Arrays.fill(distance, Integer.MAX_VALUE);
            // 起点 1 的距离为 0
            distance[1] = 0;

            // 进行 n - 1 次松弛操作
            // 松弛操作的目的是更新最短路径
            for (int j = 0; j < n - 1; j++) {
                // 遍历每个节点，松弛所有从该节点出发的边
                for (int from = 1; from < n + 1; from++) {
                    // 遍历当前节点的所有邻接边
                    for (int[] edge : graph.get(from)) {
                        int to = edge[0];
                        int w = edge[1];
                        // 如果当前节点到目标节点的距离可以缩短，则更新距离
                        if (distance[from] != Integer.MAX_VALUE && distance[from] + w < distance[to]) {
                            distance[to] = distance[from] + w;
                        }
                    }
                }
            }

            // 用来标记是否存在负环
            boolean flag = false;
            // 在进行一次松弛操作，检查是否能继续更新最短路径
            // 如果能继续更新说明存在负环
            for (int from = 1; from < n + 1; from++) {
                for (int[] edge : graph.get(from)) {
                    int to = edge[0];
                    int w = edge[1];
                    // 检查是否能进一步松弛，即存在负环
                    if (distance[from] != Integer.MAX_VALUE && distance[from] + w < distance[to]) {
                        flag = true;  // 存在负环
                    }
                }
            }

            // 如果发现有负环，输出 "YES"，否则输出 "NO"
            if (flag) {
                out.println("YES");
            } else {
                out.println("NO");
            }
        }

        // 刷新输出缓冲区并关闭
        out.flush();
        out.close();
        br.close();
    }
}

```

