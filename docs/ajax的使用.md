# ajax的使用

## 一.post请求

```js
$.ajax({
    url:"hthfAdd",//ReqestMapping中的url路径
    type:"post",//请求方式
    data:{      //参数内容
        content:content,
        htid:hiddenhtid,
    },
    success:function(msg){

        }

})
```

## 二.get请求

```js
$.ajax({
      type: "get",
      url: "memberLogin", //ReqestMapping中的url路径
      data: "uname="+uname+"&upass="+upass+"&yzm="+yzm,//参数
      success: function(data){        //返回体

            }
        });
```
