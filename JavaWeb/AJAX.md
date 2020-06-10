# AJAX

### 1.什么是Ajax请求

* AJAX 即“**A**synchronous **J**avascript **A**nd **X**ML”（异步 JavaScript 和 XML），是指一种创建交互式网页应用的网页开发 技术。
* **ajax 是一种浏览器通过 js 异步发起请求，局部更新页面的技术。** 
* **Ajax 请求的局部更新，浏览器地址栏不会发生变化** 
* **局部更新不会舍弃原来页面的内容**

### 2.原生的Ajax请求：

* 使用XMLHttpRequest;

  ```javascript
  <script type="text/javascript"> 
      // 在这里使用 javaScript 语言发起 Ajax 请求，访问服务器 AjaxServlet 中 	
      javaScriptAjax function ajaxRequest() {
          // 1、我们首先要创建 XMLHttpRequest
          var xmlhttprequest = new XMLHttpRequest(); 
          // 2、调用 open 方法设置请求参数 
          xmlhttprequest.open("GET","http://localhost:8080/16_json_ajax_i18n/ajaxServlet?action=javaScriptAj ax",true) 
          // 4、在 send 方法前绑定 onreadystatechange 事件，处理请求完成后的操作。 
          xmlhttprequest.onreadystatechange = function(){
              if (xmlhttprequest.readyState == 4 && xmlhttprequest.status == 200) { 
                  var jsonObj = JSON.parse(xmlhttprequest.responseText); 
                  // 把响应的数据显示在页面上 
                  document.getElementById("div01").innerHTML = "编号：" + jsonObj.id + " , 姓名：" + jsonObj.name; 
              } 
          }
          // 3、调用 send 方法发送请求 
          xmlhttprequest.send();
      }
  </script>
  ```

### 3. jQuery中的Ajax请求

1. $.ajax 方法 

   ​	url 	表示请求的地址 

   ​	type 	表示请求的类型 GET 或 POST 请求 

   ​	data 	表示发送给服务器的数据 

   ​			格式有两种： 

   ​						一：name=value&name=value 	

   ​						二：{key:value} 

   ​	success 	请求成功，响应的回调函数 

   ​	dataType 	响应的数据类型 

   ​			常用的数据类型有： 

   ​					text 表示纯文本 

   ​					xml 表示 xml 数据 

   ​					json 表示 json 对象

   ```javascript
   $("#ajaxBtn").click(function(){
       $.ajax({ 
           url:"http://localhost:8080/16_json_ajax_i18n/ajaxServlet",
           // data:"action=jQueryAjax", 
           data:{action:"jQueryAjax"}, 
           type:"GET", success:function (data) { 
               // alert("服务器返回的数据是：" + data); 
               // var jsonObj = JSON.parse(data); 
               $("#msg").html("编号：" + data.id + " , 姓名：" + data.name); 
           },
           dataType : "json" 
       });
   });
   ```

   

2. $.get 方法和$.post 方法 

   url  	请求的 url 地址 

   data 	发送的数据 

   callback 	成功的回调函数 

   type 	返回的数据类型 

   ```javascript
   // ajax--get 请求
   $("#getBtn").click(function(){
       $.get(
        	"http://localhost:8080/16_json_ajax_i18n/ajaxServlet",
           "action=jQueryGet",
           function (data) {
           $("#msg").html(" get 编号：" + data.id + " , 姓名：" + data.name); },
           "json"); 
   }); 
   
   // ajax--post 请求
   $("#postBtn").click(function(){ 
       $.post(
      		"http://localhost:8080/16_json_ajax_i18n/ajaxServlet",
           "action=jQueryPost",
           function (data) {
               $("#msg").html(" post 编号：" + data.id + " , 姓名：" + data.name);},
           "json");
   });
   ```

   

3. ​	$.getJSON 方法 

   url 	请求的 url 地址 

   data 	发送给服务器的数据 

   callback 	成功的回调函数

   ```javascript
   // ajax--getJson 请求 
   $("#getJSONBtn").click(function(){ 
       $.getJSON(
         	"http://localhost:8080/16_json_ajax_i18n/ajaxServlet",
           "action=jQueryGetJSON",
           function (data) { $("#msg").html(" getJSON 编号：" + data.id + " , 姓名：" + data.name); 
          });
   });
   ```

   

4. 表单序列化 serialize() 

   * serialize()可以把表单中所有表单项的内容都获取到，并以 name=value&name=value 的形式进行拼接

   ```javascript
   // ajax 请求 
   $("#submit").click(function(){ // 把参数序列化 
       $.getJSON(
           "http://localhost:8080/16_json_ajax_i18n/ajaxServlet",
           "action=jQuerySerialize&" + $("#form01").serialize(),
           function (data) { $("#msg").html(" Serialize 编号：" + data.id + " , 姓名：" + data.name); 
        }); 
   });
   ```



**注意：**Ajax请求存在**跨域**问题；

​		跨域：当一个请求url的**协议、域名、端口**三者之间任意一个与当前页面url不同即为跨域

​		

|      **当前页面url**      | **被请求页面url**               | **是否跨域** | 原因                           |
| :-----------------------: | ------------------------------- | ------------ | ------------------------------ |
|   http://www.test.com/    | http://www.test.com/index.html  | 否           | 同源（协议、域名、端口号相同） |
|   http://www.test.com/    | https://www.test.com/index.html | 跨域         | 协议不同（http/https）         |
|   http://www.test.com/    | http://www.baidu.com/           | 跨域         | 主域名不同（test/baidu）       |
|   http://www.test.com/    | http://blog.test.com/           | 跨域         | 子域名不同（www/blog）         |
| http://www.test.com:8080/ | http://www.test.com:7001/       | 跨域         | 端口号不同（8080/7001）        |

解决方法：https://mp.weixin.qq.com/s?__biz=MzI4Njg5MDA5NA==&mid=2247485060&idx=3&sn=2fb60dea9ca3fde0aa53004f70ed7f6e&chksm=ebd74785dca0ce93cc1c2177bf6637106f3de13d9c0437b269f7dd594cc49e01e4c87dc63e6f&token=1755043505&lang=zh_CN###rd