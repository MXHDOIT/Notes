# Vue

### 1.Vue基础

*  vue 简介

  1. 是JavaScript的框架
  2. 简化了Dom操作
  3. 响应式的数据驱动

* 基础知识

  1. 导入开发版本的Vue.js

     ```html
      <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
     ```

  2. 创建Vue的实例，设置el属性和data属性

  3. 使用简洁的模板语法把数据渲染到页面上

     * Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统

```html
<!--例子：-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>vue基础</title>
</head>
<body>
  <div id="app">
    {{message}}
  </div>
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    var app = new Vue({
      el:'#app',
      data:{
        message:'Hello Vue!'
      }
    })
  </script>
</body>
</html>
```

### 2. el挂载点

* Vue实例的作用范围：
  *  Vue会管理el选项命中的元素及其内部的后代元素
* 可以使用其他的选择器，但是建议使用ID选择器（唯一）
* 可以使用其他的双标签，但是不能使用HTML和BODY

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>el挂载点</title>
</head>
<body>
    {{message}}
    <div id="app" class="app">
        {{message}}
        <span>{{message}}</span>
    </div>
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            //el:".app",
            //el:"div",
            data:{
                message:"华山剑法！"
            }
        })
    </script>
</body>
</html>
```

### 3.data数据对象

* Vue中用到的数据定义在data中
* data中可以定义复杂类型的数据
* 渲染复杂类型数据时，遵守js的语法即可

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>data数据对象</title>
</head>
<body>
    <div id="app">
        <span>{{message}}</span>
        <h3>{{school.name}}  {{school.phone}}</h3>
        <ul>
            <li>{{campus[0]}}</li>
            <li>{{campus[1]}}</li>
            <li>{{campus[2]}}</li>
            <li>{{campus[3]}}</li>
        </ul>
    </div>
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>  
        var app = new Vue({
            el:"#app",
            data:{
                message:"华山论剑",
                school:{
                    name:"张三",
                    phone:"12345678910"
                },
                campus:["one","two","three","four"]
            }
        })
    </script>
</body>
</html>
```

###  4.v-text指令

* v-text指令的作用是：设置标签的内容（textContent）
* 默认写法会替换全部内容，使用差值表达式{{}}可以替换指定的内容
* 内部支持写表达式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-tetx指令</title>
</head>
<body>
    <div id="app">
        <h2 v-text="message+'!'">程序员</h2>
        <h3>{{message+"!"}}程序员</h3>
    </div>
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>  
        var app = new Vue({
            el:"#app",
            data:{
                message:"万事万物皆对象",
            }
        })
    </script>
</body>
</html>
```

### 5.v-html指令

*  v-html指令的作用是：设置元素的innerHTML
* 内容中有HTML结构会被解析为标签
* v-text指令无论内容是什么都只会解析成文本

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-html指令</title>
</head>
<body>
    <div id="app">
        <h4 v-html="message"></h4>
        <h4 v-text="message"></h4>
    </div>
        <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data:{
                //message:"百度一下"
                message:"<a href='http://www.baidu.com'>百度一下</a>"
            }
        })
    </script>
</body>
</html>
```

### 6.1 v-on指令

* v-on指令的作用是：**为元素绑定事件**
* 事件名不需要写on
* 指令可以写为@
* 绑定的方法定义在methods属性中
* 方法内部通过this关键字可以访问定义在data中数据

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-on指令</title>
</head>
<body>
    <div id="app">
        <input type="button" value="v-on指令" v-on:click="doIt">
        <input type="button" value="v-on简写" @click="doIt">
        <input type="button" value="v-on双击" @dblclick="doIt">
        <h3 @click="changeFood">{{food}}</h3>
    </div>
     <!-- 开发环境版本，包含了有帮助的命令行警告 -->
     <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data:{
                food:"烤鸡翅膀"
            },
            methods:{
                doIt:function(){
                    alert("点我干啥！")
                },
                changeFood:function(){
                    this.food+="我最爱吃！"
                }
            }
        })
    </script>
</body>
</html>
```

### 6.2 v-on补充

*  事件绑定的方法写成函数调用的形式，可以传入自定义参数
* 定义方法时需要定义形参来接收传入的实参
* 事件的后面跟上.修饰符可以对事件进行限制
* .enter可以限制触发的按键为回车
* 事件修饰符有多种

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-on指令补充</title>
</head>
<body>
    <div id="app">
        <input type="button" value="点击" @click="doIt('老铁',666)"> 
        <input type="text" @keyup.enter="sayHi">
    </div>
        <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            methods:{
                doIt:function(p1,p2){
                    console.log("点击！！！");
                    console.log(p1);
                    alert(p2);
                },
                sayHi:function(){
                    alert("输入完毕！！！")
                }
            }
        })
    </script>
</body>
</html>
```

### 7.v-show指令

* v-show指令的作用是：**根据真假切换元素的显示状态**
* 原理是修改元素的display，实现显示隐藏
* 指令后面的内容，最终都会解析为布尔值
* 值为true元素显示，值为false元素隐藏
* 数据改变之后，对元素显示状态会同步更新

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-show指令</title>
</head>
<body>
    <div id="app">
        <img src="shaosiming.jpg" v-show="isShow">
        <button @click="changeIsShow">点击切换显示状态</button>
        <img src="shaosiming.jpg" v-show="age>=18">
        <button @click="changeAge">点击增加年龄进行显示</button>
        <button @click="changeAge2">点击减少年龄进行隐藏</button>
    </div>
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data:{
                isShow:false,
                age:17
            },
            methods:{
                changeIsShow:function(){
                    this.isShow = !this.isShow
                },
                changeAge:function(){
                    this.age++
                },
                changeAge2:function(){
                    this.age=17
                }
            }
        })
    </script>
</body>
</html>
```

### 8.v-if指令

* v-if指令的作用是：根据表达式的真假切换元素的显示状态
* 本质是通过操纵dom元素来切换显示状态
* 表达式的值为true，元素存在于dom树中，为false，从dom树中移除
* 频繁切换v-show，反之使用v-if，前者切换的消耗较小

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-if指令</title>
</head>
<body>
    <div id="app">
        <button @click="changeIsShow">点击切换显示</button>
        <p v-if="isShow">天下武功，唯快不破！</p>
        <button @click="changeTemperature">点击隐藏温度</button>
        <button @click="changeTemperature2">点击显示温度</button>
        <p v-show="temperature>30">热死了！！！</p>
    </div>
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data:{
                isShow:false,
                temperature:20
            },
            methods:{
                changeIsShow:function(){
                    this.isShow = !this.isShow
                },
                changeTemperature:function(){
                    this.temperature = 20
                },
                changeTemperature2:function(){
                    this.temperature = 35
                }
            }
        })
    </script>
</body>
</html>
```

### 9.v-bind指令

* v-bind指令的作用是：为元素绑定属性
* 完整写法是：v-bind：属性名
* 简写的话可以直接省略v-bind，只保留：属性名
* 需要动态的增删class建议使用对象的方式

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-bind指令</title>
    <style>
        .active{
            border:10px  solid red;
        }
    </style>
</head>
<body>
    <div id="app">
        <img v-bind:src="imgSrc">
        <br>
        <img :src="imgSrc" :title="imgTitle+'!!!!'" :class="isActive?'active':''" @click="changeActive">
        <br>
        <img :src="imgSrc" :title="imgTitle+'!!!'" :class="{active:isActive}" @click="changeActive">
    </div>
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data:{
                imgSrc:"shaosiming.jpg",
                imgTitle:"少司命",
                isActive:false
            },
            methods:{
                changeActive:function(){
                    this.isActive = !this.isActive
                }
            }
        })
    </script>
</body>
</html>
```

### 10.v-for指令

* v-for指令的作用是：根据数据生成列表结构
* 数组经常和v-for一起使用
* 语法是（item，index）in 数据
* item和index可以结合其他指令一起使用
* 数组长度的更新会同步到页面上，是响应式的

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-for指令</title>
</head>
<body>
    <div id="app">
        <button @click="add">添加蔬菜</button>
        <button @click="remove">减少蔬菜</button>
        <ul>
            <li v-for="(item,index) in arr">
                {{index+1}}四大神兽：{{item}}
            </li>
        </ul>
        <h2 v-for="item in vegetables" v-bind:title="item.name">
            {{item.name}}
        </h2>
    </div>
        <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data:{
                arr:["青龙","白虎","朱雀","玄武"],
                vegetables:[
                    {name:"青菜"},
                    {name:"白菜"}
                ]
            },
            methods:{
                add:function(){
                    this.vegetables.push({name:"胡萝卜"});
                },
                remove:function(){
                    this.vegetables.shift();
                }
            }
        })
    </script>
</body>
</html>
```

### 11.v-model指令

* v-model指令的作用是便捷的设置和获取表单元素的值
* 绑定的数据会和表单元素的值相关联
* 绑定的数据<->表单元素的数据

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <input type="button" value="设置message" @click="setMessage">
        <input type="text" v-model="message" @keyup.enter="getMessage">
        <h3>{{message}}</h3>
    </div>
        <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data:{
                message:"你好"
            },
            methods:{
                getMessage:function(){
                    alert(this.message)
                },
                setMessage:function(){
                    this.message="hello"
                }
            }
        })
    </script>
</body>
</html>
```

### 12.axios基础

* axios必须先导入才可以使用

  ```html
  <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
  ```

* 语法：

  ```html
  axios.get(地址？key=value&key2=value2).then(function(response){},function(err){})
  axios.post(地址,{key:value,key2:value2}).then(function(response){},function(err){})
  ```

* 使用get或post方法即可发送对应的请求

* then方法中的回调函数会在请求成功或失败时触发

* 通过回调函数的形参可以获取响应内容或错误信息

* 了解axios更多用法请上：https://github.com/axios/axios

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <input type="button" value="axios:get请求" class="get">
    <input type="button" value="axios:post请求" class="post">
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script>
        /*
            接口1:随机笑话
            请求地址:https://autumnfish.cn/api/joke/list
            请求方法:get
            请求参数:num(笑话条数,数字)
            响应内容:随机笑话
        */
        document.querySelector(".get").onclick = function () {
            axios.get("https://autumnfish.cn/api/joke/list?num=4")
            // axios.get("https://autumnfish.cn/api/joke/qewqeq?num=6")
            .then(function (response) {
                console.log(response);
              },function(err){
                  console.log(err);
              })
        }
        /*
             接口2:用户注册
             请求地址:https://autumnfish.cn/api/user/reg
             请求方法:post
             请求参数:username(用户名,字符串)
             响应内容:注册成功或失败
         */
         document.querySelector(".post").onclick = function(){
            axios.post("https://autumnfish.cn/api/user/reg",{username:"asgdyufgq"})
            .then(function(response){
                console.log(response);
            },function(err){
                console.log(err);
            })
        }
    </script>
</body>
</html>
```

### 13.axios加Vue

* axios回调函数中的**this已经改变**，无法访问到data中的数据
* 把this保存起来，回调函数中直接使用保存的this即可
* 和本地应用最大的区别就是改变了数据来源

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>axios加vue</title>
</head>
<body>
    <div id="app">
        <input type="button" value="获取笑话" @click="getJoke">
        <p>{{joke}}</p>
    </div>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        /*
            接口:随机获取一条笑话
            请求地址:https://autumnfish.cn/api/joke
            请求方法:get
            请求参数:无
            响应内容:随机笑话
        */
        var app = new Vue({
            el:"#app",
            data:{
                joke:"笑话"
            },
            methods:{
                getJoke:function(){
                    var that = this;
                    axios.get("https://autumnfish.cn/api/joke").then(function(response){
                        that.joke = response.data
                    },function(err){})
                }
            }
        })
    </script>
</body>
</html>
```

