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

`instanceof` 的右侧类型必须是 **编译期能确定的父类/接口**，否则会直接编译失败

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

#### 构造器

```java
    private static final float DEFAULT_LOAD_FACTOR = 0.75f;

    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }

    static final int tableSizeFor(int cap) {
        int n =  -1 >>> Integer.numberOfLeadingZeros(cap - 1);
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }

    public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }

    public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }
```

#### 新增元素

```java
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }

    // onlyIfAbsent 是否只在不存在 key 时才插入（ putIfAbsent() 方法会用到）
    // evict 是否支持淘汰（ LinkedHashMap 中使用）
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        // 如果当前table为空，则进行初始化
        if ((tab = table) == null || (n = tab.length) == 0)
            // 调用 resize() 进行初始化（实际为初始化容量为 16 的数组）
            n = (tab = resize()).length;
        // (n - 1) & hash 使用哈希值和数组长度-1做与运算，快速计算通索引，如果值为null，说明无冲突，直接创建新节点
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

### HashMap 是线程的安全吗

不是。多线程环境下可能出现死循环、数据丢失。并发场景建议用 `ConcurrentHashMap`。

### ConcurrentHashMap 的实现原理

是线程安全的 `Map` ，实现了 `ConcurrentMap` 接口，底层使用了 Node 数组 + 链表/红黑树 + CAS + synchronized 锁控制 i 西安好吃呢甘泉

主要用于多线程环境下替代 `HashMap`

支持高并发读写，且不会像 `HashTable` 一样使用全表锁，仅写操作使用锁，无读锁 `volatile` 保证内存可见

**CAS**（compare and swap）操作：用于插入节点、控制初始化，保证 **原子性**

### 泛型是什么？泛型的好处？类型擦除是什么？

- 泛型是什么

  - 泛型是 Java 中引入的一种 **参数化类型** 机制，它允许在类、接口、方法中使用 **类型参数** ，从而提升程序的 **复用性** 、 **安全性** 与 **可读性**

- 泛型的好处

  - **类型安全**：泛型可以保证 **类型安全** ，编译阶段就能检查类型错误，避免 `ClassCastException` 异常；

  - **消除强制类型转换**：消除 **强制类型转换** ，无需手动强转，提高代码的可读性和安全性；

  - **提高代码复用性**：类、方法、接口都能通过泛型进行复用，不限定具体类型

- 类型擦除是什么

  - Java 泛型的本质是 **编译期语法糖** ，在编译之后，泛型信息会被 **擦除** ，转换为原始类型（通常为 `Object` 或第一个上线类型）即泛型只在编译期有效，运行时并不存在泛型类型

  - 类型擦除的影响：

    - 不能使用基本类型作为泛型类型

    - 泛型类型之间不会有多态

    - 不能通过反射获取泛型类型

    - 不能创建泛型数组

### 反射是什么？反射的好处？反射的使用场景？

- 反射是什么

  - **反射** 是 Java 提供的一种机制，允许程序在 **运行时动态地加载类、获取类的信息、创建对象、访问/修改字段或调用方法**，甚至修改访问权限，主要依赖于 `java.lang.reflect` 包，常用的类包括 `Class<?>`、`Field`、`Method`、`Constructor`

- 反射的好处

  - 运行时动态操作类和对象

  - 提高框架的通用性和灵活性：Java 中大量框架（如 Spring、 MyBatis）通过反射实现对象创建、属性注入等操作

  - 可以打破封装访问私有成员

- 反射的常见使用场景

  - **框架开发**：通过反射获取配置文件、扫描包、创建对象、调用方法

  - **工厂模式/插件开发**：通过反射加载外部插件，并调用其方法

  - **注解解析**：获取注解信息（例如：自定义注解 + 反射解析）

  - **序列化框架**：如 Jackson、Gson 序列化时读取属性名

  - **AOP 动态代理**：JDK 动态代理、CGLib 等都基于反射

  - **测试工具**：JUnit / Mock 框架用反射调用测试方法

### 动态代理有哪些方式？原理是什么？

- 什么是动态代理？

  - 动态代理是指，在运行时 **动态创建代理对象** ， 并在代理对象中对目标对象的方法进行增强或控制

  - 相比于静态代理（手写实现代理类），**动态代理不需要提前编写代理类**，更灵活通用，常用语：AOP（日志、事务、安全控制），框架底层实现（如 MyBatis Mapper）

- 动态代理有哪些方式？

  - **JDK 动态代理**：基于接口的 `Proxy.newProxyInstance()`，目标类必须实现接口

  - **CGLib 动态代理**：通过继承方式使用字节码增强，目标类无需接口，但不能是 `final` 类

  - **Java ASM 字节码增强**：更底层的字节码操控库，高性能框架使用，如 Dubbo

- JDK 动态代理原理是什么？

  - 利用 `java.lang.reflect.Proxy` 创建代理类

  - 使用 `InvocationHandler` 接口处理方法调用

  - 本质：**在内存中动态生成一个实现了目标接口的类，调用目标方法前后插入增强逻辑**

- CGLib 动态代理原理？

  - 使用第三方库 `cglib` ，底层使用 ASM 字节码生成技术

  - 创建一个 **子类对象** 作为代理

  - 对方法进行重写，在重写方法中添加增强逻辑

  - 本质：**继承目标类并重写方法实现增强，非接口也能代理**

### JDK 动态代理和 Cglib 动态代理的区别？

| 特性             | **JDK 动态代理**                   | **CGLib 动态代理**         |
| ---------------- | ---------------------------------- | -------------------------- |
| 是否要求实现接口 | 是                                 | 否                         |
| 原理             | 实现接口 + InvocationHandler       | 继承类 + 字节码增强        |
| 对 final 的限制  | 可代理 final 类                    | 无法代理 final 类或方法    |
| Spring 默认选择  | Spring AOP 默认用 JDK ，否则 CGLib |                            |
| 性能             | JDK 1.8 之后优化，两者接近         | 启动慢，运行快（基于 ASM） |

### JDK 新特性：虚拟线程的原理了解吗？

**虚拟线程** 是由 **JVM 管理的轻量级线程**，每个虚拟线程不再绑定一个 OS 内核线程，可以成千上万地创建，极大提升了高并发处理能力

传统线程：

- Java 中的线程（Platform Thread）是操作系统级线程，创建和上下文切换成本高

虚拟线程：

- 属于用于态线程，由 JVm 协调调度，创建成本极低，堆栈空间占用小，可创建 **百万级线程**

- 创建开销极低；调度机制由 JVM 控制调度（非 OS）；栈默认很小但可拓展；会挂起当前虚拟线程而不会阻塞物理线程；除了启动方式其他和普通线程几乎一致

### String 底层数据类型？

- 在 Java8 及以前，底层是 `char[]` **字符数组**，每个 `char` 占 2 字节，支持完整的 Unicode 但是浪费内存

- Java 9 及以后，引入了 **Compact Strings** 紧凑字符串机制，底层改为使用 `byte[]` + **编码标志** （`coder`），如国字符串只包含 Latin-1 字符（ASCII），就是用单字节编码，否则使用 UTF-16 双字节编码

### String 类能被继承吗？

不能，Java 中的 `String` 类**不能被继承**，因为它被定义为 `final` 类

- 保证**不可变性**：`String` 是不可变对象，一旦创建，其值就不能改变。如果可以继承，就可能被子类重写方法破坏不可变性

- **安全性**：`String` 常被用于类加载器、文件路径、网络地址等，如果能被继承并篡改，将会带来安全问题

- **性能优化**：JVM 对 `String` 做了很多优化（如字符串常量池、编译期常量折叠），以上依赖 `String` 行为的确定性，若允许继承，JVM 无法可靠进行优化

### String 是不可变的，如何实现？

- `String` 类被 `final` 修饰，无法被继承：防止子类通过继承重写方法破坏不可变性

- 成员变量 `value` 是 `final` ，不可重新赋值：无论是 Java8 之前的 `char[]`，还是 Java9 之后的 `byte[]`，都使用 `final` 修饰

- 不提供任何 **修改原始字符内容** 的方法：所有修改的方法实际上都是返回一个新的 `String` 对象，而不是修改原对象

- 安全的**防御性复制**：某些构造器中， `String` 会对传入的字符数组进行**复制**，而不是直接应用

这样做的好处是线程安全、支持字符串常量池、提升性能与安全性。

### String 存储有长度限制吗？

最大长度并没有明确的固定限制，但受到底层数组实现和 JVM 限制的影响，其长度在实际使用中是有上限的。

Java 中数组的最大长度受 `int` 类型的影响，理论上 `String` 最长可以有约 21 亿个字符，实际上可用长度比这个小

### String 为什么设计为不可变的？

- 不可变的体现：`String` 类和内部数据 `char[]`、`byte[]` 都是使用`final`进行修饰，且没有任何方法可以修改已有字符串对象的内容

- 原因：

  - 安全性：`String` 常用语网络连接、文件路径、类加载器等场景，如果可以被修改，攻击者可能通过修改字符串内容篡改程序行为，带来严重安全隐患

  - 支持**字符串常量池**：Java 使用字符串常量池来优化性能与内存使用，相同的字符串字面量只存一份，提高效率，如果 `String` 是可变的，那么多个引用共享一个字符串对象时，任何修改都会影响其他地方

  - 线程安全：不可变对象天然线程安全，不需要加锁控制访问

  - 可靠的 `hashCode()` 缓存：`String` 的 `hashCode()` 会被频繁使用（比如在 `HashMap` 中作为 key），不可变性保证 `hashCode()` 值不会变化，因此可以安全地缓存结果，提升性能

### 字符串常量池是什么？

字符串常量池是 JVM 中的一块特殊内存区域，用来存放：**字符串字面量**、使用 `String.intern()` 方法加入的字符串。当程序创建一个字符串字面量时，JVM 会先检查常量池中是否已有该字符串：有就直接返回常量池中的引用，没有就创建新对象并放入常量池

### String/StringBuilder/StringBuffer 有什么区别？

| 类名          | 是否可变  | 线程安全  | 性能             | 常见用途             |
| ------------- | --------- | --------- | ---------------- | -------------------- |
| String        | ❌ 不可变 | ✅ 安全   | 慢（创建新对象） | 字符串常量，少量拼接 |
| StringBuilder | ✅ 可变   | ❌ 不安全 | 快               | 单线程大量拼接       |
| StringBuffer  | ✅ 可变   | ✅ 安全   | 中等             | 多线程拼接           |

### 深拷贝&浅拷贝是什么，区别？

| 概念   | 说明                                                                               |
| ------ | ---------------------------------------------------------------------------------- |
| 浅拷贝 | 拷贝对象时，只复制对象的**基本属性**，引用类型的字段仍指向**同一内存地址**（共用） |
| 深拷贝 | 拷贝对象时，不仅复制基本属性，还**递归复制引用对象**，使得新旧对象完全独立         |

实现深拷贝的两种方法：

- 手动实现 `clone()` 并递归拷贝应用对象

- 利用序列化实现

### Error 和 Exception 有什么区别？

`Exception` 还可以被程序处理的异常，而 `Error` 是 JVM 层面的问题，通常不能也不应该处理，两者都继承于 `Throwable` 类

| 比较项     | Exception（异常）                  | Error（错误）                        |
| ---------- | ---------------------------------- | ------------------------------------ |
| 所属层次   | 程序级异常（开发者行为）           | 系统级错误（JVM 层面）               |
| 是否可处理 | ✅ 可以 try-catch 或 throws        | ❌ 一般不建议处理                    |
| 常见子类   | NullPointerException、IOException  | OutOfMemoryError、StackOverflowError |
| 是否应处理 | ✅ 必须预料并处理                  | ❌ 通常无法预料和处理                |
| 是否可恢复 | ✅ 通常可恢复                      | ❌ 通常不可恢复                      |
| 使用场景   | 用户输入错误、文件丢失、网络中断等 | 内存溢出、线程死锁、类加载失败等     |

### 异常的分类和区别

| 分类           | 说明                                  | 是否强制处理 |
| -------------- | ------------------------------------- | ------------ |
| **受检异常**   | 编译时必须处理（try-catch 或 throws） | ✅ 是        |
| **非受检异常** | 编译器不会强制处理                    | ❌ 否        |
| **Error**      | 严重错误，JVM 层级，不建议处理        | ❌ 否        |

1. 受检异常：编译器强制要求处理的异常，否则编译抱错，如文件操作、数据库连接、网络通信

2. 非受检异常：编译器不会强制处理，但是运行时会崩溃报错，如空指针、数组越界、除零等

3. Error：程序无法控制、处理不了的错误

### finally 中的代码一定会被执行吗

理论上是一定执行，但实际上存在极少数情况不会执行，如：

1. 调用 `System.exit(0)`

```java
try {
    System.out.println("try");
    System.exit(0); // 退出程序
} finally {
    System.out.println("finally");
}
```

2. 发生了 **虚拟机错误**，如栈溢出、虚拟机栈溢出、方法调用栈溢出等导致 JVM 崩溃

```java
try {
    recurse(); // 死递归
} finally {
    System.out.println("finally"); // 不会执行
}
```

3. finally 代码所在线程被强制终止

```java
Thread t = new Thread(() -> {
    try {
        System.out.println("try");
    } finally {
        // 代码
        System.out.println("finally"); // 不会执行
    }
});
t.start();
t.stop(); // 强制终止线程
```

### AIO、BIO、NIO 分别是什么

是网络编程中常用的三种不同 I/O 模型：

- BIO（Blocking I/O）：阻塞 I/O

  - 每个客户端链接都由一个线程负责处理，当一个线程在进行 I/O 操作时，会**阻塞**直到数据准备好

  - 一请求一线程，资源开销大；编程简单；吞吐能力差，不适合高并发

- NIO（Non-blocking I/O）：非阻塞 I/O

  - 引入了 Channel、Buffer、Selector 等概念，使用单线程处理多个客户端连接（轮询事件）

  - 非阻塞，适合高并发；使用 Selector 可以同时监控多个 Channel；编程复杂，需要管理状态和事件

- AIO（Asynchronous I/O）：异步 I/O

  - 操作系统完成 I/O 操作后主动通知程序（回调方式），无需轮询

  - 真正的异步：读写操作不阻塞；编程模式是回调；适合连接数非常高但数据读写较少的场景

| 特性     | BIO                  | NIO                        | AIO                   |
| -------- | -------------------- | -------------------------- | --------------------- |
| I/O 模型 | 阻塞                 | 非阻塞                     | 异步非阻塞            |
| 编程难度 | 简单                 | 中等                       | 较复杂（回调）        |
| 并发能力 | 差（线程开销大）     | 好（单线程处理多连接）     | 极好（无需轮询）      |
| 适用场景 | 小并发、结构简单系统 | 中等并发，如聊天室、游戏等 | 高并发但低频 I/O 场景 |
| 线程使用 | 一连接一线程         | 单线程/线程池处理多连接    | 回调驱动              |

### 同步与异步如何理解

常用于描述**代码执行流程**、**线程通信方式**或**请求-响应机制**

- 同步（Synchronous）：**执行一个任务时必须等待其完成**，再继续后续操作；阻塞等待结果

- 异步（Asynchronous）：**任务开始后无需等待结果立即执行后续任务**，不阻塞，结果回来后再处理（通过回调等方式）

同步和异步是程序执行时等待任务完成与否的区别。同步意味着调用者要等结果返回再继续执行，常见于阻塞式方法；而异步意味着任务发出后调用者立即返回，结果由回调或通知机制返回，适用于提升并发和响应速度的场景。Java 中如 `Future`、`CompletableFuture` 是异步编程的典型实现，底层可能基于线程池或事件驱动。

### 阻塞与非阻塞如何理解

阻塞（Blocking）与非阻塞（Non-blocking）是**线程与资源交互**时的重要概念，常用于描述程序在进行 I/O、线程通信、资源竞争等操作时，**线程是否需要等待**的行为。

- 阻塞：当线程发起操作（如读/写、获取锁）时，如果资源还没准备好，**线程就会被挂起等待**

- 非阻塞：无论资源是否准备好，**线程都不会被挂起**，立即返回，线程可以继续处理后续业务逻辑

| 对比维度     | 阻塞（Blocking）     | 非阻塞（Non-Blocking）            |
| ------------ | -------------------- | --------------------------------- |
| 是否等待资源 | 是（必须等待）       | 否（立刻返回）                    |
| 线程是否挂起 | 是（线程休眠）       | 否（线程继续运行）                |
| 编程复杂度   | 简单                 | 稍复杂（要处理返回状态）          |
| 适合场景     | 小规模、简单业务逻辑 | 高并发、低延迟、响应快速需求      |
| 示例         | `InputStream.read()` | `SocketChannel.read()` + Selector |

| 类型 | 同步/异步 | 阻塞/非阻塞 | 说明                     |
| ---- | --------- | ----------- | ------------------------ |
| BIO  | 同步      | 阻塞        | 同步阻塞                 |
| NIO  | 同步      | 非阻塞      | 同步非阻塞（需轮询事件） |
| AIO  | 异步      | 非阻塞      | 异步非阻塞（回调驱动）   |

## JUC

### 线程有哪几种状态？状态如何流转？

- `NEW`初始：新建状态。线程对象已创建，但尚未调用 `start()` 启动

- `RUNNABLE`运行：可运行/运行中状态。线程已经启动，可能正在执行，也可能在等待 CPU 调度

- `BLOCKED`阻塞：阻塞状态。线程在等待锁，如 `synchronized` 的锁竞争中

- `WAITING`无限期等待：等待状态。必须被其他线程唤醒（如使用 `wait()`，`join()`，`LockSupport.park()`）

- `TIMED_WAITING`定时等待：计时等待状态。等待一定时间自动唤醒，如 `sleep()`、`wait(time)`等

- `TERMINATED`终止：终止状态。线程执行完毕或发生异常退出

Java 线程有 6 种状态：`NEW`、`RUNNABLE`、`BLOCKED`、`WAITING`、`TIMED_WAITING` 和 `TERMINATED`。线程通过 `start()`从 `NEW` 进入 `RUNNABLE`，运行过程中可能因为锁竞争进入 `BLOCKED`，也可能因调用 `sleep()`、`wait()` 等方法进入等待状态。等条件满足或时间到后线程会重新回到 `RUNNABLE`，最终执行完毕进入 `TERMINATED`。这套状态机制使得 Java 能更精细地控制线程执行与调度。

### 创建线程的方式？

1. 继承 `Thread` 类

```java
public class MyThread extends Thread {
    @Override
    public void run() {
    }
}

MyThread myThread = new MyThread();
myThread.start();
```

2. 实现 `Runnable` 接口

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
    }
}

Thread myThread = new Thread(new MyRunnable());
myThread.start();
```

3. 使用匿名内部类

```java
Thread myThread = new Thread(() -> {
});
myThread.start();
```

4. 使用 `Callable` 接口 + `FutureTask`

```java
import java.util.concurrent.*;

public class MyCallable implements Callable<String> {
    @Override
    public String call() throws Exception {
        return "任务结果：" + Thread.currentThread().getName();
    }
}

FutureTask<String> task = new FutureTask<>(new MyCallable());
Thread myThread = new Thread(task);
myThread.start();

String result = task.get(); // 阻塞获取结果
System.out.println(result);
```

5. 使用线程池创建线程

```java
ExecutorService executor = Executors.newFixedThreadPool(3);
executor.execute(() -> {
    System.out.println("线程池中的任务：" + Thread.currentThread().getName());
});
executor.shutdown();
```

### 多线程有什么应用？

1. 提高程序运行效率，任务并行执行，多线程能利用多核 CPU 并行处理多个任务，减少总耗时

2. 提升程序响应速度，前后端任务分离，主线程负责交互，子线程处理耗时任务，避免阻塞 UI 或主流程

3. 资源密集型任务异步处理，多线程可以隐藏 I/O 等待时间，CPU 在等待时可以调度其他线程

4. 实现定时、周期任务

5. 并发处理大批量任务

| 应用目的     | 具体体现                                         |
| ------------ | ------------------------------------------------ |
| 提高并发能力 | 同时处理多个用户请求或任务                       |
| 提升响应速度 | 主线程快速响应，子线程处理耗时逻辑               |
| 合理利用 CPU | 充分利用多核资源，最大化 CPU 使用率              |
| 解耦系统模块 | 使用异步线程或线程池完成任务解耦，提高系统扩展性 |
| 解决 IO 瓶颈 | 多线程隐藏 IO 等待时间，实现 IO 与计算并行处理   |

### 线程池的好处？

1. 复用线程，降低资源开销：线程执行完任务后不会被销毁而是被复用来执行新的任务。可以避免频繁创建和销毁线程的开销，节省系统资源，尤其是高并发

2. 提升响应速度：任务到达时不必等待新线程的创建，线程池可立即分配已有空闲线程处理，缩短响应时间，提升系统吞吐量

3. 统一管理线程：线程池可统一分配、调度、回收县城，使程序结构更清晰、线程使用更可控，有利于资源的可视化管理和调优

4. 控制最大并发数：可通过配置最大线程数、队列长度等限制系统的并发数量，防止系统被大量并发请求压垮，保护系统稳定性

5. 支持任务调度（定时/延迟）

6. 任务排队（阻塞队列）机制：超过线程池容量的任务可以进入队列排队

7. 支持自定义拒绝策略：当任务过多且线程池与队列都满时，提供 4 种内置拒绝策略 + 自定义机制

8. 可拓展性强：钩子函数、回调，可拓展监控功能、记录日志、异常处理等，适配更复杂场景

线程池通过复用线程、限制最大并发、任务排队、拒绝策略等机制，降低系统资源开销、提升任务处理效率，是高并发环境下稳定性和性能的重要保障。

### 线程池的七个参数？

```java
public ThreadPoolExecutor(
    int corePoolSize,                  // 1. 核心线程数
    int maximumPoolSize,               // 2. 最大线程数
    long keepAliveTime,                // 3. 空闲线程最大存活时间
    TimeUnit unit,                     // 4. 存活时间单位
    BlockingQueue<Runnable> workQueue,// 5. 任务队列
    ThreadFactory threadFactory,       // 6. 线程工厂
    RejectedExecutionHandler handler   // 7. 拒绝策略
)
```

- `corePoolSize`：核心线程数，线程池中始终保留的线程数，即使空闲也不回收

- `maximumPoolSize`：最大线程数（= 核心 + 非核心线程数），队列满时才会新增非核心线程

- `keepAliveTime`：非核心线程的最大空闲时间，超过这个时间会被回收

- `unit`：空闲时间单位

- `workQueue`：任务队列，保存等待执行的任务

- `threadFactory`：线程工厂，指定线程创建方式

- `handler`：拒绝策略，任务无法处理时的处理方式

Java 线程池有七个参数，分别是：**核心线程数**、**最大线程数**、**线程存活时间**、**时间单位**、**任务队列**、**线程工厂**、**拒绝策略**，它们共同决定了线程池的行为和性能表现。

### 为什么不推荐使用 JDK 的 Executors 创建线程池？

不推荐使用 `Executors` 创建线程池，是因为它默认使用`无界队列`或`无界线程数`，可能导致 `内存溢出`或`系统崩溃`。应使用 `ThreadPoolExecutor` 显式设置参数以确保系统稳定性和可控性。

### 线程池的执行流程？

1. 任务提交：

   - 使用 `executor.execute(Runnable task)` 或 `submit()` 提交任务

2. 判断当前线程数是否小于核心线程数：

   - 如果小于，则直接创建新线程执行任务

   - 否则，尝试将任务加入工作队列

3. 判断工作队列是否已满：

   - 如果已满，则创建新的线程执行任务，直到线程数达到最大线程数

   - 如果未满，任务则会被缓存，等待空闲线程来处理

4. 如果线程数已达到上限 & 队列已满

   - 启动拒绝策略（`RejectedExecutionHandler`）处理任务，默认策略是抛异常

核心线程不够就加线程，队列不满就排队，排满再判断能不能加非核心线程，全满就走拒绝策略。

### 任务拒绝策略有哪些，怎么选择？

| 策略类名              | 说明                                         | 使用建议                         |
| --------------------- | -------------------------------------------- | -------------------------------- |
| `AbortPolicy`（默认） | 抛出 `RejectedExecutionException` 异常       | **默认策略，适合开发调试阶段**   |
| `CallerRunsPolicy`    | 由**调用线程**（提交任务的线程）执行该任务   | **资源紧张时牺牲响应速度保活性** |
| `DiscardPolicy`       | **直接丢弃任务**，不会抛出异常               | 容忍部分任务丢失、非核心任务     |
| `DiscardOldestPolicy` | 丢弃**最早的一个任务**，然后重新提交当前任务 | 实时性要求高、优先处理新任务     |

或者自定义拒绝策略

JDK 提供了四种拒绝策略：`AbortPolicy`（抛异常）、`CallerRunsPolicy`（调用线程执行）、`DiscardPolicy`（直接丢弃）、`DiscardOldestPolicy`（丢弃最老任务），选择时要根据业务的可靠性、实时性和资源承受能力来权衡。

### 线程池的核心线程数和最大线程数怎么指定？

- `corePoolSize`核心线程数：线程池中始终保留的线程数，即使空闲也不会被回收

- `maximumPoolSize`最大线程数：线程池能容纳的最大线程数量（= 核心线程 + 临时线程）

**核心线程数设置**：

一般公式：`corePoolSize = CPU 核心数 * 任务类型系数`

根据任务类型

| 任务类型   | 推荐设置                    | 说明                                |
| ---------- | --------------------------- | ----------------------------------- |
| CPU 密集型 | `CPU核心数 + 1`             | 避免 CPU 空转                       |
| IO 密集型  | `CPU核心数 × 2` 或更多      | IO 等待多，线程可以多一些提高并发性 |
| 混合型任务 | 结合 CPU 和 IO 任务综合估算 | 通常以 IO 占比为主调整              |

**最大线程数设置**：

一般公式：`maximumPoolSize = corePoolSize * 2~4`

考虑因素：

- 系统允许的最大并发量（避免 OOM）

- 队列大小是否有限（如`LinkedBlockingQueue`是无界的就不推荐设置过大）

- 响应时效 vs 资源占用权衡

核心线程数一般设置为 CPU 核心数，IO 密集型任务可设置为其 2 倍以上，最大线程数应根据系统负载、队列容量和任务特性综合设定，通常为核心线程数的 2~4 倍。

### 上线后会关注线程池的哪些指标

1. 线程池中的当前线程数`getPoolSize()`

2. 核心线程数`getCorePoolSize()`

3. 最大线程数`getMaximumPoolSize()`

4. 正在执行任务的线程数`getActiveCount()`

5. 已完成的任务总数`getCompletedTaskCount()`

6. 任务总数（包括等待中）`getTaskCount()`

7. 阻塞队列中的等待任务数`getQueue().size()`

8. 线程池是否已关闭`isShutdown() / isTerminated()`

### 怎么理解线程安全？

线程安全是指：

当多个线程同时访问某个共享资源（变量、对象、方法）时，无论运行环境采用什么样的调度方式或者这些线程如何交替执行，都能保证程序的**正确性**、**一致性**、**不出错**。

### synchronized 的原理实现是什么？

`synchronized` 的底层是基于 **对象头的 Mark Word** 实现的。每个 Java 对象在内存中都有一个对象头，其中 Mark Word 存储了锁的信息

`synchronized` 是 Java 提供的一种内置锁机制，它通过在 JVM 中使用 `monitorenter` 和 `monitorexit` 指令实现线程同步。底层是基于对象头的 Mark Word 来存储锁状态，支持偏向锁、轻量级锁、重量级锁等多种锁优化机制，以提高性能。

### synchronized 锁的升级过程？

synchronized 的锁在 JVM 中会根据**线程竞争情况**自动从轻到重进行升级，目的是提升性能和减少不必要的阻塞。这个过程叫做**锁的升级过程**

无锁 → 偏向锁 → 轻量级锁 → 重量级锁（synchronized）

锁只会升级，不会降级，即锁一旦升级为重锁，就不会再回退

- 无锁：对象初始状态，没有任何线程竞争，执行速度最快，适用于无并发的场景

- 偏向锁：假设**锁始终由同一线程持有**，第一次加锁时会把持有线程的 ID 写入对象头，后续加锁只需比对线程 ID，无需 **CAS** 操作

- 轻量级锁：多线程访问，但未发生真正的锁冲突，会尝试通过 CAS 竞争锁，如果成功，无需阻塞线程，性能更高

- 重量级锁：多线程竞争严重，CAS 失败，JVM 采用 `monitor` 机制，**线程会被阻塞、挂起**

| 当前状态 | 触发条件                     | 升级为                   |
| -------- | ---------------------------- | ------------------------ |
| 无锁     | 第一次线程进入同步代码块     | 偏向锁                   |
| 偏向锁   | 另一个线程也试图获取该锁     | 撤销偏向，升级为轻量级锁 |
| 轻量级锁 | 多线程同时竞争，CAS 失败频繁 | 重量级锁                 |

`synchronized` 锁的升级顺序为：无锁 → 偏向锁 → 轻量级锁 → 重量级锁。JVM 会根据线程竞争情况自动切换锁的实现策略，以追求性能最优。偏向锁适用于单线程访问，轻量级锁适用于低竞争，重锁用于高竞争场景。这一过程充分体现了 Java 对并发性能的优化能力。

### CAS 是什么？应用场景？优缺点？

CAS（**Compare And Swap**，比较并交换）是一种无锁（Lock-Free）编程的核心原子操作，用于多线程并发情况下保证数据一致性。

CAS 是一种原子操作，包含 3 个参数：`CAS(V, E, N)`

- `V`：内存位置（当前变量的值）

- `E`：预期值（期望变量值）

- `N`：新值（要修改成的新值）

如果 `V == E`，说明没有其他线程修改过这个值 -> 把 `V` 设为 `N` -> 操作成功

如果 `V!= E`，说明其他线程修改过这个值 -> 操作失败

CAS 操作是原子的，它是由 CPU 指令集保证的，因此是一种高效且可靠的同步机制。

CAS（Compare And Swap）是一种无锁的原子操作，它在并发场景中广泛应用于乐观锁设计，是 `java.util.concurrent` 包中高性能并发类的基础。CAS 的优势是效率高、无阻塞，但也存在 ABA 问题、自旋耗 CPU、不能直接操作多个变量等缺点。

### ReentrantLock 是什么，和 synchronized 相比的区别是什么？

`ReentrantLock` 是 Java 并发包（`java.util.concurrent.locks`）中提供的一个**可重入的显式锁**，功能比 `synchronized` 更强大和灵活。

`ReentrantLock（可重入锁）`：指同一个线程可以重复获取同一个锁，而不会被阻塞。

属于 **显示锁**，需要手动 `lock()` 和 `unlock()`。

位于 `java.util.concurrent.locks`包中。

`ReentrantLock` 是一个可重入的显式锁，相比 `synchronized`，它支持中断、限时等待、公平锁、多个条件队列等特性，使用更灵活。在高并发、复杂同步控制场景下推荐使用，但使用时需注意手动释放锁，避免死锁风险。

### AQS 是什么？怎么实现的？

`AQS` 全称是 `AbstractQueuedSynchronizer` 抽象队列同步器，是 Java 并发包（`java.util.concurrent`）中的一个基础框架，用于实现依赖先进先出等待队列的同步器，帮助开发者快速构建自定义同步工具

`AQS` 是一个用于构建锁和同步器的抽象类，内部维护一个基于 `CLH` 的 `FIFO` 队列，并通过一个 volatile 状态变量 state 记录同步状态。它支持独占和共享两种锁模式，常见的 `ReentrantLock`、`Semaphore`、`CountDownLatch` 都是基于它实现的。我们只需要重写核心的 `tryAcquire` / `tryRelease` 等方法，就能快速实现自定义同步器。

### 乐观锁是什么？悲观锁是什么？应用场景分别是什么？

**乐观锁**：假设并发冲突很少发生，不加锁，操作时先读取版本号，提交时检查是否被修改，每次操作都认为不会有其他线程同时操作，只有在更新数据时通过 **版本号或时间戳校验**

**悲观锁**：假设并发冲突一定会发生，所以每次操作都要加锁，避免冲突；在访问资源前，先**加锁**，其它线程就不能访问这个资源，读写之间互斥，保证数据一致性，但性能较低

| 方面      | 悲观锁                           | 乐观锁                         |
| --------- | -------------------------------- | ------------------------------ |
| 并发策略  | 保守，假设冲突一定发生           | 乐观，假设冲突极少发生         |
| 实现方式  | 加锁机制（阻塞其他线程）         | 无锁机制 + 版本控制（`CAS`）   |
| 性能开销  | 较大（阻塞、上下文切换）         | 较小（高并发下效率更高）       |
| 适用场景  | 写多读少，冲突概率高             | 读多写少，冲突概率低           |
| Java 代表 | `synchronized` / `ReentrantLock` | `Atomic` 类、`CAS`、版本字段等 |

- 应用场景：

  - 乐观锁适合场景：

    - **读多写少**，冲突概率很低的业务；不要求强一致性，但希望性能更高的系统；前后端协作更新

  - 悲观锁适合场景：

    - **并发冲突频繁**，数据一致性要求高的系统；数据库层中进行行级加锁时常用

乐观锁和悲观锁主要是应对并发场景下的数据一致性问题。悲观锁假设冲突一定发生，所以通过加锁机制来保证同步，比如 `synchronized` 和数据库的行级锁。而乐观锁假设冲突很少，用版本号或 `CAS` 实现非阻塞控制，性能更好。一般来说，读多写少场景用乐观锁，写多且冲突多的场景用悲观锁。

### 用数据库如何实现乐观锁？

使用版本号（时间戳）实现乐观锁：

1. 数据表中添加一个 `version` 字段，表示数据版本

2. 查询数据时获取当前 `version` 值

3. 更新数据时，将 `WHERE` 条件带上 `version`

4. 如果 `version` 没有变，说明数据未被其他事务修改，更新成功

5. 如果 `version` 变了，说明被其他事务修改，更新失败

### JMM 是什么？其中原子性、可见性、有序性问题分别是什么？

JMM（`Java Memory Model`，**Java 内存模型**）是 Java 虚拟机中定义的一种 **多线程并发访问共享变量的行为规范**，它描述了：

- 线程与主内容（共享内存）之间的交互规则；

- 变量在内存中的可见性

- 如何实现有序性与并发的正确性保障

**JMM 的核心：主内存 & 工作内存模型**

Java 中每个线程都有自己的**工作内存**（工作副本），主内存是共享的

- 主内存：存储所有共享变量

- 工作内存：线程私有的，用于存储主内存中变量的副本

线程操作变量时：先从主内存读取到工作内存；在工作内存中进行计算和操作；最后同步回主内存

### JMM 如何保证的原子性、可见性、有序性？

**JMM 三大特性**：原子性、可见性、有序性

- **原子性**：一个操作要么全部执行，要么全部不执行，中间不会被线程切换打断

  - 使用 `synchronized`；使用 `Lock`；使用原子类 `AtomicInteger`、 `AtomicLong`（底层`CAS`）

- **可见性**：一个线程修改了共享变量的值，其他线程是否能立即看到变化

  - `synchronized`释放锁会将修改同步到主内存；`volatile`对变量的读写直接操作主内存；使用`ReentrantLock` + `volatile` 也可以保证可见性

- **有序性**：编译器和处理器为优化性能，可能会**对代码指令重排序**（前提是单线程语义不变），但在多线程环境中就可能出错

  - `volatile`可以禁止指令重排序（内存屏障）;`synchronized`：内存屏障+互斥同步，天然有序性；使用`happens-before`原则（JMM 定义）

`happens-before` 原则：

- 程序顺序规则：一个线程中，代码的执行顺序按照代码顺序。

- 监视器锁规则：解锁 `unlock` 先于加锁 `lock`。

- `volatile` 变量规则：写 `volatile` 变量先于后续的读操作。

- 线程启动规则：`Thread.start()` 先于线程中代码执行。

- 线程终止规则：线程执行完毕的变量修改对 `join()` 可见。

- 线程中断规则：调用 `interrupt()` 先于检测 `isInterrupted()`。

- 对象构造规则：对象构造完成才可以看到其引用。

- 传递性：`A happens-before B，B happens-before C ⇒ A happens-before C`。

### ThreadLocal 是什么？有什么作用？

`ThreadLocal` 是 Java 提供的一种线程本地存储工具，它让 **每个线程都拥有自己的独立变量副本**，互不干扰，用来解决多线程中 **变量隔离问题** 设计的，每个线程都可以通过 `ThreadLocal.set()` 设置自己的变量副本，其他线程无法访问

工作原理：每个线程内部维护一个 `ThreadLocalMap` ，它的 key 是`ThreadLocal`实例，value 是对应的线程副本变量，每个线程有且仅有一个 `ThreadLocalMap`，每个 `ThreadLocal` 实例在该线程中作为 key 存储值，因此变量是**线程私有**的

核心方法：

```java
// 设置线程本地变量
threadLocal.set(value);

// 获取线程本地变量
T value = threadLocal.get();

// 移除当前线程对应的变量
threadLocal.remove();
```

### 算法：多线程交替打印 0-100

1. 使用 `synchronized + wait + notify` 交替等待唤醒

```java
public class AtomicPrinter {
        private static int number = 0;
    private static final int MAX = 100;
    private static final Object lock = new Object();

    public static void main(String[] args) {
        Runnable printer = () -> {
            while (true) {
                synchronized (lock) {
                    lock.notify();
                    if (number <= MAX)
                        System.out.println(Thread.currentThread().getName() + "->" + number++);
                    else break;
                    try {
                        lock.wait();
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
        };

        new Thread(printer, "T1").start();
        new Thread(printer, "T2").start();
    }
}
```

2. 使用 `Lock + Condition` 控制

```java
public class AtomicPrinter {
    private static int number = 0;
    private static final int MAX = 100;
    private static final Lock lock = new ReentrantLock();
    private static final Condition condition = lock.newCondition();

    public static void main(String[] args) {
        Runnable printer = () -> {
            while (true) {
                lock.lock();
                try {
                    condition.signal();
                    if (number <= MAX)
                        System.out.println(Thread.currentThread().getName() + "->" + number++);
                    else
                        break;
                    condition.await();
                }catch (Exception e){
                    e.printStackTrace();
                }finally {
                    lock.unlock();
                }
            }
        };

        new Thread(printer, "T1").start();
        new Thread(printer, "T2").start();
    }
}
```

### 算法：多线程交替打印 ABC

1. 使用 `synchronized + wait + notifyAll` 交替等待唤醒

```java
public class AtomicPrinter {
        private static final int LOOP = 10;
    private static int state = 0;
    private static final Object lock = new Object();

    public static void main(String[] args) {
        Runnable printerA = () -> print("A", 0);
        Runnable printerB = () -> print("B", 1);
        Runnable printerC = () -> print("C", 2);

        new Thread(printerA).start();
        new Thread(printerB).start();
        new Thread(printerC).start();
    }

    private static void print(String name, int targetStats) {
        for (int i = 0; i < LOOP; ) {
            synchronized (lock) {
                while (state % 3 != targetStats) {
                    try {
                        lock.wait();
                    } catch (Exception e) {
                        Thread.currentThread().interrupt();
                    }
                }
                System.out.println(name);
                state++;
                i++;
                lock.notifyAll();
            }
        }
    }
}
```

2. 使用 `Lock + Condition` 控制

```java
public class AtomicPrinter {
        private static final int LOOP = 10;
    private static final Lock lock = new ReentrantLock();
    private static final Condition aCondition = lock.newCondition();
    private static final Condition bCondition = lock.newCondition();
    private static final Condition cCondition = lock.newCondition();
    private static int state;

    public static void main(String[] args) {
        new Thread(() -> print("A", 0, aCondition, bCondition)).start();
        new Thread(() -> print("B", 1, bCondition, cCondition)).start();
        new Thread(() -> print("C", 2, cCondition, aCondition)).start();
    }

    private static void print(String name, int targetStats, Condition self, Condition next) {
        for (int i = 0; i < LOOP; i++) {
            lock.lock();
            try {
                while (state % 3 != targetStats)
                    self.await();
                System.out.println(name);
                state++;
                next.signal();
            } catch (Exception e) {
                Thread.currentThread().interrupt();
            } finally {
                lock.unlock();
            }
        }
    }
}
```

### 多线程的上下切换是什么？

**线程上下文切换**就是操作系统**保存当前线程状态**，然后**恢复另一个线程状态**的过程

线程上下文 = 线程运行所需的全部状态信息，包括：

- 程序计数器（PC，下一条指令地址）

- CPU 寄存器

- 栈指针

- 内核堆栈

- 内存映射

- 调度信息（如优先级、状态）

### 什么时候会发生上下文切换？

| 场景                     | 描述                                                      |
| ------------------------ | --------------------------------------------------------- |
| **时间片用完**           | 一个线程运行完分配的时间片，系统调度另一个线程            |
| **线程阻塞**             | I/O、等待锁、sleep 等操作导致线程挂起，系统切换到别的线程 |
| **线程优先级更高的就绪** | 比当前线程更高优先级的线程准备好了，触发切换              |
| **多核负载均衡**         | 操作系统将线程从一个 CPU 核心迁移到另一个                 |

### 如何减少上下文切换？

上下文切换代价高：

1. 保存和恢复寄存器状态

2. 刷新 CPU 缓存（Cache Miss）

3. TLB 刷新（地址映射表）

4. CPU 管理实践增加（调度时间）

如何减少切换：

| 方法                 | 说明                                                   |
| -------------------- | ------------------------------------------------------ |
| 限制线程数量         | 使用线程池，避免创建过多线程                           |
| 使用协程（虚拟线程） | 轻量线程切换成本极低（如 Java 虚拟线程 / Golang 协程） |
| 减少共享资源竞争     | 尽量无锁编程，或减少锁粒度、使用 `CAS` 等              |
| 使用合适的并发模型   | 如 `Disruptor`、消息队列异步处理等                     |

### 原子类的定义、原理、适用场景？

**原子类**（Atomic Classes）是 Java 提供的一组用于实现原子性操作的工具类，主要依赖底层的 `CAS` 机制实现

| 类型               | 类名                           | 说明                         |
| ------------------ | ------------------------------ | ---------------------------- |
| 原子基本类型       | `AtomicInteger`、`AtomicLong`  | 原子地更新 int / long 值     |
| 原子引用类型       | `AtomicReference<T>`           | 原子地更新对象引用           |
| 原子数组类型       | `AtomicIntegerArray` 等        | 原子操作数组元素             |
| 原子对象字段更新器 | `AtomicIntegerFieldUpdater`    | 原子地更新对象中的某个字段   |
| 高级累加器         | `LongAdder`、`LongAccumulator` | 高并发场景下优化性能的累加器 |

核心实现原理是：`CAS` + `volatile` + `Unsafe`

- `CAS`：CAS 是一种乐观锁思想，通过比较内存中的值是否等于预期值，来决定是否更新

- `volatile`：修饰原子类内部的变量，保证多线程之间变量的内存可见性

- `Unsafe`：底层使用 `sun.misc.Unsafe` 直接操作内存进行原子性操作

优缺点：

- 优点：

  - **无锁并发**：线程安全但不使用 `synchronized`，优化性能

  - **低开销**：避免上下文切换和阻塞

  - **底层硬件级别原子操作**：CPU 提供的原子指令支持

- 缺点：

  - **操作不复合**：仅适用于单变量原子操作

  - **自旋失败会重试**：高并发冲突严重时，CAS 重试消耗较高

  - **不易拓展维护**：业务逻辑复杂时代码可读性差

### Unsafe 类是什么？

`Unsafe` 类是 Java 提供的一个底层工具类，全名为：`sun.misc.Unsafe`，它提供了操作内存、线程调度、`CAS` 操作等一系列底层方法，可以实现许多高性能组件的核心逻辑，是 Java 并发编程的基石之一。

该类允许我们：

- 直接操作内存

- 实现底层 CAS 操作

- 操作对象字段的偏移

- 控制线程挂起与恢复

- 实现对象实例的无参构造

**官方默认不推荐直接使用**，只能通过反射获取其实例
