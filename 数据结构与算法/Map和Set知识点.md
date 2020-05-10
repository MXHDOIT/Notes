# Map和Set

### 基础知识：

1. Map和Set：用来进行用来进行搜索的集合类
2. 存储元素的类型：K-V模型(键值对)和K模型
3. 应用场景：
   1. K-V: 统计一个文件中单词出现的次数 ----><单词，次数>
   2. K模型：在词典中找一个单词
4. Map中存储的是K-V模型，Set中只存储了K，共同点：K不能重复

### Map

1. Map是一个接口，该接口没有继承Collection

2. Map中存储的是K-V接口的键值对，并且要求K不能重复（V是可以重复的）

3. Map中的键值对，键值对体现：在Map内部实现了Entry接口，该接口中包含有K，V

   * 方法：
     * K getKey()   ; 		        返回entry中的key
     * V getValue()   ;              返回entry中的value  
     * V  setValue(V value)    ;将键值对中的value替换为指定的value

4. Map中的方法：

   ![image-20200414223043313](https://raw.githubusercontent.com/MXHDOIT/picBed/master/img/image-20200414223043313.png)

5. Map的使用

   * 注意:

     * Map是一个接口，必须使用TreeMap或者HashMap来进行实例化

     * Map中的key可以全部分离处理，存储到Set中来进行访问(因为Key不能重复)

     * Map中的value可以全部分离处理，存储到Collection中的任何一个子集(value可能有重复)

     * 在Map中TreeMap的key不能为空，HashMap的key可以为空；他们的value都可以为空

     * Map中键值对的key不能直接修改，value可以修改

     * TreeMap和HashMap的区别：

     * | Map底层结构                | TreeMap                                          | HashMap                               |
       | -------------------------- | ------------------------------------------------ | ------------------------------------- |
       | 底层结构                   | 红黑树                                           | 哈希桶                                |
       | 插入、删除、查找时间复杂度 | O(logn)                                          | O(1)                                  |
       | 是否有序                   | 关于key有序                                      | 无序                                  |
       | 线程安全                   | 不安全                                           | 不安全                                |
       | 插入，删除，查找区别       | 需要进行元素比较                                 | 通过哈希函数计算哈希地址              |
       | 比较与覆写                 | key必须能够比较，否则会抛出ClassCastExcetion异常 | 自定义类型需要覆写equals和hascode方法 |
       | 应用场景                   | 需要key有序                                      | 不关心key是否有序，更高的查询效率     |

       * TreeMap：
         1. 如果key是自定义类型的元素，该类对象必须要可以比较大小
         2. key一定不能为空，如果为空会抛NullPointerException -->原因无法比较大小
       * HashMap：
         * key可以为空

### Set

1. Set是一个接口，该接口继承了Collection,该接口中只能存放K

2. 注意：
   1. set只能使用TreeSet和HashSet
   2. Set里面的key是不能够重复的，TreeSet：去重+排序 ;hashSet：去重
   
3. Set中元素的遍历---->迭代器：设计模式--->依次寻访容器中的元素，而又无序了解其底层数据结构或者无需暴露底层接口实现

4. Set的方法：

   ![image-20200415101105209](https://raw.githubusercontent.com/MXHDOIT/picBed/master/img/image-20200415101105209.png)

   



### TreeMap的底层的数据结构：

* 红黑树：首先红黑树是一颗二叉搜索树+增加节点的颜色限制以及性质约束 --->保证：最长路径中节点个数一定不会超过最短路径中节点个数的两倍就认为其近似平衡
  * 性质：
    1. 每个节点不是红色就是黑色
    2. 根节点一定是黑色的
    3. 没有连在一起的红色节点
    4. 每条路径中黑色节点个数是一样的
    5. 所有叶子节点(指空节点)都是黑色的



### 什么是哈希？

1. 概念：哈希是用来进行高效查找的一种数据结构，该种数据结构是通过某种方式(哈希函数) 将元素与其表格存储位置建立一一对应的关系，在查找时不需要遍历，因此可以达到高效的查找--->O(1)

2. 哈希中可能会存在哈希冲突----不同元素通过相同哈希函数计算出相同的哈希地址

3. 必须要解决哈希冲突：

   * 检测哈希函数设计是否合理，如果设计不合理---哈希函数产生的哈希地址集中在一起：不离散
     * 重新设计哈希函数：哈希函数值域必须在表格的范围之内，哈希函数产生的哈希地址尽可能离散，哈希设计尽可能简单
     * 常见哈希函数：直接定址法 除留余数法  折叠法  平方取中法  随机数法  数学分析法
   * 注意：不论一个哈希函数设计有多精妙，都不能完全解决哈希冲突

4. 解决哈希冲突的方式

   * **闭散列**：从发生哈希冲突的位置开始，找“下一个”空位置

     1. **线性探测**：找下一个空位置----->从发生哈希冲突的位置开始，逐个挨着依次往后查找；【标志位：EMPTY,EXIST,DELETE】

        * 优点：找下一个地址的方式简单
        * 缺陷：容易产生数据的堆积---发生哈希冲突的元素容易练成一片【原因：找下一个空位置时挨着往后依次查找】
        * 哈希负载因子(装载因子)：哈希表中有效元素个数/表格容量 ---> 将该比率称之为哈希负载因子--->一般肯定不会为100%
          * 0.75 ---> 扩容：增加容量

        **线性探测容易产生数据堆积缺陷能否解决？--->可以解决：找到下一个空位置方式----不要挨着依次往后查找【如下】**

     2. **二次探测** :注意---二次探测不是探测两次

        * 第一次计算出的哈希地址H0，第i次探测时，哈希地址为H(i)

          H(i) = H0+i^2;

          H(i) = H0-i^2;

        * 优点：解决了线性探测容易产生数据堆积的问题

        * 缺陷：如果空位置比较少，可能需要探测多次

     * 哈希方式：永远不会将表格存满(元素越多，产生冲突概率越大)，是使用空间换时间的一种方式

   * **开散列**：链地址法（开链法）

     * 哈希表中没有直接存元素，每个位置将来挂接都是一个链表，即：将发生冲突的元素通过链表的方式组织起来

   

### HashMap的底层：

* HashMap是基于哈希表实现的，每一个元素是一个key-value对，其内部通过单链表解决冲突问题，容量不足（超过了阀值）时，同样会自动增长。

* HashMap是非线程安全的，只是用于单线程环境下，多线程环境下可以采用concurrent并发包下的concurrentHashMap。

* HashMap 实现了Serializable接口，因此它支持序列化，实现了Cloneable接口，能被克隆。

* HashMap内部维护了一个存储数据的Entry数组，HashMap采用链表解决冲突，每一个Entry本质上是一个单向链表。当准备添加一个key-value对时，首先通过hash(key)方法计算hash值，然后通过indexFor(hash,length)求该key-value对的存储位置，计算方法是先用hash&0x7FFFFFFF后，再对length取模，这就保证每一个key-value对都能存入HashMap中，当计算出的位置相同时，由于存入位置是一个链表，则把这个key-value对插入链表头。

* HashMap中key和value都允许为null。key为null的键值对永远都放在以table[0]为头结点的链表中。

  ```java
   // 将“key-value”添加到HashMap中  
      public V put(K key, V value) {  
          // 若“key为null”，则将该键值对添加到table[0]中。  
          if (key == null)  
              return putForNullKey(value);  
          // 若“key不为null”，则计算该key的哈希值，然后将其添加到该哈希值对应的链表中。  
          int hash = hash(key.hashCode());  
          int i = indexFor(hash, table.length);  
          for (Entry<K,V> e = table[i]; e != null; e = e.next) {  
              Object k;  
              // 若“该key”对应的键值对已经存在，则用新的value取代旧的value。然后退出！  
              if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {  
                  V oldValue = e.value;  
                  e.value = value;  
                  e.recordAccess(this);  
                  return oldValue;  
              }  
          }  
   
          // 若“该key”对应的键值对不存在，则将“key-value”添加到table中  
          modCount++;
  		//将key-value添加到table[i]处
          addEntry(hash, key, value, i);  
          return null;  
      }
  
    // putForNullKey()的作用是将“key为null”键值对添加到table[0]位置  
      private V putForNullKey(V value) {  
          for (Entry<K,V> e = table[0]; e != null; e = e.next) {  
              if (e.key == null) {  
                  V oldValue = e.value;  
                  e.value = value;  
                  e.recordAccess(this);  
                  return oldValue;  
              }  
          }  
          // 如果没有存在key为null的键值对，则直接题阿见到table[0]处!  
          modCount++;  
          addEntry(0, null, value, 0);  
          return null;  
      }
  
  ```

  

*  HashMap的存储结构，如下图所示：

  ![image-20200424105117546](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200424105117546.png)

* 图中，紫色部分即代表哈希表，也称为哈希数组，数组的每个元素都是一个单链表的头节点，链表是用来解决冲突的，如果不同的key映射到了数组的同一位置处，就将其放入单链表中。

* HashMap内存储数据的Entry数组默认是16，如果没有对Entry扩容机制的话，当存储的数据一多，Entry内部的链表会很长，这就失去了HashMap的存储意义了。所以HasnMap内部有自己的扩容机制。HashMap内部有：

  * 变量size，它记录HashMap的底层数组中已用槽的数量；
  *  变量threshold，它是HashMap的阈值，用于判断是否需要调整HashMap的容量（threshold = 容量*加载因子）
  * 变量DEFAULT_LOAD_FACTOR = 0.75f，默认加载因子为0.75
  * HashMap扩容的条件是：当size大于threshold时，对HashMap进行扩容 

* 链表和红黑树的相互转换

  *  如果哈希桶中某条链表的个数**超过8**，并且**桶的个数超过64**时才会将链表转换为红黑树，否则直接扩容
  *  若红黑树中节点**小于6**时，红黑树退还为链表

* HashMap共有四个构造方法。构造方法中提到了两个很重要的参数：初始容量和加载因子。这两个参数是影响HashMap性能的重要参数，其中容量表示哈希表中槽的数量（即哈希数组的长度），初始容量是创建哈希表时的容量（从构造函数中可以看出，如果不指明，则默认为16），加载因子是哈希表在其容量自动增加之前可以达到多满的一种尺度，当哈希表中的条目数超出了加载因子与当前容量的乘积时，则要对该哈希表进行 resize 操作（即扩容）。

* 下面说下加载因子，如果**加载因子越大，对空间的利用更充分，但是查找效率会降低**（链表长度会越来越长）；如果**加载因子太小，那么表中的数据将过于稀疏**（很多空间还没用，就开始扩容了），对空间造成严重浪费。如果我们在构造方法中不指定，则系统默认加载因子为0.75，这是一个比较理想的值，一般情况下我们是无需修改的。

* 另外，无论我们指定的容量为多少，构造方法都会将实际容量设为不小于指定容量的2的次方的一个数，且最大值不能超过**2的30次方**

* 插入节点

  * Java7中：HashMap将元素向链表中插入使用的是头插
  * Java8中：HashMap将元素向链表中插入使用的是尾插
    * 原因是在多线程的情况下：避免防止死循环

* 哈希函数

  ```java
  /*
  1. key如果为空，返回的是0号桶
  2. key如果不为空，返回该key所对应的哈希码，从该位置可以看出，如果key是自定义类型，必须要重写Object类 的hashCode方法
  3. 高16bit不变，低16bit和高16bit做了一个异或，主要用于当hashmap 数组比较小的时候所有bit都参与运算了
  目的是减少碰撞
  4. 获取到哈希地址后，计算桶号的方式为：index = (table.length - 1) & hash
  5. 通过除留余数法方式获取桶号，因为Hash表的大小始终为2的n次幂，因此可以将取模转为位运算操作，提高效
   率，这也就是为什么要按照2倍方式扩容的一个原因
   
  key&(table.length-1) 二进制 index key%(table.length) index
   4&15 100&1111 100 4%16 4
   13&15 1101&1111 1101 13%16 13
   19&15 10011&1111 11 19%16 3 
  通过上述方式可知，实际上hashcode的很多位是用不上的，因此在HashMap的hash函数中，才使用了移位运算，只
  取了hashcode的前16位来做映射。2^15=65536 已经是很大的数字了，另一方面&操作比取模效率更高
  */
  static final int hash(Object key) {
   int h;
   return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
   }
  ```

* 下一次扩容大小计算: **每次都是将cap扩展到大于cap最近的2的n次幂**

  ```java
  static final int tableSizeFor(int cap) {
   int n = cap - 1; 
   n |= n >>> 1;
   n |= n >>> 2;
   n |= n >>> 4;
   n |= n >>> 8;
   n |= n >>> 16;
   return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
   }
  /*
  int n = cap - 1; 这是为了防止，cap已经是2的幂。如果cap已经是2的幂， 又没有执行这个减1操作，则执行完后面的几条无符号右移操作之后，返回的capacity将是这个cap的2倍。
  */
  ```

* resize扩容

  * 在**Java8****的扩容中，不是简单的将原数组中的每一个元素取出进行重新hash映射，而是做移位检测**。所谓移位检测的含义具体是针对HashMap做映射时的&运算所提出的，通过上文对&运算的分析可知，映射的本质即看hash值的某一位是0还是1，当扩容以后，会相比于原数组多出一位做比较，由多出来的这一位是0还是1来决定是否进行移位，而具体的移位距离，也是可知的，及位原数组的大小，我们来看下表的分析，假定原表大小为16：

    ![image-20200510225148965](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200510225148965.png)

  * 

### 面试题：HashTable 和HashMap的区别：

1. 继承父类不同
   1. Hashtable是继承Dictionary,而HashMap继承自AbstractMap类。但都实现了Map接口
2. Hashtable是线程安全的，每个方法都添加synchronize，HashMap线程不安全
3. Hashtable的key和value值都不可以为null，而HashMap的key可以为null，只有一个切都在table[0]中不一定是第一个元素，value可以是null
4. Hashtable中有elements()和contains()方法；
   1. elements()：得到全部value的值
   2. contains()：和containsValue功能相同
5. 内部实现底层的数组初始化和扩容不同
   1.  HashTable在不指定容量的情况下的默认容量为11，而HashMap为16，Hashtable不要求底层数组的容量一定要为2的整数次幂，而HashMap则要求一定为2的整数次幂。
   2.  Hashtable扩容时，将容量变为原来的2倍加1，而HashMap扩容时，将容量变为原来的2倍。
6. hash值不同
   1. 哈希值的使用不同，HashTable直接使用对象的hashCode。而HashMap重新计算hash值。
   2. hashCode是jdk根据对象的地址或者字符串或者数字算出来的int类型的数值。
   3.   Hashtable计算hash值，直接用key的hashCode()，而HashMap重新计算了key的hash值，Hashtable在求hash值对应的位置索引时，用取模运算，而HashMap在求位置索引时，则用与运算，且这里一般先用hash&0x7FFFFFFF后，再对length取模，&0x7FFFFFFF的目的是为了将负的hash值转化为正值，因为hash值有可能为负数，而&0x7FFFFFFF后，只有符号外改变，而后面的位都不变
7. 两个在遍历方式上不同
   1.  Hashtable、HashMap都使用了 Iterator。而由于历史原因，Hashtable还使用了Enumeration的方式 。

