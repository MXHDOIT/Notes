# 原来这就是 `JSON`

#### 1. JSON是啥？

* **JSON** (JavaScript Object Notation) 是一种轻量级的数据交换格式。JSON 采用完全独立于语言的文本格式，而且很多语言都提供了对 json 的支持（包括 C, C++, C#, Java, JavaScript, Perl, Python 等）。 这样就使得 JSON 成为理想的**数据交换**格式（是客户端和服务器之间业务数据的传递格式）。 类似于Xml

#### 2.JSON 怎样使用？

1. JSON在**JavaScript**中使用：

   1. 定义JSON：

      * json 是由键值对组成，并且由花括号（大括号）包围。每个键由引号引起来，键和值之间使用冒号进行分隔， 多组键值对之间进行逗号进行分隔。

      ```javascript
      //定义
      var jsonObj = {
          "key1":12,
          "key2":"abc",
          "key3":true,
          "key4":[11,"arr",false],
          "key5":{ "key5_1" : 551, "key5_2" : "key5_2_value" },
          "key6":[{ "key6_1_1":6611, "key6_1_2":"key6_1_2_value"},{"key6_2_1":6621, "key6_2_2":"key6_2_2_value" }]
      };
      ```

      

   2. 访问JSON:

      * json 本身是一个对象。 json 中的 key 我们可以理解为是对象中的一个属性。 
      * json 中的 key 访问就跟访问对象的属性一样： **json 对象.key** 

      ```javascript
      
      alert(typeof(jsonObj));// object json 就是一个对象
      alert(jsonObj.key1); //12 
      alert(jsonObj.key2); //
      abc alert(jsonObj.key3); // true
      alert(jsonObj.key4);// 得到数组[11,"arr",false] 
      // json 中 数组值的遍历 
      for(var i = 0; i < jsonObj.key4.length; i++) { 
          alert(jsonObj.key4[i]);
      }
      alert(jsonObj.key5.key5_1);//551
      alert(jsonObj.key5.key5_2);//key5_2_value
      alert( jsonObj.key6 );// 得到 json 数组 
      // 取出来每一个元素都是 json 对象 
      var jsonItem = jsonObj.key6[0];
      // alert( jsonItem.key6_1_1 ); //6611 
      alert( jsonItem.key6_1_2 ); //key6_1_2_value
      ```

      

   3. JSON字符串与JSON对象的相互转换：

      1. 把 json 对象转换成为 json 字符串 ：JSON.stringify()
      2. 把 json 字符串转换成为 json 对象：JSON.parse()

      * 一般我们要操作 json 中的数据的时候，需要 json 对象的格式；一般我们要在客户端和服务器之间进行数据交换的时候，使用 json 字符串。

      ```javascript
      // 把 json 对象转换成为 json 字符串 
      var jsonObjString = JSON.stringify(jsonObj); 
      // 特别像 Java 中对象的 toString 
      alert(jsonObjString)
      // 把 json 字符串。转换成为 json 对象
      var jsonObj2 = JSON.parse(jsonObjString); 
      alert(jsonObj2.key1);// 12
      alert(jsonObj2.key2);// abc
      ```

2. JSON在**Java**中使用：

   * 使用：gson-2.2.4.jar包进行解析

   1. **javaBean** **和** **json** **的互转**

      ```java
      @Test 
      public void test1(){
          Person person = new Person(1,"国哥好帅!"); 
          // 创建 Gson 对象实例
          Gson gson = new Gson(); 
          // toJson 方法可以把 java 对象转换成为 json 字符串 
          String personJsonString = gson.toJson(person); 
          System.out.println(personJsonString); 
          // fromJson 把 json 字符串转换回 Java 对象 
          // 第一个参数是 json 字符串 
          // 第二个参数是转换回去的 Java 对象类型
          Person person1 = gson.fromJson(personJsonString, Person.class); 
          System.out.println(person1); 
      }
      ```

      

   2. **List** **和** **json** **的互转**

      ```java
      @Test 
      public void test2() {
          List<Person> personList = new ArrayList<>();
          personList.add(new Person(1, "国哥")); 
          personList.add(new Person(2, "康师傅"));
          Gson gson = new Gson(); 
          
          // 把 List 转换为 json 字符串 
          String personListJsonString = gson.toJson(personList); 
          System.out.println(personListJsonString);
          
          List<Person> list = gson.fromJson(personListJsonString, new PersonListType().getType()); 
          System.out.println(list); 
          Person person = list.get(0); 
          System.out.println(person);
      }
      ```

      

   3. **map** **和** **json** **的互转**

      ```java
      @Test
      public void test3(){ 
          Map<Integer,Person> personMap = new HashMap<>(); 
          personMap.put(1, new Person(1, "国哥好帅"));
          personMap.put(2, new Person(2, "康师傅也好帅")); 
          Gson gson = new Gson(); 
          
          // 把 map 集合转换成为 json 字符串 
          String personMapJsonString = gson.toJson(personMap); 
          System.out.println(personMapJsonString); 
          // Map<Integer,Person> personMap2 = gson.fromJson(personMapJsonString, new PersonMapType().getType()); 
          Map<Integer,Person> personMap2 = gson.fromJson(personMapJsonString, new TypeToken<HashMap<Integer,Person>>(){}.getType());
          System.out.println(personMap2); 
          
          Person p = personMap2.get(1); 
          System.out.println(p); 
      }
      ```

   

   

   # 总：

   

   <img class="" data-copyright="0" data-ratio="0.5515625" data-s="300,640" data-src="https://mmbiz.qpic.cn/mmbiz_jpg/2BGWl1qPxib0JftI9zEibicrOWANAQHhgSWWxiaicKUdHuSxojpibb3kNmHKMSZuT8TvtCV35rjOYmlRtJoFa9YhnFyQ/640?wx_fmt=jpeg" data-type="jpeg" data-w="1280" style="width: 677px !important; height: auto !important; visibility: visible !important;" _width="677px" src="https://mmbiz.qpic.cn/mmbiz_jpg/2BGWl1qPxib0JftI9zEibicrOWANAQHhgSWWxiaicKUdHuSxojpibb3kNmHKMSZuT8TvtCV35rjOYmlRtJoFa9YhnFyQ/640?wx_fmt=jpeg&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" crossorigin="anonymous" data-fail="0">

