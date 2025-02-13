

知识点： bellman_ford算法

[代码随想录](https://www.programmercarl.com/kamacoder/0094.城市间货物运输I.html#思路)







```java
import java.io.*;
import java.util.*;

// 规定数据量的模板
public class Main {

    public static void main(String[] args) throws IOException {
        // BufferedReader 用来快速读取输入，StreamTokenizer 用来处理输入的具体数据
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StreamTokenizer in = new StreamTokenizer(br);
        // PrintWriter 用来快速输出结果
        PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));

        // 读取城市数n和道路数m
        in.nextToken();
        int n = (int) in.nval;  // 点的个数，即城市的数量
        in.nextToken();
        int m = (int) in.nval;  // 边的个数，即道路的数量

        // 创建一个数组来存储每条道路的信息，包括起点、终点和道路的权值（运输成本 - 政府补贴）
        int[][] graph = new int[m][3];

        // 创建一个数组来存储每个城市的最短路径距离，初始化为无穷大
        // 我们用 Integer.MAX_VALUE 表示尚未访问的节点，0表示起始城市的距离
        int[] distance = new int[n + 1];
        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[1] = 0;  // 从城市1出发，起点的最短路径距离为0

        // 读取每条道路的信息
        for (int i = 0; i < m; i++) {
            in.nextToken();
            graph[i][0] = (int) in.nval;  // 起点城市编号
            in.nextToken();
            graph[i][1] = (int) in.nval;  // 终点城市编号
            in.nextToken();
            graph[i][2] = (int) in.nval;  // 道路的权值（运输成本 - 政府补贴）
        }

        // 进行 Bellman-Ford 算法的松弛操作，最多需要进行 n-1 次松弛
        for (int i = 0; i < n - 1; i++) {
            // 对每条道路进行松弛操作
            for (int j = 0; j < m; j++) {
                int from = graph[j][0];  // 起点城市
                int to = graph[j][1];    // 终点城市
                int weight = graph[j][2]; // 道路的权值（运输成本 - 政府补贴）

                // 如果起点城市有已知的最短路径，并且通过这条道路到达终点的路径更短
                if (distance[from] != Integer.MAX_VALUE && distance[from] + weight < distance[to]) {
                    // 更新终点城市的最短路径
                    distance[to] = distance[from] + weight;
                }
            }
        }

        // 判断从城市1到城市n的最短路径是否可达
        if (distance[n] == Integer.MAX_VALUE) {
            out.println("unconnected");  // 如果不可达，输出 "unconnected"
        } else {
            out.println(distance[n]);  // 否则输出从城市1到城市n的最短路径的综合运输成本
        }

        // 输出缓冲区的内容，并关闭输出流
        out.flush();
        out.close();
        // 关闭输入流
        br.close();
    }
}

```

