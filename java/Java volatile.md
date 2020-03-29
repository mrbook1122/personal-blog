# Java volatile

volatile关键字是Java提供的最轻量级的同步机制。

当一个变量被volatile关键字修饰之后，它就具有可见性和有序性，但是不具有原子性。因为不具有原子性，所以在多线程竞争时，仍然会出现问题，比如：

```java
public class Main {
    private static volatile int a = 0;
    public static void main(String[] args) {
        for (int i = 0; i < 10; i++)
            new Thread(() -> {
                for (int j = 0; j < 100; j++)
                    a++;
            }).start();
        // IDEA会创建一条线程，所以>2
        while (Thread.activeCount() > 2)
            Thread.yield();
        System.out.println(a);
    }
}
```

以上代码的运行结果，a的值时小于等于1000的。

## **可见性**


