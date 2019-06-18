#### 尊重一下面试官咯。其实这个早就不用了。

>Dictionary

```java


/**
 * 这个字典抽象类是一些类的父类。例如HashTable，用来映射键值对。每一个键和值都是一个对象，在一个字典对象里面，每一个key最多只能映射一个value，value不为null，用equals来比较key的大小 过时的，新的应该实现的map接口。
 */
public abstract
class Dictionary<K,V> {
    public Dictionary() {
    }

    abstract public int size();

    abstract public boolean isEmpty();


    abstract public Enumeration<K> keys();


    abstract public Enumeration<V> elements();

    /**
     * @exception NullPointerException if the <tt>key</tt> is null
     */
    abstract public V get(Object key);

    abstract public V put(K key, V value);

    abstract public V remove(Object key);
}

```

>HashTable

#### hashTable是一个hash表的实现，用来映射key和value，key和value都不能为空，如果要从hashTable里面检索对象那么，对象应该实现hashcode和equals方法。初始化的容量和扩容因子，会影响他的性能，容量就是hashtable的数组的长度。当一个hashTable里面有较多的元素的时候hash碰撞会严重，扩容因子是决定了一个数组能填充多满，这两个参数仅仅是提示实现，剩下的细节是rehash的时候才会体现的。通常情况0.75的扩容因子是比较平衡的，初始化的容量控制了rehash时需要浪费的时间，如果初始化创建的元素比较多的话，他会自动rehash。与之前其他的集合不相同的是hashtable是同步的。

#### 问题1：HashTable如何保证同步。

#### 构造函数

```java
    //直接就创建了
    public Hashtable(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal Load: "+loadFactor);

        if (initialCapacity==0)
            initialCapacity = 1;
        this.loadFactor = loadFactor;
        table = new Entry<?,?>[initialCapacity];
        threshold = (int)Math.min(initialCapacity * loadFactor, MAX_ARRAY_SIZE + 1);//求最小值。。
    }

```

#### 有关同步的实现：

```java
    public synchronized int size() {
        return count;
    }
      public synchronized boolean isEmpty() {
        return count == 0;
    }
```

#### 同步实现都加了synchronized ^-^