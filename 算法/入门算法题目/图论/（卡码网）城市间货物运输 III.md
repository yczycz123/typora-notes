

知识点： bellman_ford算法



[代码随想录](https://www.programmercarl.com/kamacoder/0096.城市间货物运输III.html#思路)

本题目就是[787. K 站中转内最便宜的航班 - 力扣（LeetCode）](https://leetcode.cn/problems/cheapest-flights-within-k-stops/description/)

**网站讲的很清楚，尤其是拓展部分，讲到了许多我之前不明白，迷糊的东西**

[算法讲解065【必备】A星、Floyd、Bellman-Ford与SPFA_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1t94y187zW/?spm_id_from=333.1387.upload.video_card.click&vd_source=96c1635797a0d7626fb60e973a29da38)

看完网站再看左神讲解，就清晰多了



```java
import java.io.*;
import java.util.*;

// 规定数据量的模板
public class Main {

    public static void main(String[] args) throws IOException {
        // 使用 BufferedReader 和 StreamTokenizer 来高效地读取输入数据
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StreamTokenizer in = new StreamTokenizer(br);
        // PrintWriter 用来输出结果，提高输出效率
        PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));

        // 读取城市数 n 和道路数 m
        in.nextToken();
        int n = (int) in.nval;  // 城市的数量
        in.nextToken();
        int m = (int) in.nval;  // 道路的数量

        // 初始化图的表示，graph[i] 用来存储第 i 条道路的信息
        int[][] graph = new int[m][3];  // 每条道路包含 3 个信息：起点、终点和权值

        // 读取每条道路的信息
        for (int i = 0; i < m; i++) {
            in.nextToken();
            graph[i][0] = (int) in.nval;  // 起点城市编号
            in.nextToken();
            graph[i][1] = (int) in.nval;  // 终点城市编号
            in.nextToken();
            graph[i][2] = (int) in.nval;  // 道路的权值（运输成本 - 政府补贴）
        }

        // 读取起点、终点和最多经过的城市数量 k
        in.nextToken();
        int src = (int) in.nval;  // 起点城市编号
        in.nextToken();
        int dst = (int) in.nval;  // 终点城市编号
        in.nextToken();
        int k = (int) in.nval;  // 最多经过的城市数量

        // 这里最多经过 k 个城市，也就是最多经过 k+1 条边

        // 初始化距离数组，distance[i] 表示从起点城市 src 到城市 i 的最短路径
        int[] distance = new int[n + 1];
        Arrays.fill(distance, Integer.MAX_VALUE);  // 默认每个城市的最短路径为无穷大
        distance[src] = 0;  // 起点城市到自己的最短路径为 0

        // 使用动态规划的思想进行多次松弛操作，最多进行 k+1 次（即最多经过 k 个城市）
        for (int i = 0; i < k + 1; i++) {
            // disCopy 用来保存当前阶段的路径结果，以便进行松弛操作
            int[] disCopy = Arrays.copyOf(distance, distance.length);

            // 遍历每条道路进行松弛操作
            for (int j = 0; j < m; j++) {
                int from = graph[j][0];  // 当前边的起点城市
                int to = graph[j][1];    // 当前边的终点城市
                int weight = graph[j][2]; // 当前边的权值

                // 如果起点城市的最短路径不为无穷大，且通过当前道路能够得到更短的路径
                if (disCopy[from] != Integer.MAX_VALUE && disCopy[from] + weight < distance[to]) {
                    // 更新终点城市的最短路径
                    distance[to] = disCopy[from] + weight;
                }
            }
        }

        // 判断从城市 src 到城市 dst 的最短路径是否为无穷大
        // 如果是无穷大，表示在给定的城市数量限制下无法到达城市 dst
        if (distance[dst] == Integer.MAX_VALUE) {
            out.println("unreachable");  // 输出 "unreachable"，表示无法到达目标城市
        } else {
            out.println(distance[dst]);  // 否则输出从 src 到 dst 的最低运输成本
        }

        // 输出缓冲区的内容，完成输出操作
        out.flush();
        out.close();
        // 关闭输入流
        br.close();
    }
}

```

