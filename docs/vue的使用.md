# 1.Vue概述

## 1.1.早期前后端分离模式

早期的前后端分离开发模式是这样的：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <style>
            table {
                width: 600px;
                text-align: center;
                border-bottom: solid 2px #DDD;
                /* 合并边框 */
                border-collapse: collapse;
            }
            td,th {
                border-bottom: solid 1px #DDD;
                height: 40px;
            }
        </style>
    </head>
    <body>
        <h3>用户信息</h3>
        <table>
            <tr>
                <th>用户编号</th>
                <th>用户姓名</th>
                <th>用户性别</th>
                <th>用户年龄</th>
            </tr>
            <tbody id="userBox">
                <!-- 这里是动态内容 -->
            </tbody>
        </table>
        <script>
            let userArr = [{
                userId: 1,
                userName: '张三',
                userSex: '男',
                userAge: 20
            }, {
                userId: 2,
                userName: '李四',
                userSex: '女',
                userAge: 21
            }, {
                userId: 3,
                userName: '王五',
                userSex: '男',
                userAge: 22
            }]
            let userBox = document.getElementById('userBox');
            let str = '';
            for (let i = 0; i < userArr.length; i++) {
                str += '<tr>' +
                           '<td>' + userArr[i].userId + '</td>' +
                           '<td>' + userArr[i].userName + '</td>' +
                           '<td>' + userArr[i].userSex + '</td>' +
                           '<td>' + userArr[i].userAge + '</td>' +
                       '</tr>';
            }
            userBox.innerHTML = str;
        </script>
    </body>
</html>
```

以上开发模式的特点是：

1. 必须直接操作DOM，动态改变DOM对象的内容与样式。

2. 必须要进行大量的字符串拼接，才能拼接出动态内容，然后绑定到视图（html）上。

3. 这种绑定方式是单向的。即：使用javascript将动态数据绑定到DOM上，但是用户操作DOM引起的变化，却不能反映到javascript的动态数据上。
   
   ## 1.2.MVVM框架
   
   MVVM框架分为三个部分：分别是M（Model，模型层 ），V（View，视图层），VM（ViewModel，V与M连接的桥梁，也可以看作为控制器）

4. M：模型层，主要负责业务数据相关；

5. V：视图层，顾名思义，负责视图相关，细分下来就是html+css层；

6. VM：V与M沟通的桥梁，负责监听M或者V的修改，是实现MVVM双向绑定的要点；
   MVVM支持双向绑定，意思就是当M层数据进行修改时，VM层会监测到变化，并且通知V层进行相应的修改，反之修改V层则会通知M层数据进行修改，以此也实现了视图与模型层的相互解耦；
   ![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p01_00.png)
   
   ## 1.3.Vue简介
   
   Vue.js : 渐进式JavaScript 框架（官网说明），它就是一个前端MVVM框架。 Vue.js的作者为Evan You（尤雨溪），任职于Google Creative Lab。 Vue的主要特点就和它官网 https://cn.vuejs.org/ 所介绍的那样： （1） 简洁 （2） 轻量 （3）快速 （4） 数据驱动 （5） 模块友好 （6） 组件化
   
   # 2.Vue.js快速入门 Hello World
   
   ## 2.1.安装Vue环境
   
   Vue官方安装教程：https://cn.vuejs.org/v2/guide/
   请使用Vue官方安装教程讲解。
   
   ## 2.2.Hello World程序
   
   ```html
   <!DOCTYPE html>
   <html>
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>
        <div id="app">
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script>
            //定义全局变量vm
            let vm = new Vue({
                el: '#app',
                data: {
                    msg: 'Hello World!'
                }
            });
        </script>
    </body>
   </html>
   ```
   
   # 3.vue双向数据绑定
   
   修改html展示代码
   
   ```html
   <div  id="app">
    <input type="text"  v-model="msg" />
   ```

<span>{{msg}}</span>

        </div>

<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello World!'
        }
    });
</script>

```
![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p01_01.png)
可以看到我们操作的view控件数据，直接影响到了我们的vm.data，并且又进一步影响到了页面，这就是Vue的双向数据绑定。整个过程的原理如下图所示。
![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p01_02.png)
# 2.文本渲染指令
Vue使用了基于HTML的模板语法，允许开发者声明式地将DOM绑定至底层Vue实例的数据。所有Vue的模板都是合法的HTML，所以能被遵循规范的浏览器和HTML解析器解析。
在前面，我们一直使用的是字符串差值的形式渲染文本，但是除此方法之外，vue还提供了其他几种常见的文本渲染方式：
1. v-text：更新元素的innerText
2. v-html：更新元素的innerHTML
```html
<div id="app">
    <div v-html="msg"></div>
    <div v-text="msg"></div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            msg:'<p>Hello World!</p>'
        }
    });
</script>
```

在Vue中，我们可以使用 { {  } } 将数据插入到相应的模板中，这种方法是一种文本插值。 使用这种方法，如果网络慢或者JavaScript出错的话，会将 { {  } } 直接渲染到页面中。 值得庆幸的是，Vue还提供了v-text和v-html来渲染文本或元素。这样就避免了将 { {  } } 直接渲染到页面中。

# 3.属性绑定指令

如果想让html标签中的属性，也能应用Vue中的数据，那么就可以使用vue中常用的属性绑定指令：v-bind

```html
<div id="app">
    <div v-bind:title="msg">DOM元素属性绑定</div>
    <!-- v-bind的简写形式 -->
    <div :title="msg">DOM元素属性绑定</div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            msg:'Hello World!'
        }
    });
</script>
```

上面展示的是v-bind的最基本的使用，第一种是完整语法，第二种是缩写方式。
除了将元素的title属性和vue实例的相关字段进行绑定外，还能将其他的属性字段进行绑定，最常见的是对于样式的绑定，即class和style属性。

## 3.1.绑定样式

使用v-bind指令绑定class属性，就可以动态绑定元素样式了。
![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p02_01.png)

```html
<head>
    <style>
        .one{
            color: red;
        }
        .two{
            color: blue;
        }
    </style>
</head>
<body>
    <div id="app">
        <div :class="className">DOM元素的样式绑定</div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        let vm = new Vue({
            el: '#app',
            data: {
                className:'one'
            }
        });
    </script>
</body>
```

## 2.2.使用对象语法绑定样式

我们可以给v-bind:class 一个对象，也可以直接绑定数据里的一个对象，以动态地切换class。
![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p02_02.png)

```html
<head>
    <style>
        .one{
            color: red;
        }
        .two{
            font-size: 48px;
        }
    </style>
</head>
<body>
    <div id="app">
        <div :class="{one:oneActive,two:twoActive}">DOM元素的样式绑定</div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        let vm = new Vue({
            el: '#app',
            data: {
                oneActive:true,
                twoActive:false
            }
        });
    </script>
</body>
```

## 2.3.使用三目运算绑定样式

可以使用三目运算符，来动态绑定样式。

```html
<head>
    <style>
        .one{
            color: red;
        }
    </style>
</head>
<body>
    <div id="app">
        <div :class="userId==1?className:''">DOM元素的样式绑定</div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        let vm = new Vue({
            el: '#app',
            data: {
                userId:1,
                className:'one'
            }
        });
    </script>
</body>
```

## 2.4.直接绑定内联样式

也可以直接绑定内联样式。

```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<div id="app">
    <div style="color:red;font-size: 50px;">DOM元素的样式绑定</div>
    <div :style="{color:colorValue,fontSize:fontSizeValue}">DOM元素的样式绑定</div>
    <div :style="stylelist">DOM元素的样式绑定</div>
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            colorValue: 'green',
            fontSizeValue: '60px',
            stylelist: 'color:red'
        }
    });
</script>
```

> 注意：绑定style属性后，样式的书写要遵循javaScript规范。 也就是将 xxx-xxx 改写成驼峰命名方式 xxxXxxx

# 4.事件处理指令

我们可以用 v-on 指令绑定一个事件监听器，通过它调用我们 Vue 实例中定义的方法。
v-on指令可以简写为：@

```html
<div id="app">
    <button v-on:click="pointme">点击我</button>
    <!-- v-on指令的简写 -->
    <button @click="pointyou">点击你</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
        },
        methods:{
            pointme(){
                alert('点击了我');
            },
            pointyou(){
                alert('点击了你');
            }
        }
    });
</script>
```

一个案例：对一个数进行加减运算

```html
<div id="app">
    <button v-on:click="add(1)">加</button>
    <button @click="subtract(1)">减</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            num:1
        },
        methods:{
            add(value){
                this.num += value;
            },
            subtract(value){
                this.num -= value;
            }
        }
    });
</script>
```

# 5.条件渲染指令

条件渲染指令，可以根据条件判断，来设置元素的显示与隐藏。

## 5.1.v-if指令

当v-if的值为false时，网页中将不会对此元素进行渲染
![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p02_03.png)

```html
<div id="app">
    <div v-if="isShow">DOM元素的样式绑定</div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            isShow:false
        }
    });
</script>
```

## 5.2.v-else指令和v-else-if指令

我们可以使用 v-else 指令来表示 v-if 的“else 块”，v-else 元素必须紧跟在 v-if 或者 v-else-if 元素的后面——否则它将不会被识别。而v-else-if则是充当 v-if 的“else-if 块”，可以链式地使用多次。

```html
<div id="app">
    <div v-if="type==1">A</div>
    <div v-else-if="type==2">B</div>
    <div v-else-if="type==3">C</div>
    <div v-else>D</div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            type:2
        }
    });
</script>
```

## 5.3.v-if指令和v-show指令

v-show也是根据条件展示元素的指令。带有 v-show 的元素始终会被渲染并保留在 DOM 中。 v-show 是简单地切换元素的 CSS 属性 display 。

```html
<div id="app">
    <div v-if="isShow">这里是v-if</div>
    <div v-show="isShow">这里是v-show</div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            isShow:false
        }
    });
</script>
```

![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p02_04.png)
通过上面的例子，我们不难发现两者的不同：

1. v-if是真正的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

2. v-if是惰性的，只有当条件为true时才会渲染，如果条件为false则什么都不做

3. v-if有很高的切换开销，适用于条件不太容易改变的时候

4. v-show不管条件是true还是false都会进行渲染。并且只是简单地基于 CSS 进行切换

5. v-show有很高的初始渲染开销，适用于非常频繁地切换
   
   ## 5.4 控件的复用
   
   ```html
   <div id="app">
    <span v-if="userUser">
        <label for="username"> 用户名</label>
        <input type="text" id="username" placeholder="请输入用户名"  key="username"/>
    </span>
    <span v-else>
        <label for="email"> 邮箱</label>
        <input type="text" id="email" placeholder="请输入邮箱" key="email"/>
    </span>
   </div>
   <script>
    let vm = new Vue({
        el: '#app',
        data: {
            userUser:true
        },
    });
   </script>
   ```
   
   # 6.循环遍历指令
   
   vue.js 的循环渲染是依赖于 v-for 指令，它能够根据 vue 的实例里面的信息，循环遍历所需数据，然后渲染出相应的内容。它可以遍历数组类型以及对象类型的数据，js 里面的数组本身实质上也是对象，这里遍历数组和对象的时候，方式相似但又稍有不同。
   
   ## 6.1.遍历对象属性
   
   value 是遍历得到的属性值，key 是遍历得到的属性名，index 是遍历次序，这里的 key、index 都是可选参数，如果不需要，这个指令其实可以写成 v-for="value in user"；
   ![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p02_05.png)
   
   ```html
   <div id="app">
    <p v-for="(value,key,index) in user">：：</p>
    <hr>
    <p v-for="value in user"></p>
   </div>
   <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
   <script>
    let vm = new Vue({
        el: '#app',
        data: {
            user:{
                userId:1,
                userName:'张三',
                userSex:'男'
            }
        }
    });
   </script>
   ```
   
   ## 6.2.遍历数组元素
   
   value 是遍历得到的元素，index 是数组下标，这里的index 也是可选参数，如果不需要，这个指令其实可以写成 v-for="value in userArr"；
   ![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p02_06.png)
   
   ```html
   <div id="app">
    <p v-for="(item,index) in userArr">
        ,, 
        <button @click="operate(index)">操作</button>
    </p>
   </div>
   <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
   <script>
    let vm = new Vue({
        el: '#app',
        data: {
            userArr: [{
                userId: 1,
                userName: '张三',
                userSex: '男'
            }, {
                userId: 2,
                userName: '李四',
                userSex: '女'
            }, {
                userId: 3,
                userName: '王五',
                userSex: '男'
            }]
        },
        methods:{
            operate(index){
                console.log(this.userArr[index]);
            }
        }
    });
   </script>
   ```
   
   # 7.综合案例
   
   ![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p02_07.png)
   
   ```html
   <!DOCTYPE html>
   <html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
        <style>
            #app{
                        width: 500px;
                    }
                    table{
                        width: 100%;
                        border-collapse: collapse;
                    }
                    table tr th,table tr td{
                        height: 35px;
                        border-bottom: solid 1px #999;
                        text-align: center;
                    }
                    .clear-btn{
                        text-align: right;
                    }
                </style>
    </head>
    <body>
        <div id="app">
            <table>
                <tr>
                    <th>编号</th>
                    <th>姓名</th>
                    <th>年龄</th>
                    <th>操作</th>
                </tr>
                <tr v-for="(obj,index) in arr">
                    <td>{{obj.userId}}</td>
                    <td>{{obj.userName}}</td>
                    <td>{{obj.userAge}}</td>
   
                    <td><button  @click="del(index)" >删除</button></td>
                </tr>
                <tr >
                    <td><button  @click="clear()" >清空</button></td>
                </tr>
            </table>
            <h3>添加</h3>
            姓名：<input type="text" v-model="userName"><br>
            年龄：<input type="text"  v-model="userAge"><br>
            <button @click="add">添加</button>
   
        </div>
        <script>
   
            let vm = new Vue({
                el: '#app',
                data: {
                    arr: [{
                        userId: 1,
                        userName: '张三',
                        userAge: 20
                    }, {
                        userId: 2,
                        userName: '李四',
                        userAge: 21
                    }, {
                        userId: 3,
                        userName: '王五',
                        userAge: 22
                    }],
                    userName:'',
                    userAge:null,
                },
                methods:{
                    del(index){
                        //this Vue实例
                        this.arr.splice(index,1);
                    },
                    clear(){
                        this.arr = [];
                    },
                    add(){
                        //获取控件
                        let userName = this.userName;
                        let userAge = this.userAge;
                        let userId = (this.arr.length +1);
                        this.arr.push({
                            userId,
                            userName,
                            userAge
                        })
                    }
                }
            });
        </script>
    </body>
   </html>
   ```
   
   # 8.Vue方法、计算属性及监听器
   
   在vue中处理复杂的逻辑的时候，我们经常使用计算属性、方法及监听器。

6. methods：方法：它们是挂载在Vue对象上的函数，通常用于做事件处理函数，或自己封装的自定义函数。

7. computed：计算属性：在Vue中，我们可以定义一个计算属性，这个计算属性的值，可以依赖于某个data中的数据。或者说：计算属性是对数据的再加工处理。

8. watch：监听器：如果我们想要在数据发生改变时做一些业务处理，或者响应某个特定的变化，我们就可以通过监听器，监听数据的变化，从而做出相应的反应。
   
   ## 8.1.computed 计算属性
   
   计算属性是根据依赖关系进行缓存的计算，并且只在需要的时候进行更新。
   
   ```html
   <div id="app">
    <p>原数据：</p>
    <p>新数据：</p>
   </div>
   <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
   <script>
    let vm = new Vue({
        el: '#app',
        data: {
            msg:'hello world!'
        },
        computed:{
            reversedMsg(){
                return this.msg.split('').reverse().join('');
            }
        }
    });
   </script>
   ```
   
   一个案例：根据商品数量修改总价
   
   ```html
   <div id="app">
    <table width="100%" style="text-align: center;">
        <tr>
            <th>商品编号</th>
            <th>商品名称</th>
            <th>商品单价</th>
            <th>商品数量</th>
            <th>合计</th>
        </tr>
        <tr>
            <td>1</td>
            <td>小米10</td>
            <td></td>
            <td>
                <button @click="subtract">-</button>
                <button @click="add">+</button>
            </td>
            <td></td>
        </tr>
    </table>
   </div>
   <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
   <script>
    let vm = new Vue({
        el: '#app',
        data: {
            price:2999,
            quantity:1
        },
        computed:{
            totalPrice(){
                return this.price*this.quantity;
            }
        },
        methods:{
            add(){
                this.quantity++;
            },
            subtract(){
                this.quantity--;
            }
        }
    });
   </script>
   ```
   
   ## 8.2.methods 方法
   
   在使用vue的时候，可能会用到很多的方法，它们可以将功能连接到事件的指令，甚至只是创建一个小的逻辑就像其他函数一样被重用。接下来我们用方法实现上面的字符串反转。
   
   ```html
   <div id="app" >
    <p>原数据： { { msg } } </p>
    <p>新数据： { { reversedMsg() } } </p>
   </div>
   <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
   <script>
    let vm = new Vue({
        el: '#app',
        data: {
            msg:'hello world!'
        },
        methods:{
            reversedMsg(){
                return this.msg.split('').reverse().join('');
            }
        }
    });
   </script>
   ```
   
   虽然使用computed和methods方法来实现反转，两种方法得到的结果是相同的，但本质是不一样的。
   计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变的时候才会重新求值，这就意味着只要message还没有发生改变，多次访问reversedMessage计算属性立即返回的是之前计算的结果，而不会再次执行计算函数。
   而对于methods方法，只要发生重新渲染，methods调用总会执行该函数。
   如果某个computed需要的遍历一个极大的数组和做大量的计算，可以减小性能开销，如果不希望有缓存，则用methods。
   
   ```html
   <!DOCTYPE html>
   <html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
   
    </head>
    <body>
        <div id="app">
            <button @click="clickBtn"> 点我</button>
            <button @click="clickBtn(1)"> 点我2</button>
   
            年龄1 <input type="text" v-model="age1" />
            年龄2 <input type="text" v-model="age2" />
            <button @click="cal()"> 计算</button>
            {{ageSum}}
   
        </div>
        <script>
   
            let vm = new Vue({
                el: '#app',
                data: {
                    age1:null,
                    age2:null
   
                },
                methods:{
                    clickBtn(val){
                        alert('clickBtn\t'+val);
   
                        this.mt();
                    },
                    mt(){
                        alert("另外一个方法")
                    },
                    cal(){
                        this.ageSum =  parseInt(this.age1) +parseInt(this.age2) ;
                    }
                },
                computed:{
                    ageSum(){
                        return parseInt(this.age1) +parseInt(this.age2) ;
                    }
                }
            });
        </script>
    </body>
   </html>
   ```
   
   ## s8.3.watch 监听器
   
   watch能够监听数据的改变。监听之后会调用一个回调函数。 此回调函数的参数有两个：

9. 更新后的值（新值）

10. 更新前的值（旧值）
    
    ### 8.3.1.监听基本数据类型
    
    下面使用watch来监听商品数量的变化。如果商品数量小于1，就重置成上一个值。
    
    ```html
    <div id="app">
     <table width="100%" style="text-align: center;">
         <tr>
             <th>商品编号</th>
             <th>商品名称</th>
             <th>商品单价</th>
             <th>商品数量</th>
             <th>合计</th>
         </tr>
         <tr>
             <td>1</td>
             <td>小米10</td>
             <td></td>
             <td>
                 <button @click="subtract">-</button>
                 <button @click="add">+</button>
             </td>
             <td></td>
         </tr>
     </table>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
     let vm = new Vue({
         el: '#app',
         data: {
             price: 2999,
             quantity: 1
         },
         computed: {
             totalPrice() {
                 return this.price * this.quantity;
             }
         },
         methods: {
             add() {
                 this.quantity++;
             },
             subtract() {
                 this.quantity--;
             }
         },
         watch:{
             quantity(newVal,oldVal){
                 console.log(newVal,oldVal);
                 this.quantity = newVal<=0?oldVal:newVal
             }
         }
     });
    </script>
    ```
    
    ### 8.3.2.深度监听
    
    在上面的例子中，监听的简单的数据类型，数据改变很容易观察，但是当需要监听的数据变为对象类型的时候，上面的监听方法就失效了，因为上面的简单数据类型属于浅度监听，对应的对象类型就需要用到深度监听，只需要在上面的基础上加上deep: true就可以了。
    
    ```html
    <!DOCTYPE html>
    <html>
     <head>
         <meta charset="utf-8" />
         <title></title>
         <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
         <script src="./js/vue.js"></script>
     </head>
     <body>
         <div id="app">
             <table width="100%" style="text-align: center;">
                 <tr>
                     <th>商品编号</th>
                     <th>商品名称</th>
                     <th>商品单价</th>
                     <th>商品数量</th>
                     <th>合计</th>
                     <th>操作</th>
                 </tr>
                 <tr>
                     <td>1</td>
                     <td>小米10</td>
                     <td>{{goods.price}}</td>
                     <td>{{goods.quantity}}</td>
                     <td>{{totalPrice}}</td>
                     <td>
                         <button @click="subtract">-</button>
                         <button @click="add">+</button>
                     </td>
                 </tr>
             </table>
         </div>
         <script>
             let vm = new Vue({
                 el: '#app',
                 data: {
                     price: 2999,
                     quantity: 1,
                     goods: {
                         price: 2999,
                         quantity: 1
                     }
                 },
                 computed: {
                     totalPrice() {
                         return this.price * this.quantity;
                     },
                     deepQuantity(){
                         return this.goods.quantity;
                     }
    ```
    
                },
                methods: {
                    add() {
                        this.goods.quantity++;
                    },
                    subtract() {
                        this.goods.quantity--;
                    }
                },
                watch: {
                    quantity(newVal, oldVal) {
                        console.log(newVal, oldVal);
                        this.quantity = newVal < 0 ? 0 : newVal
                    },
                    goods: {
                        deep: true,
                        handler:(newVal, oldVal)=> {
                            //console.log(newVal, oldVal);
                        }
                    },
                    deepQuantity(newVal, oldVal){
                        console.log("侦听器=",newVal, oldVal);
                    }
                }
            });
        </script>
    
    </body>

</html>
```
# 9.Vue操作数组时的注意事项
由于 JavaScript 语言本身的限制，当我们对数组进行某些操作时，Vue 可能不会检测到数组元素的变化，那么进而视图也不会响应。比如：
```html
<div id="app">
    <p v-for="(item,index) in userArr">
        ,,
    </p>
    <button @click="clear">清空</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            userArr: [{
                userId: 1,
                userName: '张三',
                userSex: '男'
            }, {
                userId: 2,
                userName: '李四',
                userSex: '女'
            }, {
                userId: 3,
                userName: '王五',
                userSex: '男'
            }]
        },
        methods: {
            clear() {
                this.userArr.length = 0;
                console.log(this.userArr.length);    //0
            }
        }
    });
</script>
```
上面实例中，将数组的长度设置为0后，数组确实被清空了。但是Vue却不能检测到数组的变化，所以页面视图也不会响应。
为了解决这个问题，Vue给我们提供了如下解决方案： Vue 提供一组观察数组的变异方法（就是这些方法会改变原始数组，所以才会被Vue检测到），使用它们就可以触发视图更新。 这些方法有7个：push()、pop()、shift()、unshift()、splice()、sort()、reverse()
```html
<div id="app">
    <p v-for="(item,index) in userArr">
        ,,
    </p>
    <button @click="clear">清空</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            userArr: [{
                userId: 1,
                userName: '张三',
                userSex: '男'
            }, {
                userId: 2,
                userName: '李四',
                userSex: '女'
            }, {
                userId: 3,
                userName: '王五',
                userSex: '男'
            }]
        },
        methods: {
            clear() {
                //使用Vue中提供的变异方法
                this.userArr.splice(0,this.userArr.length);
            }
        }
    });
</script>
```
# 10.Vue的表单绑定
v-bind实现了数据的单向绑定，将vue实例中的数据同元素属性值进行绑定，接下来看一下vue中的数据双向绑定v-model。
## 10.1.v-mode实现表单绑定
```html
<div id="app">
    <h3>注册</h3>
    <form>
        用户名：<input type="text" v-model="myForm.username"/><br>
        密码：<input type="password" v-model="myForm.password" /><br>
        确认密码：<input type="password" v-model="myForm.beginpassword" /><br>
        性别：<input type="radio" v-model="myForm.sex" value="0" />男
             <input type="radio" v-model="myForm.sex" value="1" />女<br>
        爱好：<input type="checkbox" v-model="myForm.like" value="0" />读书
             <input type="checkbox" v-model="myForm.like" value="1" />体育
             <input type="checkbox" v-model="myForm.like" value="2" />旅游<br>
        国籍：<select v-model="myForm.nationality">
                <option value="0">中国</option>
                <option value="1">美国</option>
                <option value="2">英国</option>
             </select><br>
        个人简介：<textarea v-model="myForm.brief" rows="5" cols="30"></textarea><br>
             <input type="button" value="提交" @click="handler" />
    </form>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            myForm: {
                username: '',
                password: '',
                beginpassword: '',
                sex: 0,
                like: [0, 1, 2],
                nationality: 0,
                brief: '',
                level: 0
            }
        },
        methods: {
            handler: function() {
                console.log(this.myForm.username);
                console.log(this.myForm.password);
                console.log(this.myForm.beginpassword);
                console.log(this.myForm.sex);
                console.log(this.myForm.like);
                console.log(this.myForm.nationality);
                console.log(this.myForm.brief);
            }
        }
    });
</script>
```
## 10.2.v-model修饰符
v-model中还可以使用一些修饰符来实现某些功能：
1. v-model.lazy  只有在input输入框发生一个blur时才触发，也就是延迟同步到失去焦点时。
2. v-model.trim  将用户输入的前后的空格去掉。
3. v-model.number  将用户输入的字符串转换成number。
```html
<div id="app">
    <!-- 输入数据会延迟到失去焦点时才绑定 -->
    <input type="text" v-model.lazy="lazyStr" /><br>
    <!-- 输入数据会自动转换为number，所以可以直接进行运算 -->
    NaN<input type="text" v-model.number="numberStr" /><br>
    <!-- 输入数据会自动去除前后空格 -->
    <input type="text" v-model.trim="trimStr" /><br>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            lazyStr: '',
            numberStr:'',
            trimStr:''
        }
    });
</script>
```
# 11.过滤器
过滤器是对即将显示的数据做进一步的筛选处理，然后进行显示，值得注意的是过滤器并没有改变原来的数据，只是在原数据的基础上产生新的数据。
过滤器分全局过滤器和本地过滤器（局部过滤器）。
## 11.1.全局过滤器
下面定义一个全局过滤器，用于在数据前加上大写的VUE。需要注意的是，全局过滤器定义必须始终位于Vue实例之上，否则会报错。
```html
<div id="app">
    { { message | toAdd  } }
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    Vue.filter("toAdd", function(value) {
        return 'VUE' + value
    })
    var demo = new Vue({
        el: '#app',
        data: {
            message: '过滤器',
        }
    })
</script>
```
> 注意：
>
> 1. Vue.filter() 后有两个参数：过滤器名，过滤器处理函数。
> 2. 过滤器处理函数也有一个参数：要过滤的数据。
## 11.2.本地过滤器
本地过滤器存储在vue组件中，作为filters属性中的函数，我们可以注册多个过滤器存储在其中。
```html
<div id="app">
    <p> { { message | Reverse } } </p>
    <p> { { message | Length } } </p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    var demo = new Vue({
        el: '#app',
        data: {
            message: '过滤器',
        },
        filters: {
            Reverse: function(value) {
                if (!value){
                    return '';
                } 
                return value.toString().split('').reverse().join('');
            },
            Length: function(value) {
                return value.length;
            },
        },
    })
</script>
```
## 11.3.过滤器传参
过滤器也是可以传递参数的。
```html
<div id="app">
    { { msg1 | toDou (1,2) } }
    { { msg2 | toDou (10,20) } }
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    Vue.filter('toDou', function(value, param1, param2) {
        if (param2 < 10) {
            return value + param1;
        } else {
            return value + param2;
        }
    });
    new Vue({
        el: '#app',
        data: {
            msg1: 9,
            msg2: 12,
        }
    });
</script>
```
> 注意：过滤器处理函数的第一个参数，仍然是要过滤的数据。从第二个参数开始，才是过滤器本身要传递的参数。
## 11.4.串联过滤器
多个过滤器可以串联使用
```vue
<div id="app">
    { { money | prefix | suffix } }
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    Vue.filter("prefix", function(value) {
        return '￥' + value
    })
    Vue.filter("suffix", function(value) {
        return  value + '元'
    })
    var demo = new Vue({
        el: '#app',
        data: {
            money:10
        }
    })
</script>
```
## 11.5 完整代码
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
    </head>
    <body>
        <div id="app">
            <input type="text" v-model="msg" />

            <p>{{msg|filtera}}</p>
    
            <p>{{msg|reverse}}</p>
    
            <p>输入内容的长度: {{msg|reverse|len}}</p>
    
    
            <hr/>
            <input type="text" v-model="num" />
            <p>2倍 {{num|operation('1') }}</p>
            <p>平方 {{num|operation('2')}}</p>
    
        </div>
        <script>
    
            Vue.filter('filtera',function(value){
                var arr = value.split('');
                var newArr = [];
                arr.forEach(function(item){
                    if(item != 'a'){
                        newArr.push(item)
                    }else{
                        newArr.push('*')
                    }
                });
    
                return newArr.join('');
            })
    
    
            let vm = new Vue({
                el: '#app',
                data: {
                    msg:'',
                    num:null
                },
                filters:{
                    reverse:function(value){
                         if (!value){
                            return '';
                        } 
                        return value.toString().split('').reverse().join('');
                    },
                    len:function(value){
                        return value.length;
                    },
                    operation(num, type){
                        if(type == '1')    {
                            return num * 2;
                        }else{
                            return num * num;
                        }
                    }
                }
    
            });
    
    
        </script>
    </body>

</html>
```
# 12 初识Vue组件
在vue中，组件是最重要的组合部分，官方中定义组件为可复用的vue实例，分为全局组件和局部组件，接下来通过实例来分别演示两种不同的组件。
## 12.1.全局组件
全局组件可以在任意Vue示例下使用。 
```html
<div id="app">
    <mycomponent></mycomponent>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    Vue.component('mycomponent', {
        template: '<h3>我是全局组件</h3>'
    });
    var vm = new Vue({
        el: '#app',
    });
</script>
```
通过上面的例子，我们可以总结出全局组件的使用步骤：
1. 使用vue.component()注册组件，需要提供2个参数：组件的标签名和组件构造器。
2. vue.component()内部会调用组件构造器，创建一个组件实例
3. 将组建挂载到某个vue实例下。
> 注意：一个组件的template部分，必须要包裹在一个根容器中。
因为组件是可复用的vue实例，所以它们也能有data、computed、watch、methods以及生命周期钩子等。
```html
<div id="app">
    <mycomponent></mycomponent>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    Vue.component('mycomponent', {
        template: `<div>
                        <h3>我是全局组件</h3>
                         <button @click="add">加</button>
                   </div>`,
        data(){
            return {
                num:1
            }
        },
        methods:{
            add(){
                this.num++;
            }
        }
    });
    var vm = new Vue({
        el: '#app',
    });
</script>
```
> 上面代码中有两个注意事项：
>
> 1. 组件模板的内容，可以写在一对反引号中（``），这样就可以不使用字符串拼接的形式了。
> 2. 一个组件的data选项必须是一个函数，该函数返回一个对象。
## 12.2.局部组件
如果不需要全局注册，或者是让组件使用在其它组件内，可以用选项对象的components属性实现局部注册。 因此我们可以将上面的全局组件改为局部组件。
```html
<div id="app">
    <mycomponent></mycomponent>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let obj = {
        template: `<div>
                       <h3>我是局部组件</h3>
                        <button @click="add">加</button>
                   </div>`,
        data(){
            return {
                num:1
            }
        },
        methods:{
            add(){
                this.num++;
            }
        }
    };
    var vm = new Vue({
        el: '#app',
        components:{    //声明局部组件
            mycomponent:obj
        }
    });
</script>
```
## 12.3.组件模板
如果组件中的template内容过多，那么可以使用组件模板来声明template中的内容。
```html
<div id="app">
    <mycomponent></mycomponent>
</div>
<!-- 组件模板 -->
<template id="mytemplate">
    <div>
        <h3>我是局部组件</h3>
         <button @click="add">加</button>
    </div>
</template>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let obj = {
        template: '#mytemplate',      //使用选择器应用组件模板
        data() {
            return {
                num: 1
            }
        },
        methods: {
            add() {
                this.num++;
            }
        }
    };
    var vm = new Vue({
        el: '#app',
        components: { //声明局部组件
            mycomponent: obj //组件名:组件实例
        }
    });
</script>
```
## 12.4 完整代码
- 全局组件
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
    </head>
    <body>
        <div id="app">
            <mycomponent></mycomponent>
            <mycomponent></mycomponent>
            <mycomponent></mycomponent>
            <mycomponent></mycomponent>
        </div>
        <script>
            Vue.component('mycomponent', {
                template: `<div>  <h3>我是全局组件</h3>  </div>`,
                data() {
                    return {}
                },
                methods: {
                    add() {
                        this.num++;
                    }
                }
            });
            var vm = new Vue({
                el: '#app',

            });
        </script>
    </body>

</html>
```
- 局部组件
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
    </head>
    <body>
        <div id="app">
            <localcomponent></localcomponent>
            <localcomponent></localcomponent>
            <localcomponent></localcomponent>
        </div>
        <template id="btncounter">
            <div>
                局部组件 {{num}}
                <button @click='counter'>数量++</button>
            </div>
        </template>
        <script>

            var vm = new Vue({
                el: '#app',
                components:{
                    localcomponent:{
                        // template:`
                        // <div>
                        //     局部组件 {{num}}
                        //     <button @click='counter'>数量++</button>
    
                        // </div>`,
                        template:'#btncounter',
                        data(){
                            return {
                                num:1
                            }
                        },
                        methods:{
                            counter(){
                                this.num++;
                            }
                        }
                    }
                }
            });
        </script>
    </body>

</html>
```
# 13.父子组件
当我们继续在组件中写组件，形成组件嵌套的时候，就是我们所说的父子组件了。
```html
<div id="app">
    <mycomponent></mycomponent>
</div>
<template id="mytemplate">
    <div>
        <h3>我是父组件</h3>
        <!-- 在父组件中使用子组件 -->
        <subcomponents></subcomponents>
    </div>
</template>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let obj = {
        template: '#mytemplate',
        data() {
            return {}
        },
        components:{          //声明子组件
            subcomponents:{
                template:`<div>我是子组件</div>`
            }
        }
    };
    var vm = new Vue({
        el: '#app',
        components: { //声明局部组件
            mycomponent: obj //组件名:组件实例
        }
    });
</script>
```
# 14.组件之间的通信
组件与组件之间是可以互相通信的。包括父子组件之间、兄弟组件之间等等，都可以互相通信。 下面只讨论父子组件之间通信问题。
## 14.1.子组件获取父组件数据
在vue中，组件实例的作用域是孤立的，默认情况下，父子组件的数据是不能共享的，也就是说，子组件是不能直接访问父组件的数据的。为此，vue给我们提供了一个数据传递的选项prop，用来将父组件的数据传递给子组件。具体使用如下：
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
    </head>
    <body>
        <div id="app">
            <sub-component msg="张翼德"></sub-component>
            <sub-component msg="曹操"></sub-component>
        </div>
        <template id="btncounter">
            <div>
                hello {{msg}}
            </div>
        </template>
        <script>
            //子组件
            let subComponent = {
                template: '#btncounter',
                props: ['msg'],
                data() {
                    return {}
                },
                methods: {
                    counter() {
                        this.num++;
                    }
                }
            }
            var vm = new Vue({
                el: '#app',
                data: {
                    msg: 'parent'
                },
                components: {
                    subComponent
                }
            });
        </script>
    </body>
</html>
```
上面实例中，子组件获取父组件传递的数据的步骤为：
1. 在子组件标签中，声明 msg 属性，属性值即为父组件向子组件传递的值。
2. 在子组件中，使用props选项，声明接收父组件向子组件传递值的载体，即 ‘msg’ 。
3. 子组件中就可以使用 msg 获取父组件向子组件传递的值了。
也可以使用 v-bind 绑定子组件标签属性，这样就可以将父组件data数据传递个子组件了。
## 14.2.父组件获取子组件数据
和上面不一样的是，父组件想要获取子组件的数据时，需要子组件通过emit主动将自己的数据发送给父组件。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
    </head>
    <body>
        <div id="app">

            <sub-component msg="张翼德"   @getnum="getNum"></sub-component>
            <sub-component msg="曹操"    @getnum="getNum2"></sub-component>
            第一个的数据{{firstNum}} <br/>
            第二个的数据{{secondNum}}
    
    
    
    
    
    
        </div>
        <template id="btncounter">
            <div>
                hello {{msg}}
    
                <button @click="counter">{{num}}</button>
    
    
            </div>
        </template>
        <script>
            //子组件
            let subComponent = {
                template: '#btncounter',
                props: ['msg'],
                data() {
                    return {
                        num:1
                    }
                },
                methods: {
                    counter() {
                        this.num++;
                        this.$emit('getnum',this.num)
                    }
                }
            }
            var vm = new Vue({
                el: '#app',
                data: {
                    firstNum:0,
                    secondNum:0,
                    msg: 'parent'
                },
                components: {
                    subComponent
                },
                methods:{
                    getNum(num){
                        this.firstNum = num;
                        // alert('事件被调用'+num)
                    },
                    getNum2(num){
                        this.secondNum = num;
                        // alert('事件被调用'+num)
                    }
                }
            });
        </script>
    </body>

</html>
```
首先，我们需要在子组件中触发一个主动发送数据的事件，上面的例子中是一个点击事件**send**；其次，在点击事件中使用**emit**方法，这个**emit**接收两个参数：传递数据的事件和需要传递的数据，这个传递数据的事件也是自定义的；然后在父组件中引用子组件，并在引用的子组件中使用**on**监听上一步传递数据的事件，上面的例子中是**childmsg**；最后在父组件中使用这个事件，这个事件带有一个参数，就是从子组件发送过来的数据。
# 15.Vue路由
## 15.1.Vue路由基础
Vue属于单页应用（SPA），即整个应用程序中只有一个html页面。
在单页应用中（SPA），由于只是更改DOM来模拟多页面，所以页面浏览历史记录的功能就丧失了。此时，就需要前端路由来实现浏览历史记录的功能。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
        <script src="https://cdn.jsdelivr.net/npm/vue-router/dist/vue-router.js"></script>
    </head>
    <body>
        <div id="app">
            <router-link to="/home">home</router-link>
            <router-link to="/news">news</router-link>
            <router-view></router-view>
        </div>
        <script>
            const Home = {
                template: '<div>首页</div>'
            }
            const News = {
                template: '<div>新闻</div>'
            }
            var router = new VueRouter({
                 // mode: 'hash',
                mode: 'history',
                routes: [{
                        path: '/home',
                        component: Home
                    }, {
                        path: '/news',
                        component: News
                    }
                ]
            });
            var vm = new Vue({
                el: '#app',
                data: {
                },
                // 将路由添加到Vue中
                router,
            });
        </script>
    </body>
</html>
```
> 注意：
>
> 上面代码中，router-link标签默认会被渲染成一个a标签
>
> ![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p05_01.png)
路由重定向：上面代码中，我们应该设置打开浏览器就默认调整到 “首页”，所以需要把根路由/重定向到/home。 修改路由配置：
```javascript
// 2. 定义路由规则对象（每个路由应该映射一个组件）
const routes = [
    {
        path: '/',            //根路由
        redirect: '/home'     //把根路由重定向到home
    },{
        path: '/home',
        component: Home
    },{
        path: '/news',
        component: News
    }
]
```
## 15.2.嵌套路由
实际应用界面，通常由多层嵌套的组件组合而成。 比如，我们 “新闻”组件中，还嵌套着 “国内”和 “国外”组件，那么URL对应就是/news/guonei和/news/guowai。
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
        <script src="https://cdn.jsdelivr.net/npm/vue-router/dist/vue-router.js"></script>
    </head>
    <body>
        <div id="app">
            <p>
                <!-- 使用 router-link 组件来导航. to属性指定导航地址-->
                <router-link to="/home">home</router-link>
                <router-link to="/news">news</router-link>
            </p>
            <!-- 路由出口（路由匹配到的组件将渲染在这里） -->
            <router-view></router-view>
        </div>
        <script>
            // 1. 定义（路由）组件。
            const Home = {
                template: `<div>首页</div>`
            }
            const News = {
                template: `<div>
                    <p>
                        <router-link to="/news/guonei">国内新闻</router-link>
                        <router-link to="/news/guoji">国际新闻</router-link>
                    </p>
                    <router-view></router-view>
                </div>`
            }
            const GuoNeiNews = {
                template: `<div>上海最美公厕：室内长大树，墙壁会发光</div>`
            }
            const GuoJiNews = {
                template: `<div>在英国旅游坐巴士，“return ticket”真..</div>`
            }
            // 2. 定义路由规则对象（每个路由应该映射一个组件）
            const routes = [{
                    path: '/home',
                    component: Home,
                }, {
                    path: '/news',
                    component: News,
                    children: [{
                            path: '/news/guonei',
                            component: GuoNeiNews
                        }, {
                            path: '/news/guoji',
                            component: GuoJiNews
                        }
                    ]
                },
            ]
            // 3. 创建 router 实例，然后传 `routes` 配置
            const router = new VueRouter({
                //如果路由规则对象名也为routes，那么就可以简写为 routes
                routes: routes
            })
            var vm = new Vue({
                el: '#app',
                data: {},
                // 将路由添加到Vue中
                router,
            });
        </script>
    </body>
</html>
```
## 15.3 路由重定向
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
        <script src="https://cdn.jsdelivr.net/npm/vue-router/dist/vue-router.js"></script>
    </head>
    <body>
        <div id="app">
            <p>
                <!-- 使用 router-link 组件来导航. to属性指定导航地址-->
                <router-link to="/home">home</router-link>
                <router-link to="/news">news</router-link>
            </p>
            <!-- 路由出口（路由匹配到的组件将渲染在这里） -->
            <router-view></router-view>
        </div>
        <script>
            // 1. 定义（路由）组件。
            const Home = {
                template: `<div>首页</div>`
            }
            const News = {
                template: `<div>
                    <p>
                        <router-link to="/news/guonei">国内新闻</router-link>
                        <router-link to="/news/guoji">国际新闻</router-link>
                    </p>
                    <router-view></router-view>
                </div>`
            }
            const GuoNeiNews = {
                template: `<div>上海最美公厕：室内长大树，墙壁会发光</div>`
            }
            const GuoJiNews = {
                template: `<div>在英国旅游坐巴士，“return ticket”真..</div>`
            }
            // 2. 定义路由规则对象（每个路由应该映射一个组件）
            const routes = [
                /* {
                    path:'/',
                    redirect:'/home'
                }, */
                {
                    path: '/',
                    redirect: {
                        name: 'MyHome'
                    }
                },
                {
                    path: '/home',
                    name: 'MyHome',
                    component: Home,
                }, {
                    path: '/news',
                    component: News,
                    children: [{
                            path: '/news/guonei',
                            component: GuoNeiNews
                        }, {
                            path: '/news/guoji',
                            component: GuoJiNews
                        }
                    ]
                },
            ]
            // 3. 创建 router 实例，然后传 `routes` 配置
            const router = new VueRouter({
                //如果路由规则对象名也为routes，那么就可以简写为 routes
                routes: routes
            })
            var vm = new Vue({
                el: '#app',
                data: {},
                // 将路由添加到Vue中
                router,
            });
        </script>
    </body>
</html>
```
## 15.4.路由传参
路由传参有多种方式，这里我们学习两种：params与query。
### 15.4.1.params形式传参
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
        <script src="https://cdn.jsdelivr.net/npm/vue-router/dist/vue-router.js"></script>
    </head>
    <body>
        <div id="app">
            <p>
                <router-link :to="{name:'News',params:{msg:'国内'}}">国内</router-link>
            </p>
            <router-view></router-view>
        </div>
        <script>
            const News = {
                data(){
                    return {
                        msg:''
                    }
                },
                template: `<div> {{msg}}新闻</div>`,
                created(){
                    this.msg = this.$route.params.msg;
                    console.log(this.$route.params)
                }
            }
            // 2. 定义路由规则对象（每个路由应该映射一个组件）
            const routes = [
                {
                    path: '/news',
                    name: 'News',
                    component: News,
                }
            ]
            // 3. 创建 router 实例，然后传 `routes` 配置
            const router = new VueRouter({
                //如果路由规则对象名也为routes，那么就可以简写为 routes
                routes: routes
            })
            var vm = new Vue({
                el: '#app',
                data: {},
                // 将路由添加到Vue中
                router,
            });
        </script>
    </body>
</html>
```
> 注意：
>
> 1. 使用v-bind绑定to属性。
> 2. to属性的值是一个json对象，此对象有两个属性：name属性和params属性。
> 3. name属性就是要路由的对象。所以，在路由规则列表中，每一个路由规则都应用有一个name值。
> 4. params属性就是要传递的参数。也是一个json对象。
> 5. 组件接收参数时，使用 this.$route.params.参数名 的形式。
### 15.4.2.query形式传参
```html
<div id="app">
    <p>
        <router-link :to="{path:'/home',query:{msg:'hello world!'}}">home</router-link>
        <router-link :to="{path:'/news',query:{id:id,name:name}}">news</router-link>
    </p>
    <router-view></router-view>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue-router/dist/vue-router.js"></script>
<script type="text/javascript">
    const Home = {
        template: '<div>,首页</div>'
    }
    const News = {
        template: `<div>新闻;  
                        参数1：； 
                        参数2：
                    </div>`
    }
    const routes = [{
        path: '/home',
        component: Home
    }, {
        path: '/news',
        component: News
    }]
    const router = new VueRouter({
        routes
    })
    var vm = new Vue({
        el: '#app',
        data: {
            id:1,
            name:'zhangsan'
        },
        router
    })
</script>
```
> 注意：
>
> 1. to属性的值仍然是一个josn对象，但是两个属性变了，一个是path，一个是query。
> 2. path属性就是路由地址，对应路由规则中的path值。
> 3. query属性就是要传递的参数。也是一个json对象。
> 4. 组件接收参数时，使用 this.$route.query.参数名 的形式。
### 15.4.3.params方式与query方式的区别
query方式传值：
![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p05_02.png)
params方式传值：
![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p05_03.png)
> 总结：params方式与query方式的区别：
>
> 1. query方式：
>
>    类似于get方式，参数会在路由中显示，可以用做刷新后仍然存在的参数。
>
>    利用路由规则中的path跳转。
>
> 2. params方式：
>
>    类似于post方式，参数不会在路由中显示，页面刷新后参数将不存在。
>
>    利用路由规则中的name跳转。
### 15.4.4 完整代码
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <!-- <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> -->
        <script src="./js/vue.js"></script>
        <script src="https://cdn.jsdelivr.net/npm/vue-router/dist/vue-router.js"></script>
    </head>
    <body>
        <div id="app">
            <p>
                <router-link :to="{path:'/home',query:{msg:'query的形式参数'}}">首页</router-link>

                <router-link :to="{name:'News',params:{msg:'国内'}}">国内</router-link>
    
            </p>
            <router-view></router-view>
        </div>
        <script>
            const Home = {
                template: '<div>首页: {{this.$route.query.msg}}</div>'
            }
            const News = {
                data() {
                    return {
                        msg: ''
                    }
                },
                template: `<div> {{msg}}新闻</div>`,
                created() {
                    this.msg = this.$route.params.msg;
                    console.log(this.$route.params)
                }
            }
            // 2. 定义路由规则对象（每个路由应该映射一个组件）
            const routes = [
                {
                    path: '/home',
                    name: 'Home',
                    component: Home,
                },
                {
                path: '/news',
                name: 'News',
                component: News,
            }]
            // 3. 创建 router 实例，然后传 `routes` 配置
            const router = new VueRouter({
                //如果路由规则对象名也为routes，那么就可以简写为 routes
                routes: routes
            })
            var vm = new Vue({
                el: '#app',
                data: {},
                // 将路由添加到Vue中
                router,
            });
        </script>
    </body>

</html>
```
# 16.编程式路由
## 16.1.利用JS实现路由跳转
router-link标签可以实现页面超链接形式的路由跳转。但是实际开发中，在很多情况下，需要通过某些逻辑判断来确定如何进行路由跳转。也就是说：需要在js代码中进行路由跳转。此时可以使用编程式路由。
1. 使用this.$router.push方法可以实现路由跳转，方法的第一个参数可为string类型的路径，或者可以通过对象将相应参数传入。
2. 通过this.$router.go(n)方法可以实现路由的前进后退，n表示跳转的个数，正数表示前进，负数表示后退。
3. 如果只想实现前进后退可以使用this.$router.forward()（前进一页），以及this.$router.back()（后退一页）。
```html
<div id="app">
    <p>
        <button @click="toHome">首页</button>
        <button @click="toNews">新闻</button>
        <button @click="toLogin">登陆</button>
        <button @click="doForward1">前进</button>
        <button @click="doForward2">前进</button>
        <button @click="doBack1">后退</button>
        <button @click="doBack2">后退</button>
    </p>
    <router-view></router-view>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue-router/dist/vue-router.js"></script>
<script type="text/javascript">
    const Home = {
        template: '<div>首页</div>'
    }
    const News = {
        template: '<div>新闻：</div>'
    }
    const Login = {
        template: '<div>登陆</div>'
    }
    const routes = [{
        path: '/',
        component: Home
    }, {
        path: '/home',
        component: Home
    }, {
        path: '/news',
        component: News
    }, {
        path: '/login',
        component: Login
    }]
    const router = new VueRouter({
        routes
    })
    var vm = new Vue({
        el: '#app',
        data: {},
        router,
        methods:{
            toHome(){
                //无参数时，push方法中直接写路由地址
                this.$router.push('/home');
            },
            toNews(){
                //有参数时，push方法中写一个json对象
                this.$router.push({path:'/news',query:{name:'zhangsan'}});
            },
            toLogin(){
                this.$router.push('/login');
            },
            doForward1(){
                this.$router.forward();
            },
            doForward2(){
                this.$router.go(1);
            },
            doBack1(){
                this.$router.back();
            },
            doBack2(){
                this.$router.go(-1);
            }
        }
    })
</script>
```
## 16.2.通过watch实现路由监听
通过watch属性设置监听$route变化，达到监听路由跳转的目的。
在上面代码中添加watch监听：
```javascript
watch: {
    // 监听路由跳转。
    $route(newRoute, oldRoute) {
        console.log('watch', newRoute, oldRoute)
    }
}
```
## 16.3.导航守卫
路由跳转前做一些验证，比如登录验证，是网站中的普遍需求。 对此，vue-route 提供了实现导航守卫（navigation-guards）的功能。
你可以使用 router.beforeEach 注册一个全局前置守卫：
```javascript
const router = new VueRouter({ ... })
router.beforeEach((to, from, next) => {
    // ...
})
```
每个守卫方法接收三个参数：
1. to：即将要进入的目标路由对象（去哪里），可以使用 to.path 获取即将要进入路由地址。
2. from：当前导航正要离开的路由对象（从哪来），可以使用 from.path 获取正要离开的路由地址。
3. next：一个函数，表示继续执行下一个路由。（如果没有next，将不会进入到下一个路由）
下面例子中实现了如下功能：
1. 列举需要判断登录状态的 “路由集合”，当跳转至集合中的路由时，如果“未登录状态”，则跳转到登录页面
2. 当直接进入登录页面LoginPage时，如果“已登录状态”，则跳转到首页HomePage；
```html
<div id="box">
    <p>
        <router-link to="/home">home</router-link>
        <router-link to="/news">news</router-link>
        <router-link to="/music">music</router-link>
        <router-link to="/login">login</router-link>
    </p>
    <router-view></router-view>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue-router/dist/vue-router.js"></script>
<script type="text/javascript">
    const Home = {
        template: '<div>首页</div>'
    }
    const News = {
        template: '<div>新闻</div>'
    }
    const Music = {
        template: '<div>音乐</div>'
    }
    const Login = {
        template: '<div>登录</div>'
    }
    const routes = [{
        path: '/',
        component: Home
    }, {
        path: '/home',
        component: Home
    }, {
        path: '/news',
        component: News
    }, {
        path: '/music',
        component: Music
    }, {
        path: '/login',
        component: Login
    }]
    const router = new VueRouter({
        routes // （缩写）相当于 routes: routes
    })
    var vm = new Vue({
        el: '#box',
        data: {},
        router
    })
    // 添加全局路由守卫
    router.beforeEach((to, from, next) => {
        //创建守卫规则集合(这里表示'/news'与'/music'路径是需要权限验证的)
        const nextRoute = ['/news', '/music'];
        // 使用isLogin来模拟是否登录
        let isLogin = false;
        // 判断to.path(要跳转的路径)是否是需要权限验证的
        if (nextRoute.indexOf(to.path) >= 0) {
            if (!isLogin) {
                router.push({
                    path: '/login'
                })
                location.reload(); //必须要有
            }
        }
        // 已登录状态；当路由到login时，跳转至home
        if (to.path === '/login') {
            if (isLogin) {
                router.push({
                    path: '/home'
                });
                location.reload();
            }
        }
        next(); //必须要有
    });
</script>
```
# 17.Vue生命周期
在使用vue进行日常开发中，我们总有这样的需求，想在页面刚一加载出这个表格组件时，就发送请求去后台拉取数据，亦或者想在组件加载前显示个loading图，当组件加载出来就让这个loading图消失等等这样或那样的需求。
要实现这些需求，最重要的一点就是我怎么知道这个组件什么时候加载，换句话说我该什么时候向后台发送请求，为了解决这种问题，组件的生命周期钩子函数就应运而生。
## 17.1.Vue生命周期图示
下面这张图，就是Vue官网给我们展示的Vue生命周期图：
![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p04_01.png)
这是官方文档给出的一个组件从被创建出来到最后被销毁所要经历的一系列过程，所以这个过程也叫做一个组件的生命周期图。从图中我们可以看到，一个组件从被创建到最后被销毁，总共要经历以下8个过程：
1. beforeCreate:组件实例创建之前
2. created：组件实例创建完毕
3. beforeMount：组件DOM挂载之前
4. mounted：组件DOM挂载完毕
5. beforeUpate：组件数据更新之前
6. updated：组件数据更新完毕
7. beforeDestoy：组件实例销毁之前
8. destoyed：组件实例销毁完毕
## 17.2.代码演示
```html
<div id="app">
    <h1></h1>
    <button @click="add">加</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            num:10
        },
        //组件实例创建之前
        beforeCreate() {
            console.log('beforeCreate 组件实例创建前');
        },
        //组件实例创建完毕
        created() {
            console.log('created 组件实例创建完毕');
        },
        //组件DOM挂载之前
        beforeMount() {
            console.log('beforeMount 组件DOM挂载前');
        },
        //组件DOM挂载完毕
        mounted() {
            console.log('mounted 组件DOM挂载完毕');
        },
        //组件数据更新之前
        beforeUpdate() {
            console.log('beforeUpdate 组件数据更新前');
        },
        //组件数据更新完毕
        updated() {
            console.log('updated 组件数据更新完毕');
        },
        //组件实例销毁之前
        beforeDestroy() {
            console.log('beforeDestroy 组件实例销毁前');
        },
        //组件实例销毁完毕
        destroyed() {
            console.log('destroyed 组件实例销毁完毕');
        },
        methods:{
            //改变数据，可以演示beforeUpdate与updated
            add(){
                this.num++;
            }
        }
    })
    //使用$destroy()销毁当前实例，可以演示beforeDestroy与destroyed
    //app.$destroy();
</script>
```
## 17.3.总结
以上就是vue中组件生命周期钩子函数执行的各个过程以及执行的时机，但是这些钩子函数到底该怎么用呢？针对前言中提出的需求我们又该怎么解决呢？在这里，给大家举个例子：
> 例如有一个表格组件：
>
> 1. 我们想在表格加载之前显示个loading图，那么我们可以在组件实例创建之前的钩子函数beforeCreate里面将loading图显示。
> 2. 当组件实例加载出来，我们可以在created钩子函数里让这个loading图消失。
> 3. 当表格被加载好之后我们想让它马上去拉取后台数据，那么我们可以在组件DOM挂载之前的钩子函数beforeMount里面去发送请求。
> 4. 从后台拉取的数据要绑定在DOM中，那么就必须要在组件DOM挂载完毕的钩子函数mounted里面去做。
> 5. 表格中的数据在更新前和更新后，我们都需要做一个处理，那么这些工作就可以放在beforeUpdate和updated中去做。
> 6. 当应用程序结束后，或组件实例准备销毁时，有一些善后处理的工作（比如释放资源）就可以放在beforeDestroy和destroyed中去做。
# 18 搭建Vue-Cli脚手架
vue-cli 是一个官方发布 vue.js 项目脚手架，使用 vue-cli 可以快速创建 vue 项目。 下面介绍 vue-cli 的整个搭建过程。
> 注意：以下内容是基于Vue-cli4.0以上版本的。
## 18.1.安装npm
NPM（node package manager）是随同node.js一起安装的包管理工具，能解决前端代码部署上的很多问题，常见的使用场景有以下几种：
1. 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
2. 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
3. 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。
实际上，npm就是前端的Maven。
从node官网下载并安装node，安装步骤很简单，只要一路next就可以了。 安装完成后，打开命令行工具输入命令node -v，如下图，如果出现对应版本号，就说明安装成功了。
```c
C:\Users\Administrator>node -v
v12.18.0
```
我们所需要的npm也已经安装好了，输入如下命令，显示出npm的版本信息。
```c
C:\Users\Administrator>npm -v
6.14.4
```
到这里node的环境已经安装完了,npm包管理工具也有了，但是由于npm的有些资源被墙，为了更快更稳定,所以我们需要切换到淘宝的npm镜像——cnpm。
## 18.2.安装cnpm
点击进入淘宝的cnpm网站,里面有详细的配置方法。 或者直接在命令行输入：
```c
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
然后等待，安装完成。 输入cnpm -v，可以查看当前cnpm版本，这个和npm的版本还是不一样的。
```c
C:\Users\Administrator>cnpm -v
cnpm@6.1.1
```
使用cnpm的方法就是，需要用到npm的地方直接使用cnpm替换就可以了。
## 18.3.全局安装 vue-cli
全局安装vuecli（就相当与在本机的npm仓库中有了vuecli）：
```c
//安装最新@vue/cli版本
npm install -g @vue/cli
//安装指定的@vue/cli版本    
npm install -g @vue/cli@4.1.1
```
安装完成后，使用 vue -V  显示版本号来测试vue是否安装成功。
```c
C:\Users\Administrator>vue -V
@vue/cli 4.1.1
```
## 18.4.创建vue-cli工程
1. 在命令行下进入到工作空间文件夹中，输入如下命令：
   vue create hello     hello是工程名（注意：工程名必须全部小写）
2. 选择预设模板。这里选择“Manually select features”（手动选择特征）
3. 通过 ↑↓ 箭头选择依赖，按 “空格” 选中，按 “a” 全选，按 “i” 反选。
   1. Babel：转码器，可以将ES6代码转为ES5代码，可兼容不支持ES6的浏览器。 
   2. TypeScript：是JavaScript的超集（.ts文件），包含并扩展了 JavaScript 的语法。需要被编译输出为 JavaScript在浏览器运行。
   3. Progressive Web App (PWA) Support：渐进式Web应用程序
   4. Router ：vue-router（vue路由）
   5. Vuex ：vuex（vue的状态管理模式）
   6. CSS Pre-processors ：CSS 预处理器（如：less、sass）
   7. Linter / Formatter：代码风格检查和格式化（如：ESlint）
   8. Unit Testing ：单元测试（unit tests）
   9. E2E Testing ：e2e（end to end） 测试
      第一次创建工程时，可以只选择Babel和Router即可。
4. 选择是否使用history 形式的路由，也就是询问路径是否带 # 号，这里选择 n。
5. 询问将依赖文件放在独立文件中，还是package.json中：为了保持工程配置文件的整洁性，这里选择“In package.json”。
6. 询问是否将当前选择保存以备下次使用。这里选择“n”。
7. 接下来耐心等待安装：最后出现：
   ![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p06_01.png)
   按照提示，进入工程根目录。
8. 输入：npm run serve 启动工程。
   ![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p06_02.png)
9. 启动成功。
   ![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p06_03.png)
10. 在浏览器中测试：http://localhost:8080
    ![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p06_04.png)
# 19.Vue-cli工程目录结构及运行分析
![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p06_05.png)
1. package.json文件：这里配置了本工程中安装的模块：
   ```javascript
   "dependencies": {
       "core-js": "^3.6.5",
       "vue": "^2.6.11",
       "vue-router": "^3.2.0"
   },
   ```
2. html入口文件：public目录中的index.html文件。在此文件中有一个总容器：
   > 注意：整个工程中只有index.html一个页面，所以，Vue-cli工程就是一个SPA（单页应用）工程。
   ```html
   <div id="app"></div>
   ```
3. javascript入口文件：src目录中的main.js文件。在入口js文件中：
   1. 首先导入了vue模块，路由模块和App.vue主vue组件。
   2. 然后创建了一个Vue实例。
   3. 在此Vue实例中，调用了render（渲染视图函数）函数，此函数返回App.vue主组件。也就是使用App.vue来渲染index.html视图组件。
   4. 最后，使用$mount方法，将index.html中的总容器（#app）挂载到Vue实例上。
4. App.vue主组件：
   template标签中书写当前组件html代码。这里调用了公共组件HelloWorld。
   script标签中书写当前组件的js代码。这里导入了HelloWorld公共组件成为子组件。
   style标签中书写当前组件的css代码。
   > 注意：一个vue文件，就是一个Vue组件
5. HelloWorld.vue公共组件：
   这是vuecli给我们提供的一个示例组件。至此，我们终于知道：为什么运行后浏览器中会显示这样的页面。
6. 最后，在package.json文件中有这样的配置：
   ```javascript
   "scripts": {
       "serve": "vue-cli-service serve",
       "build": "vue-cli-service build"
   }
   ```
   于是，我们知道了，npm run serve命令，实际上就是运行“vue-cli-service serve”
## 19.1 Vue-cli工程配置文件
vuecli3.x以上版本中为了精简配置文件，专门设置了一个可选的配置文件。 只要在项目的根目录（与package.json同级）中，添加名称为vue.config.js的文件，就能自动被vue-cli加载。 在此文件中可以配置vuecli工程中的一些配置。
```javascript
module.exports = {
    lintOnSave:false, //关闭代码格式化校验工具
    devServer:{
        port: 81 //修改启动端口
    }
}
```
## 19.2 自定义组件
```vue
<template>
    <div>
        自定义组件
        <hr/>
        {{msg}}
    </div>
</template>
<script>
    export default {
        name:'First',
        data(){
            return {
                msg: '自定义数据'
            }
        }
    }
</script>
<style>
</style>
```
19.3 在App.vue中使用
```vue
<template>
    <div id="app">

        <First />
    </div>

</template>
<script>
    import First from './components/First.vue'
    export default {
        name: 'App',
        components: {
            First
        },
        data() {
            return {
            }
        }
    }
</script>
<style>
</style>
```
# 20.使用Vue-cli完成todoList案例
todoList案例功能介绍：
1. 输入框输入待办事项后点击添加，该事项会出现在todo区域内，输入框内的文字消失
2. todo区域内的事项为待办事项，点击某一事项后，该事项移入done区域
3. done区域内的事项为完成事项，点击某一事项后，该事项消失
![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p06_06.png)
## 20.1.工程目录结构
![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p06_07.png)
1. 主组件App.vue
2. 共通组件：AddNew.vue、ToDoList.vue,DoneList.vue
## 20.2.组件代码
共通组件 Input.vue
```vue
<template>
  <div>

      <input type="text" v-model="inp" placeholder="请输入内容" /> <button @click="add"> 添加</button>

</div>
</template>
<script>
  export default {
    name: 'Template',
    components: {
    },
    data() {
      return {
          inp:''
      }
    },
    methods: {
        add(){
            this.$emit('get',this.inp);
            this.inp = '';
        }
    },
    created() { }
  }
</script>
<style>
</style>
```
共通组件 ToDoList.vue
```vue
<template>
  <div>

      <ol class="list">
          <li v-for="(todo ,index) in todolist"  >{{todo}}  <button @click="rm(index)">×</button>  </li>
      </ol>

</div>
</template>
<script>
  export default {
    name: 'ToDoList',
    props:['todolist'],
    components: {
    },
    data() {
      return {
      }
    },
    methods: {
        rm(index){

            this.$emit('rm',index);
        }
    },
    created() { }

  }
</script>

<style scoped>

    .list li{
        font-size: 30px;
    }


</style>

```
组件DoneList.vue
```vue
<template>
  <div>

      <ol>
              <li v-for="(todo ,index) in list"  >{{todo}}    </li>
      </ol>

  </div>
</template>
<script>
  export default {
    name: 'DoneList',
    props:['list'],
    components: {
    },
    data() {
      return {
      }
    },
    methods: {
    },
    created() { }
  }
</script>
<style>
</style>
```

主组件App.vue

```vue
<template>
    <div id="app">

        <Input  @get="get"></Input>
        待处理任务:<br/>

        <ToDoList @rm="rm" :todolist="todolist"></ToDoList>
        已完成任务:<br/>
        <DoneList :list="doneList"></DoneList>
    </div>
</template>
<script>
    //@    src  
    import Input from '@/components/Input'
    import ToDoList from '@/components/ToDoList'
    import DoneList from '@/components/DoneList'
    export default {
        name: 'App',
        components: {
            Input,
            ToDoList,
            DoneList
        },
        data() {
            return {
                todolist:[],
                doneList:[]
            }
        },
        methods:{
            get(inp){
                this.todolist.push(inp)
            },
            rm(index){
                // alert("parent: "+index);
                this.doneList.push(this.todolist.splice(index,1)[0])
            }
        }
    }
</script>
<style>
</style>
```

# 21.安装vue-router

```bash
npm install vue-router
```

如果在一个模块化工程中使用它，必须要通过 `Vue.use()` 明确地安装路由功能：

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
```

创建router目录，在router目录下创建index.js

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
import A from '@/components/A'
import B from '@/components/B'
export default new VueRouter({
  mode: 'history',
  routes: [
    {
      path: '/aaa',
      name: 'A',
      component: A
    },
    {
      path: '/bbb',
      name: 'B',
      component: B
    }
  ]
})
```

在main.js中引入

```js
import Vue from 'vue'
import App from './App.vue'
// import VueRouter from 'vue-router'
import router from  './router'
Vue.config.productionTip = false
new Vue({
 router,
  render: h => h(App),
}).$mount('#app')
```

在App.vue中使用路由

```html
<router-link to="/aaa">aaa</router-link>

<router-link to="/bbb">bbb</router-link>

<router-view></router-view>
```

# 22.element-ui插件

ele团队开发的基于vue2.0的组件。 [官网](https://element.eleme.io/#/zh-CN)
安装

```
npm i element-ui -S
```

在main.js中import 

```js
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
```

- 使用
  在APP.vue中使用布局容器
  
  ```html
  <template>
    <div id="app">
     <el-container>
       <el-header class="header">Header</el-header>
       <el-container>
         <el-aside width="200px" class="aside">Aside</el-aside>
         <el-main>Main</el-main>
       </el-container>
     </el-container>
    </div>
  </template>
  <script>
    export default {
        name: 'App',
        components: {
  
        },
        data() {
            return {
  
            }
        },
        methods:{
        }
    }
  </script>
  <style scoped>
  .header{
    border-bottom: 1px solid aqua ;
  }
  .aside{
    border-right: 1px solid aqua;
  }
  </style>
  ```
  
  ## 22.1 封装组件使用路由的方式访问

- 在views 下面封装了index.vue
  
  ```html
  <template>
      <div>
          <el-container class="wrap" >
            <el-header class="header">Header</el-header>
            <el-container>
              <el-aside width="200px" class="aside">Aside</el-aside>
              <el-main>Main</el-main>
            </el-container>
          </el-container>
      </div>
  </template>
  
  <script>
  </script>
  
  <style scoped>
  
      .wrap{
          height: 100vh;
      }
  
      .header{
          border-bottom: 1px solid aqua ;
      }
  
      .aside{
          border-right: 1px solid aqua;
      }
  </style>
  ```

- views/login/index.vue 登录
  
  ```html
  <template>
  <div>登录画面</div>
  </template>
  <script>
  export default {
    name: 'Login',
    components: {
    },
    data() {
      return {
      }
    },
    methods: {
    },
    created() { }
  }
  </script>
  <style>
  </style>
  ```

- 修改App.vue
  
  ```html
  <div id="app">
  
        <router-view></router-view>
  
    </div>
  ```

- 路由
  
  ```js
  import Vue from 'vue'
  import VueRouter from 'vue-router'
  Vue.use(VueRouter);
  export default new VueRouter({
    mode:'hash',
    routes:[
        {
            path:'/',
            name:'Index',
            component:()=>import('@/views')
        },
        {
            path:'/login',
            name:'Login',
            component:() =>import('@/views/login')
        }
    ]
  });
  ```
  
  ## 22.2 使用菜单组件
  
  在src/components目录下创建menu.vue组件
  
  ```html
  <template>
    <el-menu default-active="2" class="el-menu-vertical-demo" router background-color="#222D32" text-color="#B8C7CE"
     active-text-color="#fff">
        <el-submenu index="1">
            <template slot="title">
                <span>权限管理</span>
            </template>
            <el-menu-item-group>
                <el-menu-item index="/user"> <i class="el-icon-location"></i>用户管理</el-menu-item>
                <el-menu-item index="/menu"> <i class="el-icon-location"></i>菜单管理</el-menu-item>
            </el-menu-item-group>
        </el-submenu>
  
        <el-submenu index="2">
            <template slot="title">
                <span>ElementUI组件</span>
            </template>
            <el-menu-item-group>
                <el-menu-item index="/table"> <i class="el-icon-location"></i>表格组件</el-menu-item>
            </el-menu-item-group>
        </el-submenu>
    </el-menu>
  </template>
  <script>
    export default {
        name: 'Menu'
    }
  </script>
  <style>
  </style>
  ```
  
  在home中import
  
  ```php+HTML
  <template>
    <div>
        <el-container class="wrap" >
          <el-header class="header">Header</el-header>
          <el-container>
            <el-aside width="280px" class="aside">
                <Menu></Menu>
            </el-aside>
            <el-main>
                <router-view></router-view>
            </el-main>
          </el-container>
        </el-container>
    </div>
  </template>
  <script>
    import Menu from '@/components/menu'
    export default{
        name:'Index',
        components:{
            Menu
        }
    }
  </script>
  <style scoped>
  
    .wrap{
        height: 100vh;
    }
  
    .header{
        border-bottom: 1px solid aqua ;
        background-color: cornflowerblue;
    }
  
    .aside{
        background-color: #222D32;
        border-right: 1px solid aqua;
    }
  </style>
  ```
  
  # 23.Axios 网络请求
  
  ## 23.1 安装
  
  ```
  npm install axios
  ```
  
  ![](../../imgs/vuejs/2020-12-03_165913.png)
  
  ## 23.2 在组件中引入axios
  
  ```js
  import axios from  'axios'
  ```

- 在create函数中使用
  
  ```js
  axios.get('stu.json')
    .then((res)=>{
    this.tableData = res.data
  })
  .catch(function (error) {
    console.log(error);
  });
  ```

- 完整的使用代码
  
  ```html
  <template>
    <div>
        <el-table :data="tableData" style="width: 100%" size="mini">
             <el-table-column
                    prop="name"
                    label="直接显示名字"
                    width="180">
            </el-table-column>
  
            <el-table-column label="日期" width="180">
                <template slot-scope="scope">
                    <i class="el-icon-time"></i>
                    <span style="margin-left: 10px">{{ scope.row.date }}</span>
                </template>
  
            </el-table-column>
  ```
  
            <el-table-column label="姓名" width="180">
                <template slot-scope="scope">
                    <el-popover trigger="hover" placement="top">
                        <p>姓名: {{ scope.row.name }}</p>
                        <p>住址: {{ scope.row.address }}</p>
                        <div slot="reference" class="name-wrapper">
                            <el-tag size="medium">{{ scope.row.name }}</el-tag>
                        </div>
                    </el-popover>
                </template>
            </el-table-column>
      
            <el-table-column label="操作">
                <template slot-scope="scope">
                    <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                    <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                </template>
            </el-table-column>
        </el-table>
  
  </div>

</template>
<script>
    import axios from  'axios'
    export default {
        name: 'Table',
        components: {
        },
        data() {
            return {
                tableData: []
            }
        },
        methods: {
        },
        created() {
            axios.get('stu.json')
             .then((res)=>{
                 this.tableData = res.data
             })
              .catch(function (error) {
                console.log(error);
              });

        }
    }

</script>

<style>
</style>

```
## 23.3 全局配置
配置通用路径
```js
axios.defaults.baseURL = 'http://127.0.0.1:8090/web';
```

# 24 Vuex

## 24.1 Vuex是什么

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。Vuex 也集成到 Vue 的官方调试工具 [devtools extension](https://github.com/vuejs/vue-devtools)[ ](https://github.com/vuejs/vue-devtools)[ (opens new window)](https://github.com/vuejs/vue-devtools)，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。

### 24.1.1 什么是“状态管理模式”？

让我们从一个简单的 Vue 计数应用开始：

```js
new Vue({
  // state
  data () {
    return {
      count: 0
    }
  },
  // view
  template: `
    <div>{{ count }}</div>
  `,
  // actions
  methods: {
    increment () {
      this.count++
    }
  }
})
```

这个状态自管理应用包含以下几个部分：

- **state**，驱动应用的数据源；

- **view**，以声明方式将 **state** 映射到视图；

- **actions**，响应在 **view** 上的用户输入导致的状态变化。
  以下是一个表示“单向数据流”理念的简单示意：
  ![](https://vuex.vuejs.org/flow.png)
  但是，当我们的应用遇到**多个组件共享状态**时，单向数据流的简洁性很容易被破坏：

- 多个视图依赖于同一状态。

- 来自不同视图的行为需要变更同一状态。
  对于问题一，传参的方法对于多层嵌套的组件将会非常繁琐，并且对于兄弟组件间的状态传递无能为力。对于问题二，我们经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝。以上的这些模式非常脆弱，通常会导致无法维护的代码。
  因此，我们为什么不把组件的共享状态抽取出来，以一个全局单例模式管理呢？在这种模式下，我们的组件树构成了一个巨大的“视图”，不管在树的哪个位置，任何组件都能获取状态或者触发行为！
  通过定义和隔离状态管理中的各种概念并通过强制规则维持视图和状态间的独立性，我们的代码将会变得更结构化且易维护。
  ![vuex](https://vuex.vuejs.org/vuex.png)
  
  ### 24.1.2 什么情况下我应该使用 Vuex？
  
  Vuex 可以帮助我们管理共享状态，并附带了更多的概念和框架。这需要对短期和长期效益进行权衡。
  如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。确实是如此——如果您的应用够简单，您最好不要使用 Vuex。一个简单的 [store 模式](https://cn.vuejs.org/v2/guide/state-management.html#简单状态管理起步使用)就足够您所需了。但是，如果您需要构建一个中大型单页应用，您很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择。引用 Redux 的作者 Dan Abramov 的话说就是：
  
  > Flux 架构就像眼镜：您自会知道什么时候需要它。
  
  ## 24.2 安装
  
  ```bash
  npm install vuex --save
  ```
  
  在一个模块化的打包系统中，您必须显式地通过 `Vue.use()` 来安装 Vuex
  
  ```js
  import Vue from 'vue'
  import Vuex from 'vuex'
  Vue.use(Vuex)
  ```
  
  ## 24.3 开始
  
  每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。Vuex 和单纯的全局对象有以下两点不同：
1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。

2. 你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地**提交 (commit) mutation**。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。
   
   ## 24.4.最简单的 Store
   
   [安装](https://vuex.vuejs.org/zh/installation.html) Vuex 之后，让我们来创建一个 store。创建过程直截了当——仅需要提供一个初始 state 对象和一些 mutation：
   
   ```js
   import Vue from 'vue'
   import Vuex from 'vuex'
   Vue.use(Vuex)
   const store = new Vuex.Store({
   state: {
    count: 0
   },
   mutations: {
    increment (state) {
      state.count++
    }
   }
   })
   ```
   
   现在，你可以通过 `store.state` 来获取状态对象，以及通过 `store.commit` 方法触发状态变更：
   
   ```js
   store.commit('increment')
   console.log(store.state.count) // -> 1
   ```
   
   为了在 Vue 组件中访问 `this.$store` property，你需要为 Vue 实例提供创建好的 store。Vuex 提供了一个从根组件向所有子组件，以 `store` 选项的方式“注入”该 store 的机制：
   
   ```js
   new Vue({
   el: '#app',
   store: store,
   })
   ```
   
   提示
   如果使用 ES6，你也可以以 ES6 对象的 property 简写 (用在对象某个 property 的 key 和被传入的变量同名时)：
   
   ```js
   new Vue({
   el: '#app',
   store
   })
   ```
   
   现在我们可以从组件的方法提交一个变更：
   
   ```js
   methods: {
   increment() {
    this.$store.commit('increment')
    console.log(this.$store.state.count)
   }
   }
   ```
   
   再次强调，我们通过提交 mutation 的方式，而非直接改变 `store.state.count`，是因为我们想要更明确地追踪到状态的变化。这个简单的约定能够让你的意图更加明显，这样你在阅读代码的时候能更容易地解读应用内部的状态改变。此外，这样也让我们有机会去实现一些能记录每次状态改变，保存状态快照的调试工具。有了它，我们甚至可以实现如时间穿梭般的调试体验。
   由于 store 中的状态是响应式的，在组件中调用 store 中的状态简单到仅需要在计算属性中返回即可。触发变化也仅仅是在组件的 methods 中提交 mutation。
   这是一个[最基本的 Vuex 记数应用](https://jsfiddle.net/n9jmu5v7/1269/)
   vuex数据持久化
   https://juejin.cn/post/6844903650427404302
   
   # 24.Vue-cli的打包部署
   
   ## 24.1.Vue-cli工程为什么要打包部署
   
   我们所完成的工程，最终都是要部署到服务器（Tomcat、Nginx等等）上运行的。但是，服务器是不可能识别vue文件的，因为vue文件只是Vue-cli工程中的一种自定义文件。
   所以，我们需要将vue文件中的html代码、css代码、js代码抽取出来，重新打包成真正的html文件、css、文件和js文件，然后才能部署到服务器上。
   ![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p06_08.png)
   
   ## 24.2.Vue-cli工程打包配置
   
   在vue.config.js文件中添加如下配置：
   
   ```js
   module.exports = {
    //打包基本目录
    publicPath:'./',
    //输出目录
    outputDir:'dist',
    //静态资源目录
    assetsDir:'assets',
    ... ...
   }
   ```
   
   在命令行中进入到工程目录，运行如下命令：
   
   ```c
   npm run build
   ```
   
   在工程目录中，就会出现 "dist" 目录，这就是打包好的工程
   ![img](https://null_688_6639.gitee.io/javase/15Vue/assets/p06_09.png) 
   
   > 注意：打包工程必须要放在一个 HTTP 服务器中才能运行。
   
   # 26.整合springboot+jwt
- 搭建springboot-web项目

- 添加jwt
  
  > jwt整合

- 导包
  
  ```xml
  <dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>3.4.0</version>
  </dependency>
  ```

- 生成token(JSON)
  
  ```java
  package com.neuedu.jwt.controller;
  import com.auth0.jwt.JWT;
  import com.auth0.jwt.JWTCreator;
  import com.auth0.jwt.algorithms.Algorithm;
  import com.neuedu.jwt.User;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RestController;
  @RestController
  public class LoginController {
    public static final String KEY = "loginUser";
    /**
     * http://www.jshand.org/login
     * 登录，  原始 （username、password ->sessionid） 、 （username、password ->token（json））
     * @return
     */
    @RequestMapping("/login")
    public String login(){
        //验证用户名、密码
        //通过之后，返回登录的令牌 token(JSON)
        User user = new User(); //模拟从数据库中查询的 用户信息
        user.setId(1);
        user.setUserName("admin");
        user.setDisplayName("管理员");
        user.setTelphone("13888888888");
        JWTCreator.Builder builder = JWT.create();
        String token = builder.withClaim("userId", user.getId())
                .withClaim("userName", user.getUserName())
                .sign(Algorithm.HMAC256(KEY));
        return token;
    }
  }
  ```
  
  ```java
  package com.neuedu.jwt;
  public class User {
    private Integer id;
    private String userName;
    private String displayName;
    private String telphone;
    public Integer getId() {
        return id;
    }
    public void setId(Integer id) {
        this.id = id;
    }
    public String getUserName() {
        return userName;
    }
    public void setUserName(String userName) {
        this.userName = userName;
    }
    public String getDisplayName() {
        return displayName;
    }
    public void setDisplayName(String displayName) {
        this.displayName = displayName;
    }
    public String getTelphone() {
        return telphone;
    }
    public void setTelphone(String telphone) {
        this.telphone = telphone;
    }
    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", displayName='" + displayName + '\'' +
                ", telphone='" + telphone + '\'' +
                '}';
    }
  }
  ```

- 发送给Vuejs（存储到sessionstorage，放到header）

- Vuejs请求的时候，携带 token(使用header形式),如果刷新，从sessionstorage取出
