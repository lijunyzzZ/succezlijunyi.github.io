#### 看完了HashMap的实现继续看LinkedHashMap，看完了这个去看Collections里面的工具方法。  

#### 特点：hash表和双向链表的map实现，迭代器返回的顺序就是插入的顺序。和TreeMap不一样的是，他的顺序完全就是插入的顺序，而不是利用comparator或者自然排序的顺序。他可以根据map创建，这个时候LinkedHashMap的顺序就是map迭代的顺序。在构造的时候可以传一个boolean的参数如果是true就按照访问顺序，如果false就是插入顺序，这个数据结构适合构建lru缓存，操作values的视图不会影响原来的映射。removeEldestEntry用来强制删除过时的键值对。允许空值，add contains remove 具有常量的时间（如果hash方法足够平均）LinkhashMap初始化容量过高不会像hashmap一样影响剧烈。这个实现不是同步的，在多线程的使用的时候会出现concurrentException。如果非要使用这个数据结构建议自己针对实现一个同步的类，或者使用collections里面的方法获取一个同步的数据结构。HashLInkedMAp使用get方法竟然是一种结构的修改。

>Entry

```java
    //在节点的时候直接了afder和before来指定顺序。
  static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after;
        Entry(int hash, K key, V value, Node<K,V> next) {
            super(hash, key, value, next);
        }
    }
```

>变量
```java

    transient LinkedHashMap.Entry<K,V> head;
    transient LinkedHashMap.Entry<K,V> tail;

    /**
     * 迭代器的顺序若是true就是遍历顺序，若是false，就是插入顺序。不可变
     * @serial
     */
    final boolean accessOrder;
```

>get方法为什么是一个修改的方法：

```java
     public V get(Object key) {
        Node<K,V> e;
        if ((e = getNode(hash(key), key)) == null)
            return null;
        if (accessOrder)
            afterNodeAccess(e); //在这里调用了方法来转黄结构的顺序。
        return e.value;
    }

    //如果这个访问节点不是尾戒点，那么就把他置为尾戒点，
    void afterNodeAccess(Node<K,V> e) { // move node to last
        LinkedHashMap.Entry<K,V> last;
        if (accessOrder && (last = tail) != e) {
            LinkedHashMap.Entry<K,V> p =
                (LinkedHashMap.Entry<K,V>)e, b = p.before, a = p.after;
            p.after = null;
            if (b == null)
                head = a;
            else
                b.after = a;
            if (a != null)
                a.before = b;
            else
                last = b;
            if (last == null)
                head = p;
            else {
                p.before = last;
                last.after = p;
            }
            tail = p;
            ++modCount;
        }
    }
```

>迭代器：

LinkedHashMap的迭代器是包装的HashMap的这个地方就不说了。

>LRU

#### lru 是一种缓存算法，这个算法遵守这个规则：认为最近没有访问的缓存，以后也不会访问。所以在到缓存满载的时候就会丢掉时间最早的缓存。

#### 1. 他支持两种操作，一种是set操作：用来设置缓存，如果已经有这个缓存了，那么需要覆盖时间，如果没有这个缓存那么就新增一个，如果缓存的大小超出了删掉最早的缓存。
#### get操作用来获取某个缓存。

**作业：实现一个LRU缓存**