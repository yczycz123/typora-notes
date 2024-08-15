[设计有setAll功能的哈希表_牛客题霸_牛客网 (nowcoder.com)](https://www.nowcoder.com/practice/7c4559f138e74ceb9ba57d76fd169967)





```java
import java.io.*;
import java.util.HashMap;

public class Code01_test {
    public static HashMap<Integer, int[]> map = new HashMap<>();
    public static int setallvalue;
    public static int setallcount;
    public static int count=0;

    public static void put(int key, int value) {
        if (!map.containsKey(key)) {
            map.put(key, new int[]{value, ++count});
        } else {
            int[] v = map.get(key);
            v[0] = value;
            v[1] = ++count;
            map.put(key, v);
        }
    }

    public static int inquire(int key) {
        if (!map.containsKey(key)) {
            return -1;
        } else {
            int c=map.get(key)[1];
            if (setallcount < c) {
                return map.get(key)[0];
            } else {
                return setallvalue;
            }
        }
    }

    public static void setall(int value) {
        setallcount = ++count;
        setallvalue = value;
    }

    public static int n, key, value, opti;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StreamTokenizer in = new StreamTokenizer(br);
        PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));
        setallvalue=0;
        setallcount=0;
        in.nextToken();
        n = (int) in.nval;
        while (n != 0) {
            n--;
            in.nextToken();
            opti = (int) in.nval;
            if (opti == 1) {
                in.nextToken();
                key = (int) in.nval;
                in.nextToken();
                value = (int) in.nval;
                put(key, value);
            } else if (opti == 2) {
                in.nextToken();
                key = (int) in.nval;
                out.println(inquire(key));
            } else if (opti == 3) {
                in.nextToken();
                value = (int) in.nval;
                setall(value);
            }
        }
        out.flush();
        out.close();
        br.close();
    }
}
```