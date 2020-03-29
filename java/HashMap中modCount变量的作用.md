# HashMap中modCount变量的作用

今天阅读HashMap的源码时发现有一个modCount的变量，这个变量在每次调用putVal()方法时都会进行加一。

这是官方的对于这个变量的注释:

```java
/**
    * The number of times this HashMap has been structurally modified
    * Structural modifications are those that change the number of mappings in
    * the HashMap or otherwise modify its internal structure (e.g.,
    * rehash).  This field is used to make iterators on Collection-views of
    * the HashMap fail-fast.  (See ConcurrentModificationException).
    */
transient int modCount;
```

## 为什么要有这个变量

由于HashMap不是线程安全的，所以在迭代的时候，会将modCount赋值到迭代器的expectedModCount属性中，然后进行迭代。如果在迭代的过程中HashMap被其他线程修改了，modCount的数值就会发生变化，这个时候expectedModCount和ModCount不相等，迭代器就会抛出ConcurrentModificationException()异常。

```java
if (size > 0 && (tab = table) != null) {
    int mc = modCount;
    for (Node<K,V> e : tab) {
        for (; e != null; e = e.next)
            action.accept(e.key);
    }
    if (modCount != mc)
        throw new ConcurrentModificationException();
}
```
