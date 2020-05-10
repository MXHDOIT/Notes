# 二叉排序树-->AVL-->红黑树-->B树丶B+树

### 1.二叉排序树

* 概念：BST（Binary Sort Tree）,对于二叉排序树的任何一个非叶子节点，要求左子节点的值比当前节点的值小，右子节点的值比当前节点的值大。

* 需求：对于一个数列，能够高效完成对数据的查询和添加。

* **二叉排序树创建和遍历(中序遍历)**

  * ```java
    //向二叉排序树添加节点
    public void add(Node node){
        if(null == root){
            root = node;
        }else{
            root.add(node);
        }
    }
    
    //此方法属于节点类
    public void add(Node node){
    	if(null == node){
    		return ;
    	}
    	
    	if(node.value < this.value){
    		//如果左节点为空
    		if(null == this.left){
    			this.left = node;
    		}else{ //如果不为空，递归添加
                this.left.add(add);
            }
    	}else{
    		if(null == this.right){
                this.right = node;
            }else{
                this.right.add(node);
            }
    	}
    }
    ```

* **二叉排序树的删除**：三种情况

  1. 删除叶子节点
  2. 删除只有一个子树的节点
  3. 删除有两颗子树的节点

  ```
  思路分析：
  第一种情况：删除叶子节点[可以归属于第二种情况]
  	1.先找到要删除的节点targetNode以及父节点parent
      2.确定targetNode是parent的左子节点还是右子节点
      3.根据情况来对应删除
      左子节点 parent.left = null;
      右子节点 parent.right = null;
  第二种情况：删除只有一个子树的节点
  	1.先找到要删除的节点targetNode以及父节点parent
      2.确定targetNode是parent的左子节点还是右子节点
      3.确定target有左子树还是右子树
      4.如果有左子树
      	1.如果targetNode是parent右子节点
      	parent.right = targetNode.left;
      	2.如果targetNode是parent左子节点
      	parent.left = targetNode.left;
      5.如果有右子树
      	1.如果targetNode是parent右子节点
      	parent.right = targetNode.right;
      	2.如果targetNode是parent左子节点
      	parent.left = targetNode.right;
  第三种情况：删除有两颗子树的节点
  	1.先找到要删除的节点targetNode以及父节点parent
  	2.从target的右子树中找到最小节点
  	3.用一个临时变量，将最小节点的值保存temp；
  	4.删除这个最小节点
  	5.targetNode.value = temp;
  ```

  ```java
   //删除节点
      public boolean deleteNode(int val){
          if(!isExist(val)){  //删除的节点存在
              return false;
          }
  
          Node node = findNode(val);  //找当前节点
          Node parent = findParent(val); //找父节点
  
          if(node.right == null){ //叶子节点或左单支
              
              if(parent == null){  //没有父节点==删除根节点
                  root = node.left;
              }else if(val < parent.val){
                  parent.left = node.left;
              }else{
                  parent.right = node.left;
              }
          }else if(node.left == null){ //叶子节点或右单支
              if(parent == null){
                  root = node.right;
              }else if(val < parent.val){
                  parent.left = node.right;
              }else{
                  parent.right = node.right;
              }
          }else{  //左右子节点都有
              int i = delRightMinNode(node); //找右子树最小值，并删除
              node.val = i;
          }
          return true;
      }
  
  	 //删除右子树最小的并返回
      private int delRightMinNode(Node node){
          if(null == node){
              throw new RuntimeException("空空以空空");
          }
  
          Node del = node.right;
          Node prev = node;
  
          while(del.left != null){
              prev = del;
              del = del.left;
          }
  
          if(del.val < prev.val){
              prev.left = del.right;
          }else{
              prev.right = del.right;
          }
  
          return del.val;
      }
  ```

  

### 2.AVL树

* 为什么会出现AVL树？

  * 如果给{1,2,3,4,5}这个数列，创建二叉排序树的话；他看起来更像一个单链表；同时对插入速度没有影响；查询速度甚至比单链表还要慢；所以出现了AVL树解决这些问题

* 概念：平衡二叉树也叫平衡二叉搜索树又被称为AVL树，可以保证查询效率

* 特点：它是一颗空树或**它的左右子树的高度差绝对值不超过1；并且左右子树也是平衡二叉树**

* **左旋转：**

  ![image-20200426092149440](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200426092149440.png)

  * ```java
    //左旋转
            public void shiftLeft(){
                //1.创建一个新的节点，值为当前节点的值
                Node node = new Node(val);
                //2.新节点的的左子树为当前节点的左子树
                node.left = this.left;
                //3新节点的右子树为当前节点的右子树的左子树
                node.right = this.right.left;
                //4.将当前节点的值设置为原来右孩子的值
                this.val = this.right.val;
                //5.把当前结点的右子树设置成当前结点右子树的右子树
                this.right = this.right.right;
                //6.把当前结点的左子树设置为新节点
                this.left = node;
            }
    ```

* **右旋转：**

  ![image-20200426092201111](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200426092201111.png)

  * ```java
     //右旋转
            public void shiftRight(){
                //1.创建一个新的节点，值为当前节点
                Node node = new Node(val);
                //2.新节点的的左子树为当前节点的左子树
                node.right = this.right;
                //3新节点的右子树为当前节点的右子树的左子树
                node.left = this.left.right;
                //4.将当前节点的值设置为原来他右孩子的值
                this.val = this.left.val;
                //5.把当前结点的右子树设置成当前结点右子树的右子树
                this.left = this.left.left;
                //6.把当前结点的左子树设置为新节点
                this.right = node;
            }
    ```

* **双旋转**

  ![image-20200426092110649](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200426092110649.png)

  * 双旋转让我们体现在完整代码中

```java
public class BinarySortTree {

    //树节点结构
    private static class Node{
        int val;
        Node right;
        Node left;

        public Node(int val) {
            this.val = val;
        }

        //左旋转
        public void shiftLeft(){
            //1.创建一个新的节点，值为当前节点的值
            Node node = new Node(val);
            //2.新节点的的左子树为当前节点的左子树
            node.left = this.left;
            //3新节点的右子树为当前节点的右子树的左子树
            node.right = this.right.left;
            //4.将当前节点的值设置为原来右孩子的值
            this.val = this.right.val;
            //5.把当前结点的右子树设置成当前结点右子树的右子树
            this.right = this.right.right;
            //6.把当前结点的左子树设置为新节点
            this.left = node;
        }

        //右旋转
        public void shiftRight(){
            //1.创建一个新的节点，值为当前节点
            Node node = new Node(val);
            //2.新节点的的左子树为当前节点的左子树
            node.right = this.right;
            //3新节点的右子树为当前节点的右子树的左子树
            node.left = this.left.right;
            //4.将当前节点的值设置为原来他右孩子的值
            this.val = this.left.val;
            //5.把当前结点的右子树设置成当前结点右子树的右子树
            this.left = this.left.left;
            //6.把当前结点的左子树设置为新节点
            this.right = node;
        }
    }

    //根节点
    private Node root;

    //空构造函数
    public BinarySortTree() {
    }

    //是否存在节点
    private boolean isExist(int val){
        Node cur = root;

        while(null != cur){
            if(val == cur.val){
                return true;
            }else if(val < cur.val){
                cur = cur.left;
            }else{
                cur = cur.right;
            }
        }
        return false;
    }

    //添加节点
    public boolean add(int val){
        if(isExist(val)){
            return false;
        }

        if(null == root){
            root = new Node(val);
            return true;
        }

        Node parent = findParent(val);
        if(val < parent.val){
            parent.left = new Node(val);
        }else{
            parent.right = new Node(val);
        }
        
        //双旋转
        if(height(root.right) - height(root.left) > 1){ 
            if(root.right!=null && height(root.right.left)>height(root.right.right)){  //
                root.right.shiftRight();
            }
            root.shiftLeft();
            return true;
        }
        if(height(root.left) - height(root.right) > 1){
            if(root.left != null && height(root.left.right) > height(root.left.left)){
                root.left.shiftLeft();
            }
            root.shiftRight();
            return true;
        }
        return true;
    }

    //求当前结点的高度
    public int height(Node node){
        if(null == node){
            return 0;
        }
        return Math.max(height(node.right),height(node.left))+1;
    }
    //寻找父节点
    private Node findParent(int val){
            Node prev = null;

            Node cur = root;

            while(null != cur){
                if(val < cur.val){
                    prev = cur;
                    cur = cur.left;
                }else if(val > cur.val){
                    prev = cur;
                    cur = cur.right;
                }else{
                    break;
                }
            }

        return prev;
    }

    //寻找当前节点
    private Node findNode(int val){
        Node cur = root;

        while(null != cur){
            if(val < cur.val){
                cur = cur.left;
            }else if(val > cur.val){
                cur = cur.right;
            }else{
                return cur;
            }
        }
        return null;
    }

    //中序遍历
    public void infOrder(){
        infOrder(root);
    }
    private void infOrder(Node node){
        if(null != node){
            infOrder(node.left);
            System.out.println(node.val);
            infOrder(node.right);
        }
    }

    //最大数
    public int maxNum(){
        if(null == root){
            throw new RuntimeException("空树");
        }

        Node cur = root;

        while(null != cur.right){
            cur = cur.right;
        }

        return cur.val;
    }

    //最小数
    public int minNum(){
        if(null == root){
            throw new RuntimeException("空树");
        }

        Node cur = root;

        while(null != cur.left){
            cur = cur.left;
        }

        return cur.val;
    }

    //删除节点
    public boolean deleteNode(int val){
        if(!isExist(val)){
            return false;
        }

        Node node = findNode(val);
        Node parent = findParent(val);

        if(node.right == null){
            if(parent == null){
                root = node.left;
            }else if(val < parent.val){
                parent.left = node.left;
            }else{
                parent.right = node.left;
            }
        }else if(node.left == null){
            if(parent == null){
                root = node.right;
            }else if(val < parent.val){
                parent.left = node.right;
            }else{
                parent.right = node.right;
            }
        }else{
            int i = delRightMinNode(node);
            node.val = i;
        }
        return true;
    }

    //删除右子树最小的并返回
    private int delRightMinNode(Node node){
        if(null == node){
            throw new RuntimeException("空空以空空");
        }

        Node del = node.right;
        Node prev = node;

        while(del.left != null){
            prev = del;
            del = del.left;
        }

        if(del.val < prev.val){
            prev.left = del.right;
        }else{
            prev.right = del.right;
        }

        return del.val;
    }
    //测试方法
    public static void main(String[] args) {
        BinarySortTree binarySortTree = new BinarySortTree();

        int[] arr = {2,3,9,5,7,1};
        for (int i = 0; i < arr.length; i++) {
            binarySortTree.add(arr[i]);
        }

        binarySortTree.infOrder();
        System.out.println("最大数："+binarySortTree.maxNum());
        System.out.println("最小数："+binarySortTree.minNum());

        System.out.println(binarySortTree.deleteNode(10));
        binarySortTree.infOrder();
    }
}
```

* 二叉树的操作效率比较高，但是也存在问题：

  1. 二叉树需要**加载到内存中**，如果二叉树的节点少，没有什么问题。但是如果二叉树的节点很多(比如1亿)，就存在下面问题：
  2. **问题1：**在构建二叉树时，需要**多次进行I/O操作**(海量数据存在与数据库或文件中)，节点海量，构建二叉树时，速度有影响
     * 在磁盘读取的场景下，一个数据被用到，它周围的数据被用到的可能性也很大。磁盘存储的最小单位为页(每页默认为4K)，所以磁盘读取的过程中，会采取预读取的策略，比如即使只要1字节的数据，也会把该数据所在的页的所有数据读取到，以备后用
  3. **问题2：**节点海量，也会造成**二叉树的高度很大**，会降低操作速度

  **所以推出B树，B+树**

### 4. B树(B = Balance)

* 概念：B-Tree，俗名：B树，别名：B-树、B减树。它是一种平衡多路查找树(不是二路哦~)，先看下它的结构：

  ![image-20200426102157207](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200426102157207.png)

* 一颗m阶的B树特性如下：

  1. 树中每个节点最多含有m个孩子；
  2. 除根节点和叶子节点外，它每个节点至少有ceil(m/2)个子节点；
  3. 若根节点不是叶子节点，则至少有2个子节点；
  4. 每个非叶子节点包含n个关键字信息：(n, p0,p1,……,pn, k1,k2， ……，kn)，其中：
     *  k为关键字，且关键字升序排序
     * p为指向子节点的指针
     *  有n个关键字的父节点，拥有n+1个子节点(类似于n个数划分成了n+1个数据范围)
  5. **每个节点的关键字（叶子节点和非叶子节点都存放数据）**，都记录着对应的数据在磁盘中的位置，即数据在每个节点上。

* 下面，咱们来模拟下查找文件29的过程：

  1. 根据根结点指针找到文件目录的根磁盘块1，将其中的信息导入内存。【磁盘IO操作 1次】    
  2. 此时内存中有两个文件名17、35和三个存储其他磁盘页面地址的数据。根据算法我们发现：17<29<35，因此我们找到指针p2。
  3. 根据p2指针，我们定位到磁盘块3，并将其中的信息导入内存。【磁盘IO操作 2次】    
  4. 此时内存中有两个文件名26，30和三个存储其他磁盘页面地址的数据。根据算法我们发现：26<29<30，因此我们找到指针p2。
  5. 根据p2指针，我们定位到磁盘块8，并将其中的信息导入内存。【磁盘IO操作 3次】    
  6. 此时内存中有两个文件名28，29。根据算法我们查找到文件名29，并定位了该文件内存的磁盘地址。

     分析上面的过程，发现需要3次磁盘IO操作和3次内存查找操作。关于内存中的文件名查找，由于是一个有序表结构，可以利用折半查找提高效率。至于IO操作是影响整个B树查找效率的决定因素。

     当然，如果我们使用平衡二叉树的磁盘存储结构来进行查找，磁盘4次，最多5次，而且文件越多，B树比平衡二叉树所用的磁盘IO操作次数将越少，效率也越高.


### 5. B+树

* B+树是B树的一种变体，也是一个多路平衡查找树，与B树的区别如下：

  1. 每个节点最多含有m个关键字；

  2. 所有的叶子节点包含了全部的关键字信息，且叶子节点本身按照关键字顺序相连；(B树的叶子节点并没有包含全部的关键字)

  3. 所有非叶子节点可以看成索引部分，节点中含有其子节点中最大(最小)关键字；(B树中子节点不包含父节点的关键字)

  4. 所有的数据仅存在叶子节点；(B树的每个节点都会存储数据)；
     下图为B+树的结构图：

     ![image-20200426104513866](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200426104513866.png)

### 6. B*树

​	B*-tree是B+-tree的变体，在B+树的基础上(所有的叶子结点中包含了全部关键字的信息，及指向含有这些关键字记录的指针)，B*树中非根和非叶子结点再增加指向兄弟的指针；B*树定义了非叶子结点关键字个数至少为(2/3)*M，即块的最低使用率为2/3（代替B+树的1/2）。

​	![image-20200426104932912](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200426104932912.png)

## 面试题：

### 为什么说B+树比B树更适合做文件索引或数据库索引？

1. B+树的磁盘读写代价更低。B+树的每个节点并不存储关键字的指针信息，因此节点相比较与B树更小。如果把所有节点放在盘块儿中，那么1个盘块儿包含更多的关键字，相对来说IO的读写次数也就降低了。
    举个例子，假设磁盘中的一个盘块容纳16bytes，而一个关键字2bytes，一个关键字具体信息指针2bytes。一棵9阶B-tree(一个结点最多8个关键字)的内部结点需要2个盘快。而B+ 树内部结点只需要1个盘快。当需要把内部结点读入内存中的时候，B 树就比B+ 树多一次盘块查找时间(在磁盘中就是盘片旋转的时间)。
2. B+树查询效率更加稳定。任何关键字的查找必须走一条从根节点到叶子节点的路径，所有关键字的查询路径相同，所以查询效率相当。
3. B+树叶子节点的链表结构，更方便范围查询。只需要遍历叶子节点，就可以实现整颗树的遍历。而数据库中基于范围的查询是很频繁的，B树在这种查询场景下，效率低下。

### 7. 红黑树

* 概念：首先红黑树是一颗二叉搜索树+增加节点的颜色限制以及性质约束 --->保证：最长路径中节点个数一定不会超过最短路径中节点个数的两倍就认为其近似平衡

* 性质：
  1. 每个节点不是红色就是黑色
  2. 根节点一定是黑色的
  3. 没有连在一起的红色节点
  4. 每条路径中黑色节点个数是一样的
  5. 所有叶子节点(指空节点)都是黑色的