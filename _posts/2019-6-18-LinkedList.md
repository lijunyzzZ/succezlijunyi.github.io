>linkedList

#### 2019年06月09日13:57:20

**LinkedList是实现list接口的双向链表**

在看LinkedList之前应该先看看他的父类AbstractSequentialList

>AbstractSequentialList

```java


/**
 * 这个类提供了一个顺序访问的list 相当于链表，如果是随机访问的应该使用 AbstractList
 * 这个类也实现了随机访问的方法 set 和get add  rermove通过使用listIterator
 *
 * 实现一个list的程序只需要实现listIterator 和size 方法。如果是不能修改的列表，只需要实现
 * 迭代器的hashNext next hasprevious previous 和index方法
 *
 * 对于一个可修改的list这个程序还应该实现迭代器的set方法，对于大小可变的list应该实现迭代器的remvoe和add
 *
 */

public abstract class AbstractSequentialList<E> extends AbstractList<E> {
 
    protected AbstractSequentialList() {
    }

    /**
     * 通过迭代器来返回下一个元素
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public E get(int index) {
        try {
            return listIterator(index).next();
        } catch (NoSuchElementException exc) {
            throw new IndexOutOfBoundsException("Index: "+index);
        }
    }

    /**
     * 通过迭代器来返回一个元素然后设置他
     */
    public E set(int index, E element) {
        try {
            ListIterator<E> e = listIterator(index);
            E oldVal = e.next();
            e.set(element);
            return oldVal;
        } catch (NoSuchElementException exc) {
            throw new IndexOutOfBoundsException("Index: "+index);
        }
    }

    /**
     * 通过迭代器来获取第index个元素然后在这个元素后面插入
     */
    public void add(int index, E element) {
        try {
            listIterator(index).add(element);
        } catch (NoSuchElementException exc) {
            throw new IndexOutOfBoundsException("Index: "+index);
        }
    }

    /**
     * 通过迭代器找到这个元素，然后删除他
     */
    public E remove(int index) {
        try {
            ListIterator<E> e = listIterator(index);
            E outCast = e.next();
            e.remove();
            return outCast;
        } catch (NoSuchElementException exc) {
            throw new IndexOutOfBoundsException("Index: "+index);
        }
    }


    // Bulk Operations

    /**
     * 将集合c的所有元素都加入到这个集合的第index位，这个也是通过迭代器的add方法实现的，
     * 如果在添加的过程中这个参数集合放生改变是无法预料的
     * 
     */
    public boolean addAll(int index, Collection<? extends E> c) {
        try {
            boolean modified = false;
            ListIterator<E> e1 = listIterator(index);
            Iterator<? extends E> e2 = c.iterator();
            while (e2.hasNext()) {
                e1.add(e2.next());
                modified = true;
            }
            return modified;
        } catch (NoSuchElementException exc) {
            throw new IndexOutOfBoundsException("Index: "+index);
        }
    }


    // Iterators
    public Iterator<E> iterator() {
        return listIterator();
    }

    /**
     * 返回一个序列化的迭代器list 由子类实现，
     */
    public abstract ListIterator<E> listIterator(int index);
}

```

>Deque

```java

package java.util;

/**
 * 一个线性的集合，双端队列，支持双端插入。这个接口支持没有容量限制的deque
 * 这个接口定义的方法都可以作用在元素的结尾，提供农了insert remove 和 examine
 * 这些方法都有两种形式存在，一种操作失败的时候抛异常，一种是返回false或者是空值
 * 
 * 双边队列也能被用来作为后进先出的栈，这个接口应该优于stack使用。作为stack使用的时候
 * 元素的push和pop都应该在队首开始。对应的方法： push  == addFirst
 * pop == removeFirst
 * peek == peekFirst
 * 对于queue 或者stack  peek始终都是从队首开始的。
 * 
 * 提供了两个删除内部元素的方法：removeFirstOccurrence removeLastOccurrence
 * 这个接口不提供通过index来访问 元素。
 *
 * 在使用这个双端队列的时候不建议插入null值，因为peek也会返回null，
 *
 * <p>This interface is a member of the <a
 * href="{@docRoot}/../technotes/guides/collections/index.html"> Java Collections
 * Framework</a>.
 *
 * @author Doug Lea
 * @author Josh Bloch
 * @since  1.6
 * @param <E> the type of elements held in this collection
 */
public interface Deque<E> extends Queue<E> {
    /**
     * 将元素加入到列表的头部之前，如果失败返回false
     */
    void addFirst(E e);

    /**
     * 在元素最后插入一个元素，如果失败了就返回false
     */
    void addLast(E e);

    /**
     * 在队列前面插入一个元素，如果成功就返回true，失败返回false 不会像add一样抛出异常
     */
    boolean offerFirst(E e);


    boolean offerLast(E e);

    /**
     * 删除队列的第一个元素，如果失败抛出异常
     */
    E removeFirst();

    E removeLast();

    /**
     * 删除队首元素，返回这个元素，如果队列为空返回null
     */
    E pollFirst();


    E pollLast();

    /**
     * 获取第一个元素，如果队列为空抛出异常。
     */
    E getFirst();


    E getLast();

    /**
     * 获取第一个元素，如果队列为null返回null
     */
    E peekFirst();


    E peekLast();

    /**
     * 删除从队首开始的第一个出现和o相同的元素，（判断依据都为null或者equals（））成功返回true
     */
    boolean removeFirstOccurrence(Object o);

    boolean removeLastOccurrence(Object o);

    // *** Queue methods ***

    boolean add(E e);

    boolean offer(E e);

    E remove();

    E poll();

    E element();

    E peek();


    // *** Stack methods ***

    /**
     * 加队首加一个元素和addFirst方法一样，如果违反了限制，抛出异常
     */
    void push(E e);

    /**
     * removeFirst
     */
    E pop();


    // *** Collection methods ***


    boolean remove(Object o);

    boolean contains(Object o);

    public int size();

    Iterator<E> iterator();

    Iterator<E> descendingIterator();

}

```

>Queue

```java

/**
 * 一个先进先出的集合，除了Collection提供的方法，他还提供了 插入 取出 查询操作方法
 * 如果一个操作失败了一种情况是抛出异常，一种是会返回一个null或者是false 这个决定于操作的实现。
 * 后面的处理主要是有容量限制的才会有，这种里面操作是没有办法失败的
 *
 *
 * 队列大多数情况都是先进先出. 其中也有例外例如先进后出的栈和按照一定优先级顺序的队列，插入到时候判断顺序。
 * 不关顺序是怎样的，调用remvoe移除元素。或者poll，在先进先出的队列里面新元素会被加到队列的末位。其他的
 * 队列会有自己的规则，每个队列的实现都应该指定他的顺序，
 * 
 * 如果使用offer插入一个元素否则返回一个false这个和add方法不同，这个方法被设计出来即便是操作失败的时候也是
 * 正常的，例如固定长度的队列。
 * 
 * remove和poll方法会返回这个队列的头，删除哪个元素取决于元素的排序策略。唯一不同的是，
 * poll 方法在队列是空的时候回返回false remove会抛出异常
 * 
 * element 和 peek会返回头节点但是不会删除这个节点，
 *
 * 如果要实现一个阻塞队列那么需要实现这个接口的子接口： Blockqueue
 *
 * 队列的实现通常是不允许插入一个null 但是有些可以 例如LinkedList，即便是可以允许空的元素也最好不要插入null值，
 * 因为在队列里面poll的返回值为null是代表了空集合
 * 
 *
 * 队列往往不会定义基于元素的hashcode或者equals方法，而是基于自身的hashCode和equals因为队列的顺序影响了这个的结果
 *
 *
 * 
 */
public interface Queue<E> extends Collection<E> {
    /**
     * 插入一个元素，如果成功返回true，如果么有剩余空间就抛出异常
     */
    boolean add(E e);

    /**
     * 插入一个元素成功返回true 失败返回false在指定空间的queue这个方法优于add
     */
    boolean offer(E e);

    /**
     * 删除一个元素，如果队列是空的就抛出异常
     *
     * @return the head of this queue
     * @throws NoSuchElementException if this queue is empty
     */
    E remove();

    /**
     * 删除一个队列的头部，如果成功就返回这个元素，没如果不城成功就返回null
     *
     */
    E poll();

    /**
     * 查看头部元素如果这个队列为空的时候抛出一个异常
     *
     * @return the head of this queue
     * @throws NoSuchElementException if this queue is empty
     */
    E element();

    /**
     * 返回队列头，如果这个队列是空的那么就返回null
     */
    E peek();
}

```

>LinkedList

#### 双向链表的实现，实现了list的所有操作方法，允许null，这个链表不是同步的，如果要在多线程的地方使这个链表，那么需要在外部同步。如果有这样的需求可以使用Collections里面synchronizedList包装一下对象，然后在多线程的场景下使用。他的迭代器也是fast-fail的

#### 成员变量

```java
    //大小
 transient int size = 0;

    /**
     * 第一个节点
     */
    transient Node<E> first;

    /**
     * 最后一个节点
     */
    transient Node<E> last
```

#### 构造方法

```java
    //创建一个空的list
    public LinkedList() {
    }

    /**
     * clone一个目标list 调用addAll来实现
     */
    public LinkedList(Collection<? extends E> c) {
        this();
        addAll(c);
    }
```

#### 节点类

```java
   private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```

#### 由于他很多都是委托给iterator来实现的方法，所以直接看迭代器的实现。

```java

    /**
     * 通过index来返回一个非空的节点。可以看出使用这个方法去获取节点的时候使用的是遍历。
     */
    Node<E> node(int index) {
        // assert isElementIndex(index);

        if (index < (size >> 1)) {
            Node<E> x = first;
            for (int i = 0; i < index; i++)
                x = x.next;
            return x;
        } else {
            Node<E> x = last;
            for (int i = size - 1; i > index; i--)
                x = x.prev;
            return x;
        }
    }
//迭代器的关键就是通过node方法来获取到第index的节点
private class ListItr implements ListIterator<E> {
        private Node<E> lastReturned;
        private Node<E> next;
        private int nextIndex;
        private int expectedModCount = modCount;

        ListItr(int index) {
            // assert isPositionIndex(index);
            next = (index == size) ? null : node(index);
            nextIndex = index;
        }

        public boolean hasNext() {
            return nextIndex < size;
        }

        public E next() {
            checkForComodification();
            if (!hasNext())
                throw new NoSuchElementException();

            lastReturned = next;
            next = next.next;
            nextIndex++;
            return lastReturned.item;
        }

        public boolean hasPrevious() {
            return nextIndex > 0;
        }

        public E previous() {
            checkForComodification();
            if (!hasPrevious())
                throw new NoSuchElementException();

            lastReturned = next = (next == null) ? last : next.prev;
            nextIndex--;
            return lastReturned.item;
        }

        public int nextIndex() {
            return nextIndex;
        }

        public int previousIndex() {
            return nextIndex - 1;
        }

        public void remove() {
            checkForComodification();
            if (lastReturned == null)
                throw new IllegalStateException();

            Node<E> lastNext = lastReturned.next;
            unlink(lastReturned);
            if (next == lastReturned)
                next = lastNext;
            else
                nextIndex--;
            lastReturned = null;
            expectedModCount++;
        }

        public void set(E e) {
            if (lastReturned == null)
                throw new IllegalStateException();
            checkForComodification();
            lastReturned.item = e;
        }

        public void add(E e) {
            checkForComodification();
            lastReturned = null;
            if (next == null)
                linkLast(e);
            else
                linkBefore(e, next);
            nextIndex++;
            expectedModCount++;
        }

        public void forEachRemaining(Consumer<? super E> action) {
            Objects.requireNonNull(action);
            while (modCount == expectedModCount && nextIndex < size) {
                action.accept(next.item);
                lastReturned = next;
                next = next.next;
                nextIndex++;
            }
            checkForComodification();
        }

        final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
    }

    //降序迭代器。
    private class DescendingIterator implements Iterator<E> {
        private final ListItr itr = new ListItr(size());
        public boolean hasNext() {
            return itr.hasPrevious();
        }
        public E next() {
            return itr.previous();
        }
        public void remove() {
            itr.remove();
        }
    }
```

#### 重要的方法：

```java
    /**
     * Saves the state of this {@code LinkedList} instance to a stream
     * (that is, serializes it).
     *
     * @serialData The size of the list (the number of elements it
     *             contains) is emitted (int), followed by all of its
     *             elements (each an Object) in the proper order.
     */
    private void writeObject(java.io.ObjectOutputStream s)
        throws java.io.IOException {
        // Write out any hidden serialization magic
        s.defaultWriteObject();

        // Write out size
        s.writeInt(size);

        // Write out all elements in the proper order.
        for (Node<E> x = first; x != null; x = x.next)
            s.writeObject(x.item);
    }

    /**
     * Reconstitutes this {@code LinkedList} instance from a stream
     * (that is, deserializes it).
     */
    @SuppressWarnings("unchecked")
    private void readObject(java.io.ObjectInputStream s)
        throws java.io.IOException, ClassNotFoundException {
        // Read in any hidden serialization magic
        s.defaultReadObject();

        // Read in size
        int size = s.readInt();

        // Read in all elements in the proper order.
        for (int i = 0; i < size; i++)
            linkLast((E)s.readObject());
    }
    //为什么把这两个方法来出来说呢，因为这个方法没有看见地方调用，查了一下才发现是jvm序列化一个对象的时候才会调用这个方法。
    //https://stackoverflow.com/questions/20421639/why-method-writeobject-in-linkedlist-is-private-but-never-been-called

```
