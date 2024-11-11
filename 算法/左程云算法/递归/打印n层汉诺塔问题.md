





```java
public class Code07_test {
    public static void main(String[] args) {
        hanoiTower(3);
    }

    public static void hanoiTower(int n) {
        if (n > 0) {
            f(3,"左","右","中");
        }
    }
    
    //把所有移动抽象成从from移动到to

    public static void f(int i, String from, String to, String other) {
        if (i == 1) {
            System.out.println("移动圆盘1从" + from + "到" + to);
            return;
        }
        f(i - 1, from, other, to);
        System.out.println("移动圆盘" + i + "从" + from + "到" + to);
        f(i - 1, other, to, from);

    }
}

```