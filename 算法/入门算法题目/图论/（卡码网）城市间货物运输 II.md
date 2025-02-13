

知识点： bellman_ford算法

[95. 城市间货物运输 II](https://kamacoder.com/problempage.php?pid=1153)





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

        // 初始化距离数组，distance[i] 表示从城市 1 到城市 i 的最短路径
        // 将所有城市的初始距离设为无穷大，表示不可达
        int[] distance = new int[n + 1];
        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[1] = 0;  // 起点城市 1 到自己的距离为 0

        // 读取每条道路的信息
        for (int i = 0; i < m; i++) {
            in.nextToken();
            graph[i][0] = (int) in.nval;  // 起点城市编号
            in.nextToken();
            graph[i][1] = (int) in.nval;  // 终点城市编号
            in.nextToken();
            graph[i][2] = (int) in.nval;  // 道路的权值（运输成本 - 政府补贴）
        }

        // 执行 Bellman-Ford 算法来计算最短路径
        // Bellman-Ford 算法的核心是通过多次松弛边来计算最短路径
        // 最多进行 n-1 次松弛操作
        for (int i = 0; i < n - 1; i++) {
            // 对所有边进行松弛操作
            for (int j = 0; j < m; j++) {
                int from = graph[j][0];  // 当前边的起点
                int to = graph[j][1];    // 当前边的终点
                int weight = graph[j][2]; // 当前边的权值

                // 如果起点的最短路径不为无穷大，并且通过当前边松弛后能得到更短路径
                if (distance[from] != Integer.MAX_VALUE && distance[from] + weight < distance[to]) {
                    // 更新终点的最短路径
                    distance[to] = distance[from] + weight;
                }
            }
        }

        // 检测是否存在负权回路
        // 如果存在负权回路，某条边的终点的距离仍然可以通过松弛更新，说明存在负权回路
        for (int j = 0; j < m; j++) {
            int from = graph[j][0];  // 当前边的起点
            int to = graph[j][1];    // 当前边的终点
            int weight = graph[j][2]; // 当前边的权值

            // 检查是否有负权回路
            if (distance[from] != Integer.MAX_VALUE && distance[from] + weight < distance[to]) {
                // 如果发现了负权回路，输出 "circle" 并结束程序
                out.println("circle");
                out.flush();
                out.close();
                br.close();
                return;  // 直接返回，结束程序
            }
        }

        // 如果从城市 1 到城市 n 的距离仍然是无穷大，说明两者不连通
        if (distance[n] == Integer.MAX_VALUE) {
            out.println("unconnected");  // 如果不可达，输出 "unconnected"
        } else {
            out.println(distance[n]);  // 否则输出最短路径的距离
        }

        // 输出缓冲区的内容，完成输出操作
        out.flush();
        out.close();
        // 关闭输入流
        br.close();
    }
}

```



### 代码详细注释：

1. **输入部分**：
   - 使用 `BufferedReader` 和 `StreamTokenizer` 来读取输入。`StreamTokenizer` 将输入流分解成一系列的 token，在这个问题中每个 token 都是一个整数。读取数据时，首先读入城市数 `n` 和道路数 `m`，接着读取每条道路的信息（起点城市 `s`，终点城市 `t`，以及这条道路的权值 `v`）。
2. **图的表示**：
   - `graph[i]` 用来存储第 `i` 条道路的信息，包括起点、终点和该条道路的权值（`v`）。
   - `distance[]` 数组用于保存从起点（城市 1）到其他城市的最短路径，初始值设为 `Integer.MAX_VALUE`，表示不可达，只有起点城市的距离为 0。
3. **Bellman-Ford 算法**：
   - Bellman-Ford 算法通过多次松弛操作（最多 `n-1` 次）来计算最短路径。松弛操作的原理是：如果从某个节点的已知最短路径加上边的权值比目标节点的已知最短路径更短，就更新目标节点的最短路径。
   - 对于每一条边，如果经过松弛操作可以得到更短的路径，就更新相应的城市的最短路径。
4. **检测负权回路**：
   - Bellman-Ford 算法还可以用来检测图中是否存在负权回路。如果在 `n-1` 次松弛操作之后，仍然有边可以被松弛，说明图中存在负权回路。此时输出 `"circle"`，并结束程序。
5. **结果输出**：
   - 如果从城市 1 到城市 n 的最短路径为 `Integer.MAX_VALUE`，说明这两个城市不连通，输出 `"unconnected"`。
   - 否则，输出从城市 1 到城市 n 的最短路径。如果该路径的值为负数，表示通过这条路径不仅能覆盖运输成本，还能获得政府补贴，理论上是有盈利的。

### 核心算法：

- **Bellman-Ford 算法**：通过多次对所有边进行松弛操作，能够正确地计算最短路径，并且能检测负权回路。该算法适用于有负权边的图，但时间复杂度较高，为 `O(n * m)`。

### 注意：

- **负权回路检测**：如果在 `n-1` 次松弛操作之后，仍然有可以松弛的边，意味着图中存在负权回路，这时候应该输出 `"circle"`。
- **不可达判断**：如果 `distance[n]` 仍为 `Integer.MAX_VALUE`，说明从城市 1 到城市 n 不存在路径，应该输出 `"unconnected"`。

### 总结：

这段代码实现了一个通过 Bellman-Ford 算法计算最短路径的功能，并且能够检测负权回路，最后根据最短路径的结果输出相应的消息。