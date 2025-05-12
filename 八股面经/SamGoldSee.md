# SamGoldSee 八股

## JavaSE

### 集合的分类有哪些？

Java 集合主要分为两大体系：

- **Collection** 接口：存储单个元素

  - List：有序可重复，如 ArrayList、 LinkedList

  - Set：无序不可重复，如 HashSet、 TreeSet、 LinkedHashSet

  - Queue：队列，先进先出，如 LinkedList、 PriorityQueue

- **Map** 接口：存储键值对

  - HashMap：无序的键值对映射表，线程不安全，效率高，默认初始容量为 16，扩容时加载因子为 0.75

  - HashTable：同 HashMap，线程安全，效率低

  - TreeMap：有序的键值对映射表，线程不安全，效率高

  - LinkedHashMap：HashMap 的子类，保持插入顺序，线程不安全，效率高

  - ConcurrentHashMap：HashMap 的子类，线程安全，效率高，默认初始容量为 16，扩容时加载因子为 0.75

### 迭代器的实现原理是什么？

迭代器本质是通过内部类维护游标（如`cursor`）来遍历集合，同时会维护 `modCount` 用于快速失败机制，用于实现对集合元素的 **顺序访问** ，同时提供 **结构安全(fail-fast)** 的遍历过程

- 核心结构

`cursor` 作为当前迭代到的位置，指向下一个要返回的元素

`lastRet` 指向上一个通过 `next()` 返回的元素索引，供 `remove()` 使用

`expectedModCount` 创建迭代器时记录的集合修改次数，用于检测并发修改

`modCount` 记录集合的结构性修改次数，用于快速失败机制

- 主要方法

`hasNext()` 判断当前 `cursor` 是否小于 `size()` ，用于确认是否还有元素未被遍历

`next()` 先调用 `checkForComodification()` 检查集合结构是否被修改，再调用集合的 `get(cursor)` 返回元素，更新 `lastRet = cursor++` ，

`remove()` 先调用 `checkForComodification()` 检查集合结构是否被修改，删除上一次 `next()` 返回的元素，删除后同步 `expectedModCount = modCount` ，维持 fail-fast 机制正常工作

#### 快速失败机制

使用 `modCount`（集合结构修改次数）与 `expectedModCount`（迭代器快照）进行比较。

若两者不一致，说明集合在迭代中被非法修改，立即抛出 `ConcurrentModificationException`。

这是防御式编程策略，目的是避免返回错误结果或产生不一致行为。

```java
public abstract class AbstractList<E> extends AbstractCollection<E> implements List<E> {

    protected AbstractList() {
    }

    ...

    public Iterator<E> iterator() {
        return new Itr(); // 这里返回一个迭代器
    }

    private class Itr implements Iterator<E> {

        int cursor = 0;
        int lastRet = -1;
        int expectedModCount = modCount;

        public boolean hasNext() {
            return cursor != size();
        }

        public E next() {
            checkForComodification();
            try {
                E next = get(cursor);
                lastRet = cursor++;
                return next;
            } catch (IndexOutOfBoundsException e) {
                checkForComodification();
                throw new NoSuchElementException();
            }
        }

        public void remove() {
            if (lastRet == -1)
                throw new IllegalStateException(); // 说明 next() 还没被调用，或者 remove 已被调用

            checkForComodification(); // 检查结构是否被非法修改

            try {
                AbstractList.this.remove(lastRet); // 调用外部类的 remove 方法
                if (lastRet < cursor)
                    cursor--; // 删除后 cursor 要回退
                lastRet = -1; // 重置状态，确保 remove 只能调用一次
                expectedModCount = modCount; // 同步 modCount，确保 fail-fast 正常工作
            } catch (IndexOutOfBoundsException e) {
                throw new ConcurrentModificationException();
            }
        }

        final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
    }
}
```

### 为什么 `new ArrayList<>()` 时建议指定初始化容量值

因为 ArrayList 底层是数组，动态扩容代价较高（涉及数组拷贝），若预估元素较多，可提前设置容量减少扩容次数，提高性能。

### 为什么 ArrayList 默认情况下的扩容机制是扩容为原数组的 1.5 倍

主要是性能与空间的权衡。扩容过小会频繁扩容，影响性能；过大浪费内存。1.5 倍是实践中较优的折中方案。

### ArrayList 是线程的安全吗

不是。多个线程同时读写会造成数据不一致。可使用 `Collections.synchronizedList()` 或 `CopyOnWriteArrayList` 替代。

### `transient` 关键字的作用是什么？

主要作用就是让某些 **被 transient 关键字修饰的成员属性变量不被序列化/持久化** ，即使被序列化，也不会被序列化该成员属性。

可以用于一些敏感信息不希望被序列化的场景，如密码、密钥等。

### ArrayList 源码分析

```java
public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

- **特点**

基于 **动态数组** 实现（扩容机制），允许 **快速随机访问** 实现（`RandomAccess`），元素可重复，允许 `null` ，非线程安全，适合读多写少的场景。

#### 核心字段

```java
transient Object[] elementData; // 存储元素的数组（真实数据存储）
private int size;               // 当前 ArrayList 中元素的数量
```

`elementData` 是一个 **Object[] 数组** ，默认初始容量为 10
`size` 是实际元素数量，与 `elementData.length` 不一定相同（实际数量和容量）

#### 构造方法

```java
public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: " + initialCapacity);
    }
}

public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

支持初始化指定容量或默认懒加载（初始化 10，后续动态扩容影响性能）

#### 添加元素 & 扩容机制

```java
public boolean add(E e) {
    modCount++; // 记录结构性修改次数
    add(e, elementData, size);
    return true;
}

private void add(E e, Object[] elementData, int s) {
    if (s == elementData.length) // 判断容量是否已满
        elementData = grow(); // 已满调用自动扩容方法
    elementData[s] = e;
    size = s + 1;
}

private Object[] grow() {
    return grow(size + 1);
}

private Object[] grow(int minCapacity) {
    int oldCapacity = elementData.length;
    if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) { // 非空或默认容量
        int newCapacity = ArraysSupport.newLength(oldCapacity,
            minCapacity - oldCapacity, /* minimum growth */
            oldCapacity >> 1           /* preferred growth */);
        return elementData = Arrays.copyOf(elementData, newCapacity); // 复制已有内容到新扩容后的数组
    } else {
        return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)]; // 空数组，则扩容至默认容量10或需求的最小容量
    }
}

public static int newLength(int oldLength, int minGrowth, int prefGrowth) {
    // preconditions not checked because of inlining
    // assert oldLength >= 0
    // assert minGrowth > 0

    int prefLength = oldLength + Math.max(minGrowth, prefGrowth); // might overflow
    if (0 < prefLength && prefLength <= SOFT_MAX_ARRAY_LENGTH) { // SOFT_MAX_ARRAY_LENGTH = Integer.MAX_VALUE - 8
        return prefLength;
    } else {
        // put code cold in a separate method
        return hugeLength(oldLength, minGrowth);
    }
}

private static int hugeLength(int oldLength, int minGrowth) {
    int minLength = oldLength + minGrowth;
    if (minLength < 0) { // overflow
        throw new OutOfMemoryError(
            "Required array length " + oldLength + " + " + minGrowth + " is too large");
    } else if (minLength <= SOFT_MAX_ARRAY_LENGTH) {
        return SOFT_MAX_ARRAY_LENGTH;
    } else {
        return minLength;
    }
}
```

当元素数量超出当前容量，调用 `grow()` 自动扩容，扩容策略：`oldCapacity + Math.max(minGrowth, oldCapacity >> 1)`，其中 `minGrowth` 为实际需要的数组长度，`oldCapacity >> 1` 是默认扩容因子（1.5 倍）

`newLength()` 和 `hugeLength()` 专门用于处理数组扩容计算，确保既 **高效扩容** 又 **避免内存溢出** 。

`newLength()` 保证，只要不超过最大安全数组长度（`Integer.MAX_VALUE - 8`，避免 JVM 数组开销带来的问题）就使用这个新长度，如果超限了，就使用 `hugeLength()` 进行进一步判断。

`hugeLength()` 对于绝对需要的最小长度进行判断：如果小于 0（符号位进 1，导致溢出），则报错；小于等于最大安全值，尽可能取最大安全值；否则取最小长度（超大容量申请，不推荐但允许）。

#### 删除元素

- **按照索引删除**

```java
public E remove(int index) {
    Objects.checkIndex(index, size); // 检查index是否越界
    final Object[] es = elementData;

    @SuppressWarnings("unchecked") E oldValue = (E) es[index];
    fastRemove(es, index);

    return oldValue;
}

private void fastRemove(Object[] es, int i) {
    modCount++; // 记录结构性修改次数
    final int newSize;
    if ((newSize = size - 1) > i) // 判断是不是队尾元素
        System.arraycopy(es, i + 1, es, i, newSize - i); // 将 i+1 到 size-1 位置的元素复制到 i 位置
    es[size = newSize] = null;
}
```

删除后需要将后续元素向前移动。移动的时间复杂度是 O(n) （将后续元素依次向前复制）

- **按照元素删除**

```java
public boolean remove(Object o) {
    final Object[] es = elementData;
    final int size = this.size;
    int i = 0;
    found: {
        if (o == null) {
            for (; i < size; i++)
                if (es[i] == null)
                    break found;
        } else {
            for (; i < size; i++)
                if (o.equals(es[i]))
                    break found;
        }
        return false;
    }
    fastRemove(es, i);
    return true;
}
```

遍历数组查询元素，查询后调用 `fastRemove()` 根据索引删除元素

#### 访问元素

```java
public E get(int index) {
    Objects.checkIndex(index, size);
    return elementData(index);
}
```

#### 修改元素

```java
public E set(int index, E element) {
    Objects.checkIndex(index, size);
    E oldValue = elementData(index);
    elementData[index] = element;
    return oldValue;
}
```

#### 总结

`ArrayList` 是基于动态数组实现的顺序集合，支持快速随机访问 `O(1)`，但插入和删除中间元素需要移动数组 `O(n)`。通过 `ensureCapacity()` 和 `grow()` 动态扩容，默认扩容为 1.5 倍。支持 `fail-fast` 机制用于防止迭代期间结构性并发修改。

### CopyOnWriteArrayList 的实现原理和源码分析

写操作时复制原数组，修改完后再替换原数组，读操作无锁。这种读多写少场景性能好，但写开销较大。

```java
public class CopyOnWriteArrayList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

- **特点**

**线程安全** ， 采用 **写时复制**（Copy-On-Write）策略，适合读多写少（每次写操作都会对数组进行一次复制），读无锁，写加锁（`ReentrantLock`保证写操作线程安全）

#### 核心字段

```java
private transient volatile Object[] array;
final transient ReentrantLock lock = new ReentrantLock();
```

`array` 是当前使用的数据快照（`volatile` 确保可见性），写操作都在持锁后进行，确保线程安全

#### 构造方法

```java
public CopyOnWriteArrayList() {
    array = new Object[0];
}

public CopyOnWriteArrayList(Collection<? extends E> c) {
    Object[] elements = c.toArray();
    if (elements.getClass() != Object[].class)
        elements = Arrays.copyOf(elements, elements.length, Object[].class);
    this.array = elements;
}
```

无参构造的时候，初始化为一个 **空数组** 但不是 `null` ，可以避免后续空指针异常，也符合写时复制的策略（首次写操作会扩容并复制）

有参构造的时候，将集合 `c` 转换为数组，在这一步中，大多数集合返回 `Object[]` 数组，但是如 `ArrayList<E>` 等集合会返回更精确的数组 `E[]`，所以需要进行类型转换。

#### 添加元素（写操作：加锁 + 拷贝）

```java
public boolean add(E e) {
    synchronized (lock) {
        Object[] es = getArray();
        int len = es.length;
        es = Arrays.copyOf(es, len + 1);
        es[len] = e;
        setArray(es);
        return true;
    }
}

final Object[] getArray() {
    return array;
}

final void setArray(Object[] a) {
    array = a;
}
```

写入过程加锁，确保线程安全，然后拷贝原数组，扩容后添加元素，最后替换原数组。

#### 修改元素

```java
public E set(int index, E element) {
    synchronized (lock) {
        Object[] es = getArray();
        E oldValue = elementAt(es, index);

        if (oldValue != element) {
            es = es.clone();
            es[index] = element;
        }
        // Ensure volatile write semantics even when oldvalue == element
        setArray(es);
        return oldValue;
    }
}
```

修改过程加锁，拷贝原数组，修改指定位置的元素，最后替换原数组。

#### 移除元素

```java
public E remove(int index) {
    synchronized (lock) {
        Object[] es = getArray();
        int len = es.length;
        E oldValue = elementAt(es, index);
        int numMoved = len - index - 1;
        Object[] newElements;
        if (numMoved == 0)
            newElements = Arrays.copyOf(es, len - 1);
        else {
            newElements = new Object[len - 1];
            System.arraycopy(es, 0, newElements, 0, index);
            System.arraycopy(es, index + 1, newElements, index,
                                numMoved);
        }
        setArray(newElements);
        return oldValue;
    }
}

static <E> E elementAt(Object[] a, int index) {
    return (E) a[index];
}
```

删除元素时，加锁，拷贝原数组，删除指定位置的元素（判断是否是最后一位），移动后续元素，最后替换原数组

### `instanceof` 的作用

`instanceof` 是 Java 中的一个 **关键字**，用于判断一个对象是否是某个类或其子类、实现类的实例

- 语法格式

```java
@return boolean
对象 instanceof 类名
```

若对象是该类或其子类、接口的实现类的实例，则返回 `true`，否则返回 `false`

- 注意

`instanceof` 的右侧类型必须是 **编译器能确定的父类/接口**，否则会直接编译失败

`null instanceof 类` 永远是 `false`

- 与 `getClass()` 的区别

| 比较项       | `instanceof`                          | `getClass().equals()`          |
| ------------ | ------------------------------------- | ------------------------------ |
| 判断范围     | 当前类、父类、接口实现类都返回 `true` | 仅当前类返回 `true`            |
| 用于多态判断 | 推荐使用                              | 不适合多态场景                 |
| 实例         | `a instanceof Animal`                 | `a.getClass() == Animal.class` |

### HashMap 的添加元素流程

- 步骤：

计算键的 `hashCode()`，通过扰动函数得到 `hash`

定位桶 `index = hash & (length - 1)`

若桶为空，直接插入；若冲突，链表或红黑树处理

若达到扩容阈值（容量 × 负载因子），则进行扩容（2 倍）

### HashMap 扩容加载因子为什么是 0.75

0.75 是效率与空间的平衡点。在低冲突率下保持查询性能，又避免频繁扩容带来的开销。

### HashMap 扩容为什么扩容为数组长度的 2 倍

便于 hash 的重新定位。新桶位置要么是原位置，要么是原位置 + oldCapacity，简化计算逻辑，提高效率。

### HashMap 源码分析

```java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable
```

#### 核心字段

```java
transient Node<K,V>[] table;        // 哈希表数组（桶）
transient int size;                // 实际键值对数量
transient int modCount;           // 修改次数（fail-fast 支持）
int threshold;                     // 扩容阈值
final float loadFactor;           // 负载因子（默认 0.75）
```

#### Node 节点架构

```java
    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }

        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }

        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }

        public final boolean equals(Object o) {
            if (o == this)
                return true;

            return o instanceof Map.Entry<?, ?> e
                    && Objects.equals(key, e.getKey())
                    && Objects.equals(value, e.getValue());
        }
    }

    // Objects.hashCode() 实现
    public static int hashCode(Object o) {
        return o != null ? o.hashCode() : 0;
    }
```

### HashMap 是线程的安全吗

不是。多线程环境下可能出现死循环、数据丢失。并发场景建议用 `ConcurrentHashMap`。

### ConcurrentHashMap 的实现原理 & 源码分析

### 泛型是什么？泛型的好处？类型擦除是什么？

### 反射是什么？反射的好处？反射的使用场景？

### 动态代理有哪些方式？原理是什么？

### JDK 动态代理和 Cglib 动态代理的区别？

### JDK 新特性：虚拟线程的原理了解吗？
