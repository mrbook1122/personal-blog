
# MySQL隔离级别

## MySQL隔离级别的作用

MySQL设置隔离级别是为了当多个事务同时进行时，在性能与可靠性、一致性和结果再现性之间进行平衡。

从最高的一致性保护到最低，MySQL(InnoDB引擎)一共有四个隔离级别：SERIALIZABLE, REPEATABLE READ, READ COMMITTED, 和 READ UNCOMMITTED。MySQL默认隔离级别为REPEATABLE READ。

> MySQL官方文档说SERIALIZABLE和READ UNCOMMITTED很少使用。(SERIALIZABLE enforces even stricter rules than REPEATABLE READ, and is used mainly in specialized situations, such as with XA transactions and for troubleshooting issues with concurrency and deadlocks.)[文档地址](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html)

## REPEATABLE READ

当启用这个事务隔离级别时，在同一事务中会启用一致读来读取第一次读的快照。

```java
class A implements Runnable {

    @Override
    public void run() {
        try (Connection connection = DriverManager.getConnection("jdbc:mysql://localhost/test",
                "root", "123456")) {
            System.out.println("a");

//            connection.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);
            connection.setAutoCommit(false);

            String update = "UPDATE a SET id = 87 WHERE id = 86";
            String query = "SELECT id FROM a where id > 80 FOR UPDATE";

            Statement statement = connection.createStatement();

//            statement.executeUpdate(update);
            statement.executeQuery(query);
            System.out.println("aa");
            Thread.sleep(10000);
            connection.commit();


        } catch (SQLException | InterruptedException e) {
            e.printStackTrace();
        }
    }
}

class B implements Runnable {

    @Override
    public void run() {
        try (Connection connection = DriverManager.getConnection("jdbc:mysql://localhost/test",
                "root", "123456")) {
            System.out.println("b");

//            connection.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);
            connection.setAutoCommit(false);

            String query = "UPDATE a SET id = 88 WHERE id = 87";
            String insert = "INSERT INTO a VALUES (85)";
            Statement statement = connection.createStatement();


            statement.executeUpdate(query);
            System.out.println("bb");
            connection.commit();

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

public class Main {
    public static void main(String[] args) {
        new Thread(new A()).start();
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        new Thread(new B()).start();
    }
}
```
