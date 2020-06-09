# ConcurrentHashMap

* HashMap：非线程安全的，JDK1.7基于数组+链表，JDK1.8基于数组+链表+红黑树
* Hashtable：线程安全的，1.7和1.8都是数组+链表，全部方法都是基于synchronized加锁，效率非常低
* ConcurrentHashMap：线程安全的，并且支持很多场景下并发操作，提高了效率
  * JDK1.7：基于数组+链表实现，本质上基于Segment分段锁技术，Segment继承了ReentrantLock,不同的Segment之间多线程可以并发操作，同一个Segment是使用lock加锁
  * JDK1.8：基于数组+链表+红黑树实现，本质上是使用CAS+Synchronized加锁实现线程安全，不同节点多线程可以并发操作，如put操作，同一个节点，如果是空使用CAS，如果不为空，使用synchronized加锁
  * 1.7和1.8效果：
    * 读写分离可以提高效率：多线程对不同Node/Segment的插入/删除是可以并发，并行执行，对同一个Node/Segment的写操作是互斥的，读操作都是无锁操作，可以并发，并行执行。