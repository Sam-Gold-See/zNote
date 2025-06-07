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

## MySQL

### 事务的四大特性是什么？实现原理是什么？

- 事务的四大特性（**ACID**）

  - **原子性**（`Atomicity`）：事务中的所有操作要么全部成功，要么全部失败，不可分割

  - **一致性**（`Consistency`）：事务前后数据库的数据必须处于一致状态，满足数据完整性约束

  - **隔离性**（`Isolation`）：多个事务之间相互隔离，一个事务不应该看到另一个未提交事务的中间状态

  - **持久性**（`Durability`）：一旦事务提交，其结果永久保存在数据库中，即使系统宕机也不丢失

- 事务实现原理

  - 原子性实现原理：依靠日志（`Undo Log`）实现

    - 在执行前记录“回滚日志”，失败时读取 Undo Log 回滚操作

    - 所有操作都在一个逻辑单元内完成，否则全部撤销

    - MySQL InnoDB 引擎通过 `Undo Log + 回滚机制` 保证原子性

  - 一致性实现原理：依赖原子性 + 隔离性 + 数据完整性约束（主键、外键、唯一约束）等共同保障

    - 原子性保证事务内部完整

    - 隔离性保证不被其他事务干扰

    - 约束条件由数据库自身维护

  - 隔离性实现原理：依靠多版本并发控制（MVCC） + 锁机制实现

    | 隔离级别                     | 说明                         | 可能问题                 |
    | ---------------------------- | ---------------------------- | ------------------------ |
    | 读未提交（Read Uncommitted） | 可以读取未提交的数据         | 脏读                     |
    | 读已提交（Read Committed）   | 只能读已提交数据             | 不可重复读               |
    | 可重复读（Repeatable Read）  | 同一事务中读到的数据始终一致 | 幻读（可通过间隙锁避免） |
    | 串行化（Serializable）       | 全加锁，串行执行事务         | 性能最低，隔离最高       |

  - 持久性实现原理：依靠日志（Redo Log） + 刷盘机制实现

    - 事务提交前，先将修改操作写入 Redo Log

    - 写入成功后，才真正提交，系统崩溃后可以通过 Redo Log 恢复

    - MySQL InnoDB 使用 `Redo Log`（重做日志）实现事务的持久性

### 并发事务有什么问题？分别解释一下？

会引发**数据一致性**问题，主要有以下 4 种并发问题

1. **脏读**：一个事务读到了另一个未提交事务修改的数据

2. **不可重复读**：在同一事务中，两次读取同一行数据结果不同，因为另一事务中间修改了数据并提交

3. **幻读**：一个事务中两次执行相同的查询语句，结果集缺不一致（**新增或删除了行**）

   - 使用 可重复读 + 间隙锁 避免幻读

   - 使用串行化

4. **第一类丢失更新**：两个事物都读取了同一个值，经过修改前后相继提交，后提交事务覆盖了前一个事务的更新

   - 使用锁机制防止并发修改

   - 使用乐观锁（版本号/时间戳）控制并发更新

### 事务的隔离级别有哪些？实现原理是什么？

- **读未提交**

  - 所有事务都可以读取其他事务未提交的数据；

  - 实现方式：不加锁，直接读取最新数据；

  - 风险：会发生 **脏读** ，一致性最差。

- **读已提交**

  - 每次查询只能读取其他事务已提交的数据；

  - 实现方式：快照读（使用 MVCC）；

  - 仍可能发生 **不可重复读** 和 **幻读**；

  - Oracle 默认使用该隔离级别。

- **可重复读**

  - 一个事务中多次读取同一数据，结果一致；

  - 实现方式：MVCC + 间隙锁（Gap Lock）；

  - InnoDB 中通过 **快照版本号** 保证重复读一致；

  - 幻读也可通过加锁机制防止（如 `select ... for update`）。

- **串行化**

  - 所有事务串行执行（加表级锁或行级锁）；

  - 最严格，**避免所有并发问题**；

  - 实现方式：

    - 行锁 + 共享锁/排它锁；

    - 或通过乐观锁/序列号控制；

  - 性能最差，不常用于高并发场景。

设置隔离级别（只对当前 session 生效）

```sql
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

### 如何理解 MVCC？

- **MVCC**（Multiversion Concurrency Control）：多版本并发控制，是数据库实现 **高并发读写**、同时又能 **保证数据一致性** 的一种重要机制

- 每个事务读取的是“**某一时刻的快照数据**”，而不是加锁的实时数据。这允许 **读写并发进行**，避免了加锁带来的性能瓶颈。

### 为什么使用 B+树作为索引的数据结构？为什么不用哈希表或二叉树？

数据库（特别是关系型数据库如 MySQL）**磁盘 I/O 访问的代价远高于内存计算**，因此选择索引的数据结构不仅要关注**时间复杂度**，还必须考虑**磁盘存储效率与访问效率**。

- **B+树**优点：

  - 减少磁盘 I/O：树的高度低

    - B+树是**多路平衡查找树**，每个非叶子节点可有 **上百上千个子节点**，因此 B+ 树高度非常低，减少了磁盘 I/O 次数。

  - B+树所有数据存在叶子节点，支持区间查询、范围查询

    - 所有数据只在 **叶子节点** 中；各叶子节点通过 **链表** 链接，天然支持 **范围查询**（`BETWEEN`，`ORDER BY`，`GROUP BY`），对于扫描一段数据，只需一次定位起点，接着顺序遍历即可。

  - 结构稳定、查询效率稳定

    - B+ 树的结构更平衡，所有 key 的查找路径长度一致，**查询效率稳定**

- 哈希表缺点：

| 缺点             | 说明                                                   |
| ---------------- | ------------------------------------------------------ |
| 无法进行范围查询 | 哈希表是无序的，不能处理 `WHERE age > 30` 类的区间查询 |
| 哈希冲突处理复杂 | 冲突越多，效率越低，不稳定                             |
| 空间利用率差     | 哈希表通常预留较大空间，浪费严重                       |
| 不支持排序       | 无法用于 `ORDER BY`、`GROUP BY`                        |

- 二叉树缺点：

| 缺点                   | 说明                                                       |
| ---------------------- | ---------------------------------------------------------- |
| 高度高                 | 即使是平衡树，百万数据也要十几层，导致磁盘 I/O 次数多      |
| 节点分布不适合磁盘存储 | 二叉树每个节点只存一个 key，不能充分利用磁盘页大小         |
| 不支持顺序遍历         | 虽然中序遍历有序，但没有链表指针，不如 B+ 树高效           |
| I/O 不友好             | 每访问一个节点就是一次磁盘读取，效率远低于 B+ 树页缓存策略 |

### B 树和 B+树有什么区别？

| 特性                       | B 树             | B+ 树                                  |
| -------------------------- | ---------------- | -------------------------------------- |
| **数据存储位置**           | 数据存在所有节点 | **数据只存在叶子节点**，内节点只存索引 |
| **叶子节点之间是否有指针** | 无               | 有，双向/单向链表                      |
| **范围/区间查询**          | 支持，但效率低   | 非常高效（链表 + 顺序遍历）            |
| **内存命中效率**           | 一般（分布式）   | 更高（索引集中）                       |
| **访问路径是否固定**       | 取决于位置       | 所有数据查找路径等长                   |
| **磁盘读取效率**           | 较低（分散）     | 更高（节点大、扇出高）                 |
| **树的高度**               | 相对较高         | 更矮、更扁平                           |
| **数据库使用场景**         | 理论可用         | 实际数据库索引几乎都使用 B+ 树         |

### 什么是聚集索引和二级索引？

**聚集索引** ：数据本身存在索引里（叶子节点存整行），InnoDB 数据页默认是聚集索引组织

**二级索引** ：叶子节点只存索引字段和主键，查整行需回表

### 回表是什么？怎么消除回表？索引覆盖是什么？

回表是指：在通过**二级索引（非聚集索引）**查询时，无法获得整行数据，需要通过索引中保存的主键值，回到聚集索引中再次查询完整数据的过程

回表会导致：**多次访问磁盘**（一次查索引页 + 一次查数据页）；对**IO 敏感**，尤其是当结果集很大时

- 消除回表：

  - 使用覆盖索引（select 索引字段）：尽量在查询中只选取**索引中已有的字段**

  - 创建联合索引：把查询中常用的字段一起放进索引

索引覆盖是指：查询所需要的字段，都可以**从某个索引中获取**，不需要回表

```sql
CREATE TABLE user (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  age INT,
  address VARCHAR(100)
);
CREATE INDEX idx_name ON user(name);
```

对于该表，执行以下 SQL，由于 `name` 是索引字段，查询字段也只包含 `name`，所以该数据无需回表，可以完全从索引 `idx_name` 中获取

```sql
SELECT name FROM user WHERE name = '张三';
```

### 索引失效的情况有哪些？

索引失效会导致**全表扫描（全索引扫描）**

索引失效的常见情况：

| 场景                         | 是否会失效 | 优化建议                       |
| ---------------------------- | ---------- | ------------------------------ |
| 函数、表达式处理索引字段     | 是         | 将函数移除，处理完再查询       |
| 隐式类型转换                 | 是         | 保持查询条件类型与字段一致     |
| LIKE 前缀 `%` 查询           | 是         | 使用后缀通配 `LIKE 'xxx%'`     |
| OR 条件混用索引和非索引字段  | 是         | 替换为 UNION，或加上索引       |
| 复合索引未使用最左前缀       | 是         | 遵循最左匹配原则               |
| !=、NOT IN、NOT EXISTS       | 是         | 尽量使用 IN、EXISTS 替代       |
| 查询返回数据量太大           | 是         | 优化 where 过滤条件            |
| 使用范围条件后再用其他索引列 | 部分失效   | 范围条件后续列可能不再使用索引 |

### 分别解释一下 RedoLog、UndoLog、BinLog？应用场景？

- **RedoLog**（重做日志）

  - 是 **InnoDB 引擎**特有的日志，用于保证事务的持久性

  - 记录了**对磁盘数据页的物理修改操作（逻辑变更后，页的二进制变化）**

  - 应用场景：崩溃恢复（系统宕机后，通过 Redo Log 重做未刷盘的数据页，**保证事务提交后的数据不会丢失**）

- **UndoLog**（回滚日志）

  - Undo Log 适用于 **事务回滚**，即数据恢复到修改前的状态

  - 记录了**数据被修改前的值**

  - 应用场景：事务回滚、MVCC 实现（多版本并发控制）、一致性读

- **BinLog**（归档日志）

  - Bin Log 是 **MySQL Server 层**的日志，所有引擎通用

  - 记录了**所有对数据库产生变更的 SQL 语句或其他事件格式**

  - 应用场景：主从复制（数据同步）、数据恢复

### 为什么 RedoLog 使用 WAL 技术？

- **WAL** 是一种数据库常用的日志机制，核心思想是：**先写日志再写数据**

  - 在修改数据库中的实际数据页签，先将修改操作记录到日志（**RedoLog**）中

  - 日志写入成功后，才真正更新内存中的数据页

  - 后续再将数据页**延迟刷盘**，提升性能；

  - 如果数据库异常崩溃了，通过日志可以 **重放日志来恢复数据**

- 使用 WAL 的优势：

  - 确保事务持久性（Durability）

    - Redo Log 是持久化的

    - 使用 WAL，事务提交时只需将日志写入磁盘，数据页可以晚些刷

    - 保证：**一旦事务提交，无论是否宕机，数据都不会丢失**

  - 提高性能：顺序写日志，避免频繁刷盘

    - 数据页更新是 **随机写**，而 Redo Log 是 **顺序写磁盘**，速度更快

    - 减少了每次修改都要更新磁盘页的 IO 操作

  - 支持崩溃恢复机制（Crash Recovery）

    - 通过 WAL， InnoDB 在崩溃后：

      - 重做 Redo Log 中的操作

      - 恢复事务提交后的数据，保证一致性

- 流程：

  1. 用户修改数据

  2. 写入 Undo Log（用于回滚）

  3. 写入 Redo Log（prepare 阶段） ← 先写日志（WAL）

  4. 写入 Binlog

  5. Redo Log 提交（commit）

  6. 事务提交成功

  7. 后台刷盘数据页（可延迟）

### BinLog 的三种格式？

| 格式类型      | 是否记录 SQL | 是否记录行数据 | 是否安全复制 | 日志大小 | 是否易读 |
| ------------- | ------------ | -------------- | ------------ | -------- | -------- |
| **Statement** | ✅ 是        | ❌ 否          | ❌ 不一定    | ✅ 小    | ✅ 易读  |
| **Row**       | ❌ 否        | ✅ 是          | ✅ 是        | ❌ 大    | ❌ 难读  |
| **Mixed**     | ✅/❌        | ✅/❌          | ✅ 是        | 适中     | ❌ 中等  |

一般建议在 **主从同步环境中使用 Row 或 Mixed 模式**，避免数据不一致风险

可以使用

```sql
SHOW VARIABLES LIKE 'binlog_format';
SET GLOBAL binlog_format='ROW';
```

### 执行一条 SQL 的过程是怎么样的？

- 客户端连接

  - 客户端通过 TCP/IP 协议连接到 MySQL 服务器

  - 连接器负责建立连接、身份认证、权限认证

- 语法解析

  - 检查 SQL 是否符合语法规范（如 `SELECT` 后必须有字段）。

  - 将 SQL 字符串转换为 **解析树（Parse Tree）**。

  - 会进行词法分析、语法分析。

- 查询优化

  - 根据解析树生成执行计划（比如使用哪个索引、排序方式、连接顺序）

  - 会尝试生成代价最低的执行方案：

    - 是否使用索引？

    - 哪张表先查？

    - 全表扫描还是索引范围扫描？

- 权限验证（访问控制）

  - 在执行前，会检查当前用户是否对目标表有 `SELECT` 权限

  - 权限不足则中断执行，抛出 `ERROR 1044` 等错误

- 查询执行

  - 执行器按照优化器生成的执行计划执行 SQL

  - 代替部分对应的 **存储引擎接口** 来读取数据

- 存储引擎访问（InnoDB）

  - 具体的数据读写由底层存储引擎负责：

    - 索引查找 - B+树

    - 页缓存 - Buffer Pool

    - 多版本控制 - MVCC

    - 锁控制 - 行锁、间隙锁

- 返回结果

  - 将结果通过 MySQL 协议返回给客户端

  - 结果集通常会先存放在服务端的内存中（或磁盘临时文件中），在逐步发送到客户端

### 讲讲 explain 是什么，说说 explain 的一些字段

执行计划展示了 MySQL 如何执行一条 SQL，包括使用哪个索引？哪个表先被访问？是全表扫描还是索引扫描？执行的顺序？

`EXPLAIN` 是 **MySQL 中用于分析 SQL 语句执行计划**的命令，尤其常用于 `SELECT` 查询，可以帮助开发者了解 **查询优化器如何执行 SQL**，从而优化索引、语句结构，提升性能。

`EXPLAIN` 可以在查询语句前添加，查看查询在执行时的执行路径：

```sql
EXPLAIN SELECT * FROM user WHERE id = 1;
```

常见字段解析：

| 字段名          | 含义说明                                               |
| --------------- | ------------------------------------------------------ |
| `id`            | 查询的标识符，标识 SELECT 的执行顺序，**越大越先执行** |
| `select_type`   | 查询类型（如：SIMPLE、PRIMARY、SUBQUERY）              |
| `table`         | 访问的表名或别名                                       |
| `partitions`    | 使用的分区（若有）                                     |
| `type`          | **连接类型**，显示表之间连接方式，性能重要字段         |
| `possible_keys` | 可能使用的索引                                         |
| `key`           | 实际使用的索引                                         |
| `key_len`       | 索引使用的长度（单位字节）                             |
| `ref`           | 哪些列或常量与索引进行比较（过滤条件）                 |
| `rows`          | **MySQL 估计要读取的行数**（越小越好）                 |
| `filtered`      | 经过条件过滤后剩余的记录百分比                         |
| `Extra`         | 附加信息（是否使用临时表、是否排序、是否索引覆盖等）   |

重点字段详细解析：

- `type` （连接类型，越靠前性能越差）

| 类型           | 含义                               |
| -------------- | ---------------------------------- |
| `ALL`          | 全表扫描 ❌（最差）                |
| `index`        | 遍历索引（而非数据）               |
| `range`        | 范围查询（BETWEEN、<、>）          |
| `ref`          | 使用非唯一索引或唯一索引的前缀匹配 |
| `eq_ref`       | 主键或唯一索引 = 常量（最优之一）  |
| `const/system` | 表只有一行，或用常量替代表（最优） |

- `Extra` 常见值说明

| Extra 值                | 说明                                |
| ----------------------- | ----------------------------------- |
| `Using where`           | 使用了 WHERE 过滤（未使用索引过滤） |
| `Using index`           | 使用了 **索引覆盖**（无需回表）     |
| `Using index condition` | **ICP（Index Condition Pushdown）** |
| `Using temporary`       | 使用了临时表                        |
| `Using filesort`        | 使用了文件排序                      |
| `Impossible WHERE`      | where 条件永远为 false，不执行      |

### 如何优化慢 SQL？

优化慢 SQL 是数据库调优的重要环节，常见的优化思路是从 **语句本身**、**索引设计**、**表结构设计**、**执行计划分析**、**数据库参数**等多个维度 出发。

1. 定位慢 SQL 的方法

   - 慢查询日志

     - 开启方式：

       ```sql
       SET GLOBAL slow_query_log = 1;
       SET GLOBAL long_query_time = 1;
       ```

   - EXPLAIN 分析执行计划

   - 一系列 DBA 工具

2. 优化 SQL 语句

   - SQL 语法层面优化

   | 问题                    | 优化建议                               |
   | ----------------------- | -------------------------------------- |
   | 使用 `SELECT *`         | 改为只查需要的字段                     |
   | OR 查询                 | 改为 `UNION` 或分段查                  |
   | 模糊查询 `%xxx`         | 改用倒排索引、全文索引、或搜索引擎     |
   | 不必要的子查询/嵌套查询 | 优化为 JOIN                            |
   | 过多函数计算            | 尽量避免函数用于索引列（无法使用索引） |

   - 索引优化

   | 问题               | 优化建议                                          |
   | ------------------ | ------------------------------------------------- |
   | 没有索引           | 建立合适的索引（单列/联合）                       |
   | 索引未命中         | 使用 `EXPLAIN` 分析为什么没有走索引               |
   | 覆盖索引未使用     | 可通过调整字段顺序使用索引                        |
   | 索引列参与函数运算 | 函数计算会失效索引，如 `YEAR(create_time) = 2024` |
   | 索引失效           | 如 `!=`、`OR`、`IS NULL`、隐式类型转换            |

   - 表结构优化

     - 选择合适的数据类型（如 VARCHAR(20) 优于 TEXT）

     - 减少大字段（如 BLOB、TEXT）的使用或拆分冷热数据表

     - 水平分表或垂直分表

     - 选择合适的存储引擎（如 OLTP 场景使用 InnoDB）

3. 语句执行流程检查

   - 使用 `EXPLAIN` 查看以下问题

   | 关注点       | 判断依据                              |
   | ------------ | ------------------------------------- |
   | 是否全表扫描 | `type = ALL` ❌                       |
   | 是否使用索引 | `key` 是否有值                        |
   | 是否回表     | `Extra` 是否包含 `Using index`        |
   | 是否临时表   | `Extra` 是否包含 `Using temporary` ❌ |
   | 是否排序     | `Extra` 是否包含 `Using filesort` ❌  |

4. 分库分表和缓存优化

   - 缓存热点数据 - Redis

   - 热门 SQL 缓存到 CDN、Redis

   - 分库分表解决单表过大或高并发瓶颈

我会首先通过慢查询日志、performance_schema 等工具定位慢 SQL，然后通过 `EXPLAIN` 分析执行计划，关注是否存在全表扫描、索引未命中、临时表或文件排序等问题。接着检查 SQL 是否合理、是否可以用覆盖索引、是否需要添加联合索引等。此外，对于高频读的热点数据，也会考虑使用 Redis 缓存，进一步提高性能。

## Redis

### Redis 的数据类型有哪些？应用场景

1. String

   - 最基本的数据类型，存储字符串、整数或浮点数

   - 常用命令：`SET` `GET` `INCR` `DECR` `APPEND` `MSET`

   - 应用场景：

     - 缓存单个对象（如用户信息、Token）

     - 计数器（访问量、点赞数）

     - 分布式锁（结合 `SETNX` `EXPIRE`）

2. List

   - 按插入顺序排序的字符串列表，可以从两端推入、弹出元素

   - 常用命令：`LPUSH` `RPUSH` `LPOP` `RPOP` `LRANGE`

   - 应用场景：

     - 消息队列（通过`LPUSH`、`BRPOP`实现）

     - 最新动态列表

     - 代办任务列表

3. Set

   - 无序的字符串集合，自动去重

   - 常用命令：`SADD` `SREM` `SISMEMBER` `SUNION` `SINTER`

   - 应用场景：

     - 标签、兴趣等去重集合

     - 共同好友、共同兴趣的计算

     - IP 黑名单、唯一用户统计

4. Hash

   - 键值对集合，适合存储对象

   - 常用命令：`HSET` `HGET` `HDEL` `HGETALL`

   - 应用场景：

     - 存储用户信息、配置项（如 `user:1001 -> {name: 'Tom', age: 25}`）

     - 轻量级对象存储，适合结构化数据

     - 缓存对象属性

5. Sorted Set

   - 类似 Set，但每个元素携带分数，按分数排序

   - 常用命令：`ZADD` `ZRANGE` `ZREM` `ZRANK` `ZSCORE`

   - 应用场景：

     - 排行榜

     - 时间排序数据

     - 延迟队列

### Redis 的数据类型的底层数据结构有了解吗？

- String：动态字符串

底层结构： **SDS（Simple Dynamic String）**：Redis 自定义的字符串结构，支持二进制安全、动态扩容

特点：SDS 会多分配空间以减少频繁 `realloc`，长度 `O(1)` 获取

- List：列表

底层结构：小数据量时 `ziplist（压缩列表）`，大数据量时 `quicklist（快速列表）`

切换条件：元素较大或数量较多，ziplist 自动转换

- Hash：哈希表

底层结构：小哈希表时：`ziplist`，大哈希表时：`hashtable` （dict 字典）

切换条件：由配置项控制 `hash-max-ziplist-entries` `hash-max-ziplist-value`

- Set：集合

底层结构：小集合时：`intset`，大集合时：`hashtable` （dict 字典）

切换条件：由配置项控制 `set-max-intset-entries`

- Sorted Set：有序集合

底层结构：小数据量时：`ziplist`，大数据量时：`skiplist` （跳表）

### ZSet 为什么使用 SkipList 跳表实现？SkipList 跳表是什么？

- ZSet 是一个**有序集合**，要求支持：

  - 按分数排序插入元素

  - 快速根据分数区间查找

  - 获取某元素的排名

  - 快速删除或更新

| 特性                            | Redis 的需求             | SkipList 是否支持 |
| ------------------------------- | ------------------------ | ----------------- |
| 插入、删除时间复杂度            | O(log n)                 | ✅ 是             |
| 按分数范围查找（区间查询）      | 例如 `ZRANGEBYSCORE`     | ✅ 是             |
| 随机访问第 N 名成员（排名操作） | 例如 `ZRANK`, `ZREVRANK` | ✅ 是             |
| 实现简单，易于维护              | Redis 代码需高效且可维护 | ✅ 是             |

- `SkipList` 跳表

跳表是一种 **基于概率的多级索引结构**，支持像平衡树一样的 **快速查找、插入和删除操作**，时间复杂度为 `O(log n)`。

- 跳表的结构

```
Level 3:       A ------> F
Level 2:       A --> C --> F
Level 1: A --> B --> C --> D --> E --> F
```

### Redis 如何实现分布锁？

**分布式锁**是一种机制，用于协调分布式系统中多个节点对共享资源（如数据库、缓存、文件等）的互斥访问，确保同一时刻只有一个客户端能操作这部分资源

- 使用 Redis 实现分布锁的基本方式：

  - 使用 Redis 的命令：

    ```shell
    SET lock_key "client_id" NX EX 30
    ```

    | 选项          | 说明                              |
    | ------------- | --------------------------------- |
    | `SET`         | 设置锁键                          |
    | `NX`          | 如果 key 不存在才设置（即获取锁） |
    | `EX 30`       | 锁的过期时间 30 秒（防止死锁）    |
    | `lock_key`    | 锁名（例如：`order:12345:lock`）  |
    | `"client_id"` | 用于标识请求者身份，释放时需验证  |

  - 释放锁的安全做法

    ```shell
    if redis.call("GET", KEYS[1]) == ARGV[1] then
        return redis.call("DEL", KEYS[1])
    else
        return 0
    end
    ```

### SetNx 实现的分布锁有什么问题？

1. **不是原子操作，存在竞态条件**

   `SETNX` + `EXPIRE` 是 **两个独立命令**，可能导致 `lock_key` **没有过期时间**：永久死锁、Redis 不知道这个锁何时释放

2. **锁续期和释放逻辑复杂**

   如果锁快过期了，业务处理还没结束，需要续期

   - 使用 `EXPIRE` 手动续期并不安全

   - 续期失败可能导致锁被错误释放，造成 **并发写**

3. **无法安全释放锁**

   - 多个客户端争锁时，若不对比 `client_id`，可能错误地释放了别人的锁

应该使用原子 `SET` 命令替代 `SETNX + EXPIRE`

```shell
SET lock_key client_id NX EX 30
```

原子：只要设置成功，就自动带上过期时间

安全：避免中间状态产生死锁

配合 Lua 脚本解锁，确保锁由“自己”释放

### Redisson 分布式锁底层使用什么数据结构？

Redisson 实现分布式锁，底层主要依赖 **Redis** 的 **String** 类型键值对，配合一些扩展机制（如 Lua 脚本、Pub/Sub、Watchdog）来实现完整的功能。

| 功能模块           | 使用的 Redis 数据结构    |
| ------------------ | ------------------------ |
| 加锁状态           | `String`（字符串）       |
| 锁续期/元数据管理  | 键过期时间（EXPIRE）     |
| 锁解锁（原子校验） | `Lua 脚本`               |
| 可重入次数计数     | `Hash`（可选）或内存维护 |
| 阻塞/唤醒等待线程  | `Pub/Sub`                |

### Redisson 分布式锁的好处有哪些？

| 优点                      | 说明                                                   |
| ------------------------- | ------------------------------------------------------ |
| 支持可重入锁（Reentrant） | 同一线程可以多次获得同一把锁，不会死锁                 |
| 自动续期机制（Watchdog）  | 默认 30 秒自动续租，业务执行超时不担心锁被误释放       |
| 锁释放安全（Lua 校验）    | 解锁时验证客户端身份，避免误删他人锁                   |
| 原子操作封装              | 加锁、解锁等用 Lua 保证原子性，无需手写                |
| 高可用（支持 Redlock）    | 多 Redis 节点容灾，提升稳定性                          |
| 支持阻塞等待锁            | 支持 `tryLock()`、`lock()` 等带超时等待的阻塞锁        |
| 支持公平锁、公平排队      | 可以按请求顺序排队加锁，避免“饥饿”                     |
| 接口简单，开箱即用        | 使用 Java 原生风格 API，无需关心底层实现               |
| 与 Redis Cluster 完美集成 | 自动适配 Redis 哨兵、主从、集群架构                    |
| 支持异步/反应式编程模型   | 提供异步和 Reactive 风格的锁接口（例如 `lockAsync()`） |

### 解释一下如何实现可重入？

实现**可重入锁**（Reentrant Lock）是分布式锁设计中的一个重要特性，它允许**同一个线程在不释放锁的前提下重复获取同一把锁**，不会发生死锁。

可重入锁的关键：**记录加锁次数** + **判断加锁者身份**

核心流程图

```
Client Thread A 请求锁
│
├─ 是否已有锁？
│   ├─ 是自己 → 重入次数++
│   └─ 否 → 正常尝试获取 Redis 锁
│
└─ 加锁成功 → 设置锁值 client_id:thread_id + 过期时间

Client Thread A 解锁
│
├─ 是否自己持有锁？
│   ├─ 是 → 重入次数--
│       └─ 若为 0 → 删除锁
│   └─ 否 → 解锁失败（保护机制）
```

### 超市续约的看门狗机制了解吗？

**看门狗机制** 是在使用分布式锁时，**自动延长锁的过期时间**的一种机制，防止锁还未释放时就意外生效，从而导致多个线程/服务获得同一把锁，造成并发安全问题

实现原理：

- 加锁时启动定时任务

- 默认每 10 秒续期一次

- 续期逻辑

- 使用 Lua 脚本保证续期原子性

Redis 分布式锁为了防止业务执行超时导致锁提前失效，会使用“看门狗机制”，定期自动对锁续期，防止锁被误删导致并发问题。Redisson 中就内置了看门狗机制，它默认每 10 秒续期一次，把锁过期时间延长至 30 秒，直到业务执行完毕后手动释放锁为止。

### Redisson 分布式锁的问题是什么？如何解决？

1. 锁误释放问题：当多个线程/节点使用同一个 key 锁，但**标识不一致**时，容易导致**释放了不是自己加的锁**

2. 主从复制延迟导致锁失效：如果 Redis 是主从架构（读写分离），加锁成功写入主节点，但尚未同步到从节点，此时另一个客户端从从节点读取锁 key，发现不存在，会**错误地获得锁**

3. RedLock 算法的可靠性争议：Redisson 支持 RedLock（多个 Redis 节点同时加锁，获得多数锁才算成功），但该算法的可靠性曾被质疑

4. 锁续期失败：在程序崩溃、宕机；守护线程异常退出；网络抖动，续期失败

5. 性能瓶颈：Redisson 是基于 Redis 的，锁的获取、续期、释放都要走网络，**QPS 高时 Redis 会成为性能瓶颈**

6. 锁重入/可重入问题：Redisson 支持可重入锁，但如果误用**不同 RedissonClient 实例**，会导致锁不是可重入的（每个实例都有自己的 UUID）

### 介绍 Redis 的持久化方式（RDB、AOF、混合持久化）？优缺点？

- RDB （Redis DataBase）

  - 机制：将某一时刻的内存快照保存

  - 优点：启动恢复速度快；对性能影响小；文件紧凑，适合备份

  - 缺点：**可能丢失数据**；子进程开销大

- AOF（Append Only File）

  - 机制：Redis 每次执行写命令时将其**追加记录到 AOF 文件末尾**，AOF 文件可以重写/压缩以减少体积

  - 刷盘策略：`appendfsync always/everysec/no`

  - 优点：更高的数据安全性，AOF 文件是可读的、可编辑的 Redis 命令，便于恢复和审查

  - 缺点：启动恢复慢、文件体积可能比 RDB 大、写入开销更高

- 混合持久化

  - 机制：在 AOF 重写将 RDB 快照作为文件开头，后面追加 AOF 增量日志

  - 优点：兼具 RDB 启动快和 AOF 数据安全的优势，恢复速度快

  - 缺点：配置略复杂、只有在 AOF 重写时才会触发混合持久化

### Redis 的内存淘汰策略有哪些？

Redis 的**内存淘汰策略**（Eviction Policy）用于在内存达到上限时**决定哪些键要被移除**，避免 OOM（Out of Memory）异常。

| 策略名称          | 含义                                    | 特点                            |
| ----------------- | --------------------------------------- | ------------------------------- |
| `noeviction`      | 不删除数据，写入时直接返回错误          | 默认策略，**只读不写**          |
| `allkeys-lru`     | 所有 key 中淘汰最近最少使用的           | 常用于通用缓存场景              |
| `volatile-lru`    | 只对设置了过期时间的 key 进行 LRU 淘汰  | 更可控，但可能无法释放足够空间  |
| `allkeys-random`  | 所有 key 中随机淘汰                     | 适用于简单快速释放空间但不精准  |
| `volatile-random` | 只随机淘汰设置了过期时间的 key          | 风险小于 allkeys-random         |
| `volatile-ttl`    | 只淘汰 TTL 最近要过期的 key（TTL 小的） | 优先保留生存时间长的数据        |
| `allkeys-lfu`     | 所有 key 中淘汰访问频率低的             | Redis 4.0+ 引入，更精准但代价大 |
| `volatile-lfu`    | 只淘汰设置了过期时间中最少使用的 key    | 最复杂，但智能度最高            |

### 什么是缓存击穿/穿透/雪崩？分别怎么解决？

缓存中的 **击穿** / **穿透** / **雪崩** 是三种常见问题，容易导致缓存失效、数据库压力激增，甚至宕机。

- 缓存穿透（Cache Penetration）

  - 概念：请求的数据**缓存和数据库中都不存在**，每次请求都打到数据库，造成数据库压力

    | 方式                   | 说明                                                      |
    | ---------------------- | --------------------------------------------------------- |
    | **布隆过滤器（推荐）** | 把所有可能存在的 Key 预先放入布隆过滤器，拦截非法请求     |
    | **缓存空值**           | 将查不到的数据（如 `null`）也缓存，并设置短 TTL（如 30s） |
    | **参数校验**           | 对参数格式、范围提前校验，过滤掉非法请求                  |

- 缓存击穿（Cache Breakdown）

  - 概念：某个热点 Key **刚好过期**，大量请求同时访问它，缓存失效后同时穿透到数据库，造成数据库压力爆增

    | 方式                          | 说明                                                       |
    | ----------------------------- | ---------------------------------------------------------- |
    | **互斥锁 / 分布式锁（推荐）** | 只有一个请求能查询数据库并重建缓存，其他请求等待或返回旧值 |
    | **热点数据不过期**            | 对热点数据设置极长过期时间或定时刷新机制                   |
    | **预加载（缓存预热）**        | 启动或热点时间前提前加载热点数据进缓存                     |

- 缓存雪崩（Cache Avalanche）

  - 概念：大量缓存数据在同一时间失效，导致瞬时大量请求打到数据库，引发**服务雪崩**（整体宕机风险）。

    | 方式                          | 说明                                                       |
    | ----------------------------- | ---------------------------------------------------------- |
    | **过期时间加随机（推荐）**    | 给缓存 TTL 添加随机值（如 5 分钟 ± 1\~2 分钟），错开过期点 |
    | **热点缓存永不过期+异步更新** | 不让缓存过期，通过定时任务或消息队列更新                   |
    | **多级缓存架构**              | 本地缓存 + 分布式缓存，缓解瞬时压力                        |
    | **限流降级机制**              | 缓存失效时，对数据库访问做限流，并降级处理用户请求（       |

### 什么是热 Key 问题？影响？解决方案？

热 Key：在一段时间内，被访问频率异常高的 Key

**热 Key 问题**是指在缓存系统中，**某个或某几个 Key 被高频率访问**，造成资源集中访问瓶颈，可能导致服务性能下降或系统不可用，在分布式环境中尤为显著

影响：

| 问题             | 描述                                                  |
| ---------------- | ----------------------------------------------------- |
| **网络瓶颈**     | 热点 Key 集中访问某一台缓存节点，造成节点网卡/IO 饱和 |
| **单点压力过大** | Redis 单实例承压过大，QPS 激增，甚至被打挂            |
| **缓存击穿风险** | 如果热 Key 失效，同一时间大量请求穿透 DB              |
| **系统雪崩**     | 热 Key 问题扩散，服务压力连锁反应，引发雪崩           |

解决方案：

- 本地缓存 + 分布式缓存（多级缓存）

  常见架构：`Guava Cache` / `Caffeine` 本地缓存 + Redis

  热点数据先命中本地缓存，减少对 Redis 的访问

- 缓存 Key 分片（分散访问）

  将一个热 Key 拆分为多个小 Key

  示例：将 `hot_goods` 拆成 `hot_goods_1`~`hot_goods_10`

  读写时随机路由或 hash 路由（需要合并处理逻辑）

- 随机 TTL

  防止多个热 Key 同时过期，缓解雪崩

  如设置 `baseTTL + random(0, 300s)`

- 热点数据永久缓存 + 异步刷新

  热点 Key 不设置过期时间（`TTL = -1`），定时后台异步刷新缓存

  保证缓存命中率且减少过期带来的击穿

- 分布式限流降级

  当检测到某 Key QPS 异常，可以限流或降级返回默认值

- 使用 Redis Cluster 时合理分片

  避免某个槽点（slot）过热，保证 Key 分布均匀

  利用 `HASH_TAG` 控制合理分布

- 热点 Key 预警监控

  监控 Key 的访问量、命中率等

  配置自动告警，及时干预

### 什么是大 key 问题？影响？解决方案？

大 Key 问题指的是：**单个 Redis Key 对应的 Value 数据量过大**

比如：一个 Key 存储了几万个元素的列表、Hash、Set 等；某个 String 类型的值本身就很大

**常见大 Key 类型**：

- String：内容为 Base64 编码图片、大段文本、JSON 串等

- List/Set/Hash/ZSet：成员数量几十万甚至百万级

**影响**：

| 问题                      | 描述                                                |
| ------------------------- | --------------------------------------------------- |
| **操作耗时长**            | 获取 / 设置 / 删除大 Key 需要较长时间，拖慢请求响应 |
| **阻塞线程**              | Redis 是单线程模型，大 Key 的操作会阻塞后续命令执行 |
| **主从复制慢**            | 大 Key 会拖慢主从同步，导致主从延迟甚至故障         |
| **内存碎片化严重**        | 大 Key 频繁更新或删除容易造成内存碎片，影响性能     |
| **AOF 重写或 RDB 生成慢** | 持久化过程处理大 Key 代价高，可能阻塞主线程         |

**解决方案**：

1. 避免存入大 Key：

   - 优化业务逻辑，**按需缓存**，不要整页缓存大 JSON 或整表数据

   - 拆分结构，尽量使用多个小 Key 替代一个大 Key

2. 大 Key 拆分：

   - 将大集合结构拆分成多个小集合

3. 定期扫描和告警：

   - 使用 Redis 提供的命令检测大 Key

4. 加操作超时限制

   - 设置客户端或中间件访问 Redis 操作的超时时间

   - 防止大 Key 拖慢请求造成级联超时

5. 设置合理的 TTL

   - 给大 Key 设置过期时间，避免永久存在，占用内存

6. 开启 LRU 淘汰策略

   - 配置 `maxmemory-policy` 防止内存被大 Key 占满

7. 持久化策略优化

   - 关闭大 Key 的持久化（如 AOF 的 `appendfsync` 优化）

   - 对大 Key 进行 Key 过滤或分离到非持久化 Redis 节点

### Redis 的高可用了解吗？主从？哨兵？Redis cluster？

### 除了官方的还了解过其他的 Redis 高可用方案吗？

### 主从数据同步的原理了解吗？

Redis 的主从同步分为两个阶段：

- 全量同步（Full Sync）：从节点刚链接主节点，或因为断线重新连接时，**无法保证数据一致**，会触发全量同步流程

- 增量同步（Incremental Sync）：当主从断线后重新连接，如果从节点能提供上次复制的 **偏移量 offset**，并且主节点仍保留那部分数据在复制缓冲区，就可以增量同步

**先发命令再传快照，快照完成命令再补发。全量同步 = RDB 快照 + 命令补发**

## Spring

### 解释一下 IoC 和 AOP？IoC 的好处？

### AOP 的应用场景？实现原理？

### Spring 事务的实现原理？

### SpringBoot 自动装配原理？

### SpringBoot 的启动流程？

### SpringBoot 如何优雅停机

### Spring 常见注解有哪些？

### Spring Bean 的作用域有哪些？

### Spring MVC 的执行流程？

### Spring 中用到了哪些设计模式？

### 循环依赖问题是什么？三级缓存是什么？

### 三级缓存怎么解决循环依赖问题？

### @Autowired 和 @Resource 的区别？

### 事务失效的原因有哪些？

## 计算机网络

### 键入网址到网页显示，期间发生了什么？

### Http1.0 与 Http1.1 比较区别？

### Http1.1 与 Http2.0 比较区别？

### Http2.0 与 Http3.0 比较区别？

### Quic 协议了解过吗？

### Https 的加密过程可以介绍一下吗？

### Https 为什么要用两种加密算法：对称加密算法和非对称加密算法？

### TCP 的可靠性是怎么保证的？

### 介绍一下拥塞控制？

### 介绍一下 TCP 三次握手的过程？

### 介绍一下 TCP 四次挥手的过程？

### 为什么是三次握手？不是两次？

### 为什么挥手需要四次？

### 为什么有 TIME_WAIT 状态？为什么是 2MSL 时间？

### TIME_WAIT 状态的连接过多怎么处理

### TCP 粘包和拆包是什么？解决方案？

### CDN 是什么？

### HTTP 状态码有哪些？

### GET 和 POST 请求的区别？

### 半连接队列和全连接队列是什么?

### 什么是 SYN 攻击？如何避免 SYN 攻击？

## 操作系统

### 进程线程协程是什么？

### 进程间通信方式？线程间通信方式？

### 介绍一下操作系统的内存管理机制？

### 什么是虚拟内存？

### 产生死锁的四个条件？

### 介绍一下 IO 多路复用 select、poll、epoll？原理和优缺点？

### Linux 的常用命令有哪些？

### 介绍一下中断？

### 介绍一下孤儿进程 && 僵尸进程？

### 介绍一下用户态 && 内核态？

### 介绍一下进程调度算法？

## 数据结构与算法

### 讲讲归并排序

### 讲讲快速排序

### 快速排序的时间复杂度由什么影响

### 如何用两个栈模拟一个队列

## 场景题

### 如何预估接口上线后的 QPS

### 如何预估/回答接口本身能抗住多少 QPS

### 订单<未支付&到期>关闭如何实现

### 1 亿个数据取出最大前 100 个

### 大文件排序

### Redis 缓存与 MySQL 数据一致性
