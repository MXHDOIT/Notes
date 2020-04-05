# String类：引用类型

eg1：


```java
public static void main(String[] args){
    String str1 = "abcde";  //直接赋值
    System.out.println(str1);
    String str2 = new String("abcde");  
    System.out.println(str2);

    char[] array = {'a','b','c','d','e'};
    String str3 = new String(array);
    System.out.println(str3);

    System.out.println(str1 == str2);  //false
    System.out.println(str1 == str3);  //false
}
```

**双引号中的内容都放在常量池；**

eg2：

        

```java
public static void main(String[] args){
	String str = "abcde";  //直接赋值
    String str2 = new String("abcde");
    char[] array = {'a','b','c','d','e'};
    String str3 = new String(array);
    String str4 = "ab"+"cde";  //编译期已经确定是"abcde"	
    String str5 = "ab"+ new String("cde");    
	System.out.println(str == str2);  //false
   	System.out.println(str == str3);  //false
    System.out.println(str == str4);  //true
    System.out.println(str == str5);  //false
    System.out.println(str2 == str3);  //false
}
```

**String使用==比较并不是在比较字符串内容, 而是比较两个引用是否是指向同一个对象；**

eg3：

```java
String str1 = new String("Hello");
String str2 = new String("Hello");
System.out.println(str1.equals(str2));  //true
```

eg4：

```java
String str = new String("Hello");
// 方式一
System.out.println(str.equals("Hello"));  //如果str为空，就会抛出空指针异常
// 方式二
System.out.println("Hello".equals(str));
```

*** **intern()方法手动入池

```java
// 该字符串常量并没有保存在对象池之中
String str1 = new String("hello") ; 
String str2 = "hello" ; 
System.out.println(str1 == str2); 
// 执行结果：false 
String str1 = new String("hello").intern() ; 
String str2 = "hello" ; 
System.out.println(str1 == str2); 
// 执行结果：true
```

面试题：**请解释String类中两种对象实例化的区别**
1. 直接赋值：只会开辟一块堆内存空间，并且该字符串对象可以自动保存在对象池中以供下次使用。
2. 构造方法：会开辟两块堆内存空间，其中一块成为垃圾空间，不会自动保存在对象池中，可以使用
intern()方法手工入池。

#### 修改字符串

1.借助原字符串，创建新的字符串
eg:

```java
String str = "Hello"; 
str = "h" + str.substring(1);   
//substring() 提取一个子串；
//substring(1)：从1号下标开始提取子串
System.out.println(str); 
// 执行结果：hello
```

2.使用 "反射" 操作破坏封装, 访问一个类内部的 private 成员.
反射：
	Java的自省过程；
	指在程序运行过程中，获取/修改某个对象的详细信息（类型、属性等），相当于让一个对象更好的“认清自己”；
eg:

```java
public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException {
        String str = "Hello";
    // 获取 String 类中的 value 字段. 这个 value 和 String 源码中的 value 是匹配的.
        Field valueField = String.class.getDeclaredField("value");
    // 将这个字段的访问属性设为 true
        valueField.setAccessible(true);
    // 把 str 中的 value 属性获取到.
        char[] value = (char[]) valueField.get(str);
    // 修改 value 的值
        value[0] = 'h';
        System.out.println(str);
    }
```

* copyValueOf();  //将数组转换成字符串
* @Deprecated 是一个注解，表示此方法已经被弃用，但是仍然可以正常使用；

#### 字节与字符串：

eg:

```java
public static void main(String[] args) throws UnsupportedEncodingException {
    byte[] bytes = {97,98,99,100};
    String str = new String(bytes,1,2);
    System.out.println(str);   
   	String str2 = "abc";
    byte[] bytes1 = str2.getBytes();
    System.out.println(Arrays.toString(bytes1));

    String str3 = "高";
    byte[] bytes2 = str3.getBytes("utf8");
    System.out.println(Arrays.toString(bytes2));
}
```


 **那么何时使用 byte[], 何时使用 char[] 呢?**

* byte[] ：
  		把String按照一个字节一个字节的方式处理；
  		这种适合在网络传输,数据存储这样的场景下使用；
  		更适合针对二进制数据来操作。（打开后不认识的文件就是二进制数据）
* char[] ：
  		把String按照一个字符一个字符的方式处理；
  		更适合针对文本数据来操作,尤其是包含中文的时候。（打开后认识的文件就是文本数据）
  		

#### 字符串比较：

* equals：区分大小写；
* equalsIanoreCase：不区分大小写；
* compareTo：比较字符串的大小关系；
			 A.compareTo(B):A和B比较；
			 >0：A>B
			 =0：A=B
			 <0：A<B
	

#### 字符串查找：

* contains(CharSequence s)：判断一个字符串是否存在；
			String str = "helloworld" ; 
			System.out.println(str.contains("world")); // true
* indexOf(String str)：从头开始查找指定字符串的位置，找到了返回其下标，找不到返回-1；
		   如果有多个，返回第一个的下标；
			String str = "helloworld" ; 
			System.out.println(str.indexOf("world")); // 5,w开始的索引
			System.out.println(str.indexOf("bit")); // -1，没有查到
			if (str.indexOf("hello") != -1) { 
				System.out.println("可以查到指定字符串！"); 
			}
* indexOf(String str,int fromIndex):从指定位置开始查找；
			String str = "helloworld" ; 
			System.out.println(str.indexOf("l")); // 2 
			System.out.println(str.indexOf("l",5)); // 8 
			System.out.println(str.lastIndexOf("1")); //8
* lastIndexOf(String str):从后往前找；
* lastIndexOf(String str,int fromIndex):从指定位置从后往前找；
* startsWith(String prefix):判断字符串是否以指定字符开头；
* startsWith(String prefix，int toffest)：从指定位置判断是否以指定字符开头；
* endsWith(String suffix):判断字符串是否以指定字符结尾；
			String str = "**@@helloworld!!" ; 
			System.out.println(str.startsWith("**")); // true 
			System.out.println(str.startsWith("@@",2)); // ture 
			System.out.println(str.endsWith("!!")); //true;

#### 字符串替换：

* replaceAll(String regex,String replacement):替换所有的指定内容；
* replaceFirst(String regex,String replacement):替换首个内容；
			String str = "helloworld" ; 
			System.out.println(str.replaceAll("l", "_")); 
			System.out.println(str.replaceFirst("l", "_"));
（字符串是不可变对象，所以字符串的替换并不修改当前字符穿，而是产生一个新的字符串）

字符串拆分：
* split(String regex):将字符串全部拆分；
			String str = "hello world hello bit" ; 
			String[] result = str.split(" ") ; // 按照空格拆分
			for(String s: result) { 
				System.out.println(s); 
			}
* split(String regex,int limit):将字符串部分拆分，该数组的长度就是limit极限；
			String str = "hello world hello bit" ; 
			String[] result = str.split(" ",2) ; 
			for(String s: result) { 
				System.out.println(s); 
			}

#### 字符串截取：

* substring(int beginIndex):从指定索引截取到结尾；
* substring(int beginIndex,int endIndex):截取部分内容；
			String str = "helloworld" ; 
			System.out.println(str.substring(5)); 
			System.out.println(str.substring(0,5));

（KMP算法：查找子串在主串当中的位置）

其他操作方法：
* trim():去掉字符串中的左右空格，保留中间的空格；
			String str = " hello world " ; 
			System.out.println("["+str+"]"); 
			System.out.println("["+str.trim()+"]");
	
* toUpperCase():字符串转大写；
* toLowerCase():字符串转小写；
			String str = " hello%$$%@#$%world 哈哈哈 " ;
			System.out.println(str.toUpperCase());
			System.out.println(str.toLowerCase());
	
* (native) intern():字符串入池操作；
* concat(String str):字符串连接，等同于"+"，不入池；

* length():取得字符串长度；
			String str = " hello%$$%@#$%world 哈哈哈 " ;
			System.out.println(str.length());

* isEmpty():判断是否为空字符串，但不是null，而是长度为0；
			System.out.println("hello".isEmpty());
			System.out.println("".isEmpty());
			System.out.println(new String().isEmpty());

### StringBuffer和StringBuilder
两者大部分的功能是相同的，这里主要介绍StringBuffer
eg：观察StringBuffr的使用

```java
public class Test{
public static void main(String[] args) {
	StringBuffer sb = new StringBuffer();
	sb.append("Hello").append("World");
	fun(sb);
	System.out.println(sb);
}
public static void fun(StringBuffer temp) {
	temp.append("\n").append("www.bit.com.cn");
	}
}
```


* String和StringBuffer最大的区别在于：String的内容无法修改，而StringBuffer的内容可以修改。频繁修改字符串的情况考虑使用StingBuffer。

**synchronized:被synchronized修饰——》线程安全；**
**StringBuffer适用于多线程；（被synchronized修饰）**
**StringBuilder适用于单线程；**

####  注意： 

* String和StringBuffer类不能直接转换。如果要想互相转换，可以采用如下原则:
* String变为StringBuffer:利用StringBuffer的构造方法或append()方法；
* StringBuffer变为String:调用toString()方法；
* 字符串反转：
	reverse();
			StringBuffer sb = new StringBuffer("helloworld");
			System.out.println(sb.reverse());
* 字符串删除：
	delete(int beginIndex,int endIndex):删除指定范围的内容
			StringBuffer sb = new StringBuffer("helloworld");
			System.out.println(sb.delete(5, 10));
* 插入数据：
	insert(int offset, 各种数据类型 b);
			StringBuffer sb = new StringBuffer("helloworld");
			System.out.println(sb.delete(5, 10).insert(0, "你好"));


**面试题：请解释String、StringBuffer、StringBuilder的区别:**
* String的内容不可修改，StringBuffer与StringBuilder的内容可以修改.
* StringBuffer与StringBuilder大部分功能是相似的
* StringBuffer采用同步处理，属于线程安全操作；而StringBuilder采用异步处理，属于线程不安全操作






