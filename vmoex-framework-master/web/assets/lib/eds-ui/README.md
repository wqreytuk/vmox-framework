# edsUI

简单实用的web端js组件，包含功能：

 - [弹出层模态框](https://github.com/Jaggle/eds-ui#遮罩层)
 - [使用post方法请求链接](https://github.com/Jaggle/eds-ui#post提交a标签)
 - [异步form表单提交](https://github.com/Jaggle/eds-ui#form-异步表单提交)
 - url处理
 - [js模板引擎](https://github.com/Jaggle/eds-ui#模板引擎)

## 安装

```
bower install eds-ui
```

## 测试示例

进入到eds-ui所在目录， 运行 `bower install`， 这会在eds-ui的根目录安装需要的依赖（依赖存放的目录为web/vendor）, 
浏览器打开文件sample.html即可使用示例。使用完之后可以删除生成的web目录

## 使用示例

本插件功能和设计相对比较简单，下面列出所有的使用方法，没有其他doc供参阅。

### 遮罩层

`edsUI.mask()` 会生成一个遮罩，是一个中间件，用于突出模态框和屏蔽网页上面的其他操作，建议配合alert、confirm等一起使用。
可以手动使用`edsUI.close()`关闭


### 吐司 - 自动消失的提示文字
会生成一个自动消失的提示信息，一般用于作用不是很大的提示信息。

```javascript
edsUI.toast("提示");        //没有遮罩
edsUI.mask().toast("提示"); //带遮罩
```


### 警告框

提示框，一般输出警告信息，错误信息。

```javascript
edsUI.alert("提示");
edsUI.mask().alert("提示");

edsUI.mask().alert('提示',function(){
    // 点击确定的回调函数
})

```

### 确认框

确认框，一些关键操作可以用confirm提到警示作用。

```javascript
edsUI.confirm("标题","提示的内容",['确定','取消'],function(){
   //第一个按钮的回调
},function(){
  // 第二个按钮的回调
});
```

`edsUI.mask().confirm("提示");`


//这是一个可变参数的函数，下面是一个只有两个参数但是使用次数比较多的例子：

```javascript
edsUI.confirm('提示内容',function(){
    //点击确定按钮的回调函数
})
```

### 弹出输入框

输入框 ，使用`e.content`获得输入内容

原型： `prompt: function (title,btnArray,btn1Call,btn2Call)`

简单实例：

```javascript
edsUI.prompt('请输入价格',function(e){
  edsUI.mask().alert('您输入的价格是' + e.content);
});
```

### html 自定义模态框内容

自定义模态框区域内容

原型：` html : function (title,htmlContent) {`

简单实例：

```javascript
edsUI.mask().html('标题',"<div class='content'>" +
 "<h1>你好</h1></div>");
```

### form 异步表单提交

使form表单变成异步提交。

form: `function (formId,callback,loading)`

简单实例： 

```javascript
edsUI.form('#form',function(result){
   console.log('服务器返回的内容是' + result);
   
   if(result.status) {
       //服务器返回的json,并且status为true，即操作成功
       console.log('表单提交成功');
       edsUI.toast(result.message);
   } else {
       //操作失败！
         console.log('表单提交失败');
       edsUI.mask().alert('result.message');
   }
   
},['#submit','text'])
```

上面的代码会自动提交选择器为#form的表单，并且在提交的过程中使#submit中的文字变成'正在提交...'

### 模板引擎

模板引擎使用 `edsUI.parse()` 方法可以将一段html代码，使用占位符表示变量，然后通过渲染可以得到所需的html节点，使用示例:

```html
<script type="text/html" id="tpl">
    <div class="block">
        <h1>@title@</h1>
        <p>@description@</p>
        <button>@action@</button>
    </div>
</script>
```

```javascript
edsUI.parse($('#tpl').html(), {
    title: '这里输入标题',
    description: '这里输入描述',
    action: '这里是操作'
}).appendTo('.container');
```

以上代码将会生成一个div节点，并且填充数据后附加到.container节点的后面。

### post提交a标签

有时候我们页面上面有个a标签或者一个button，会触发一个ajax事件，需要自己去写一个$.post()或者$.ajax()方法。现在只需要在a标签中添加一个data-edsPost属性 ，edsUI就能帮你提交你需要的数据了

	<a data-edsPost href="http://127.0.0.1:8000/order/approval/23">审核通过</a>

当点击上面的a标签的时候，edsUI会自动帮你提交一个post请求到href指定的地址。

### Why this name ?

因为当时这个组件是用于我前（前？）公司的一个项目，叫`E都市`,拼音缩写就是eds，所以一直沿用到现在。

### who will use ?

主要给后台开发人员使用，因为写的很简单，总共就200多行代码，专业前端当然不用啦，像我这种写php的，看了bs的模态框，竟然要自己写html，简直不能忍，而我这里 `edsUI.alert()` 也能生成一个效果很炫的提示框，简单且方便。自己写也不是一件很麻烦的事，所以自己动手，丰衣足食。
