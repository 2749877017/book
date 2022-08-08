# Vue3.x绑定数据与循环

## 一、绑定数据

### **1、业务逻辑：**

```javascript
export default {
    name: "App",
    data() {
        return {
            msg: "你好vue",
            userinfo: {
                username: "张三",
                age: 20
            }
        };
    },
};
```

### 2. **template模板：**

```html
<template> 

  <p>msg的值：{{ msg }}</p>

  <p>绑定对象：{{ userinfo.username }}</p>

</template>
```

## 二、v-html绑定html

### 1、业务逻辑：

```javascript
export default {
    name: "App",
    data() {
        return {
            h2: "<h2>这是一个html内容</h2>"
        };
    },
};
```

### **2、template模板**

```html
<span v-html="h2"></span>
```

## 三、v-bind绑定属性

### 1、业务逻辑：

```js
export default {
    name: "App",
    data() {
        return {
            logoSrc: "https://www.itying.com/themes/itying/images/logo.gif"
        };
    },
};
```

### **2、template模板：**

```html
<img :src="logoSrc" alt="logo">
```

## 四、v-bind动态参数

```html
<a v-bind:[attributeName]="url"> ... </a>
```

这里`attributeName`将被动态地评估为JavaScript表达式，并且其评估值将用作参数的最终值。例如，如果您的组件实例具有一个数据属性`attributeName`，其值为`"href"`，则此绑定将等效于`v-bind:href`

### 1、业务逻辑：

```js
export default {
    name: "App",
    data() {
        return {
           attributeName: "href",
           linkUrl: "http://www.itying.com",
        };
    },
};
```

### 2、template模板：

```html
<a v-bind:[attributeName]="linkUrl"> 这是一个地址 </a>
或者
<a :[attributeName]="linkUrl"> 这是一个地址 </a>
```

## 五、v-for循环数组

### 1、业务逻辑

```js
export default {
    name: "App",
    data() {
        return {
            list1: ['马总', '刘总', '李总'],
            list2: [{
                    'title': '新闻111'
                },
                {
                    'title': '新闻222'
                },
                {
                    'title': '新闻33'
                },
                {
                    'title': '新闻44'
                }
            ],
            list3: [{
                    "cate": "国内新闻",
                    "list": [

                        {
                            'title': '国内新闻11111'
                        },
                        {
                            'title': '国内新闻2222'
                        }
                    ]
                },
                {
                    "cate": "国际新闻",
                    "list": [

                        {
                            'title': '国际新闻11111'
                        },
                        {
                            'title': '国际新闻2222'
                        }
                    ]
                }

            ]

        };
    },
};
```

### 2、template模板

注意vue3.x中循环数据需要有唯一的key值，代码如下

```html
<ul>
    <li v-for="(item,index) in list1" :key="index">
        {{item}}
    </li>
</ul>
```

```html
<ul>
    <li v-for="(item,index) in list2" :key="index">
        {{item.title}}
    </li>
</ul>
```

```html
<ul>
    <li v-for="(item,index) in list3" :key="index">
        {{item.cate}}
        <ol>
            <li v-for="(news,i) in item.list" :key="i">
                {{news.title}}
            </li>
        </ol>

    </li>
</ul>
```

## 六、v-for循环对象

### 1、业务逻辑

```js
export default {
    name: "App",
    data() {
        return {           
             myObject: {
                title: 'How to do lists in Vue',
                author: 'Jane Doe',
                publishedAt: '2020-03-22'
            }
        };
    },
};
```

### 2、template模板

```html
<ul id="v-for-object" class="demo">

  <li v-for="(value, name, index) in myObject" :key="index">
       {{ name }}: {{ value }}--{{index}}
  </li>
</ul>
```
