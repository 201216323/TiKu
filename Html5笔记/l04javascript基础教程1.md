北京的天气越来越冷的，早上冷的都不想从被窝里面起来，昨天下午高强度的健身导致身上肌肉非常的酸痛，但是，学习这件事情最怕的是有懈怠之心，所以就起床来学习并写下这篇文章
------2016年11月7日早上。言归正传。

![image](http://p1.pstatp.com/large/110d0001c722c29daed6)


本节开始学习HTML5基础中的JavaSctipt。JavaScript基础总共有11个部分，这些课程以前也都学习接触过，但后来做了Android 开发也就没有在怎么用。

> 下面是目录部分。

1. JavaScript基础教程
1. JavaScript语法详解
1. JavaScript函数
1. JavaScript异常处理和事件处理
1. JavaScript DOM对象
1. JavaScript事件详解
1. JavaScript内置对象
1. JavaScript DOM对象控制HTML元素详解
1. JavaScript浏览器对象
1. JavaScript面向对象详解
1. Javascript瀑布流

> 本节部分主要来学习前六部分。

### 第一部分：JavaScript基础教程
##### 第一节、Javascript基础-介绍、实现、输出

> JavaScript介绍 ：

1、JavaScript是互联网上最流行的脚本语言，这门语言可用于web和HTML，更可广泛用于服务器、pc端、移动端。

2、JavaScript脚本语言：


```
JavaScript是一种轻量级的编程语言。
JavaScript是可插入HTML页面的编程代码。
JavaScript插入HTML页面后，可由所有的浏览器执行。
```

3、JavaScript实现 ：

> JavaScript用法：


```
HTML中的脚本必须位于<script></script>标签之间
脚本可被放置在HTML页面的<body>和<head>部分中。
```

> JavaScript标签：


```
在HTML中插入JavaScript，使用<script>标签
在<script></script>之间书写代码
```

> JavaScript使用限制：


```
在HTML中，不限制脚本数量
通常会把脚本放置于<head>标签中，以不干扰页面内容
```

4、JavaScript输出：

> JavaScript通常用来操作HTML。

> 文档输出：


```
document.writeln("<h1>标签h1</h1>")
```
简单的测试程序来测试JavaScript操作HTML：


```
<!DOCTYPE html>
<html>
<head lang="en">
<meta charset="UTF-8">
<title>JavaScript基础教程</title>
</head>
<body>
<p id="pid">Hello p</p>
<script >
    document.getElementById("pid").innerHTML="javaScript";
</script>
</body>
</html>
```
运行结果：

![image](http://p3.pstatp.com/large/10690002a8320c6ea391)


```
当不使用script标签的时候，应该输出Hello p，但在使用了script标签后这里替换掉了原来p标签中的内容，输出的是javaScript。
```


##### 第二节、Javascript基础-语法和注释
> JavaScript语法：

1、JavaScript语句：


```
JavaScript语句向浏览器发出的命令。语句的作用是告诉浏览器该做什么。
```

2、分号：


```
语句之间的分割是分号（;）
注意：分号是可选项，有时候看到不以分号隔开的。
```

3、JavaScript代码

```
按照编写顺序依次执行
```

4、标识符：

```
JavaScript标识符必须以字母、下划线或美元符号开始。
JavaScript关键字是不能作为标识符，这点和java也一样。
```

5、JavaScript对大小写敏感：

6、空格：

```
JavaScript会忽略到多余的空格
```

7、代码换行：

```
不要拆分JavaScript关键字来换行。
```

8、保留字：

![image](http://p1.pstatp.com/large/106d0006b20a990f4f5e)

> JavaScript注释：

1、单行注释： //

2、多行注释：/**/


##### 第三节、Javascript基础-变量和数据类型
> 1、Javascript变量：
var i$ = 10;

var j = 10;

var m = i$ + j;
> 2、Javascript数据类型：
1. 字符串（String）
1. 数字（Number）
1. 布尔（Boolean）
1. 数组（Array）
1. 对象（Object）
1. 空（Null）
1. 未定义
1. 可以通过赋值为null的方式清除变量

其实上面所提到的数据类型和Java语言中的数据类型都是一样的，毕竟都是处理逻辑的语言，肯定都有类似的关键字。
### 第二部分：JavaScript语法详解
##### 第一节：Javascript语法-运算符(1)

![image](http://p3.pstatp.com/large/106900053a299600449a)

本节讲述1、2、3、

简单测试程序：


```
<!DOCTYPE html>
<html>
<head lang="en">
<meta charset="UTF-8">
<title>Javascript语法-运算符</title>
</head>
<body>
    <p>i=10,j=10,i+j=?</p>
    <p id="sumid">运算结果</p>
    <button onclick="mysum()">结果</button>
    <script>
        function mysum(){
            var i=10;
            var j=10;
            var sum = i+j;
            document.getElementById("sumid").innerHTML = sum;
    }
    </script>
</body>
</html>
```
运行结果：

![image](http://p1.pstatp.com/large/106900053b60a8ac87b6)

当点击结果的时候，上面的p标签就会显示i+j的结果，这里只是练习一下function函数，其他的那些运算符和java中的运算符都差不多。


##### 第二节：Javascript语法-运算符(2)
本节讲述上节中的4、5、6、


```
”==“和”===“的区别：
var i="10";
var j=10;
var sum = i+j;
document.write(i==j);
上述返回true，但如果使用"==="来比较，返回就是false了。其他的涉及到两等和三等的运算符都是这样的。

条件运算符：
var i = 10;
var j = -1;
var sum = i + j;
document.write(sum > i ? "大于" : "不大于");
上面所示：其实也是三目运算符，这在java中同样也有，都一样。
```
##### 第三节：Javascript语法-条件语句if...else
条件语句同java中的条件语句一样，这里就不过多的表述。


```
<script>
var i = 11;
if (i > 10){
    document.write("i大于10");
}else if(i < 10){
    document.write("i小于10")
}else{
    document.write("i等于10")
}
</script>
```
##### 第四节：Javascript语法-条件语句switch
switch语句和java中的语法也是一样的。



```
<script>
    var i = 11;
    switch (i) {
        case 1:
        break;
        case 2:
        break;
        default :
        break
    }
</script>
```
##### 第五节：Javascript语法-循环语句for循环
for循环语句和java中的语法也是一样的。

```
for循环：
<script>
    var i=[1,2,3,4,5,6];
    for(var j=0;j<6;j++){
        document.write(i[j]+"、");
    }
</script>
    for in 循环：
    <script>
        var i=[1,2,3,4,5,6];
        var j=0;
        for(j in i){
            document.write(i[j]+"+");
    }
</script>
```
##### 第六节：Javascript语法-循环语句while循环
while循环语句和java中的语法也是一样的，分while循环和do...while循环。


```
while循环：
<script>
    var i = 1;
    while (i < 10) {
        document.write(i + "<br/>");
    i++;
    }
    </script>
    do.....while循环：
    <script>
        var i = 1;
    do {
        document.write(i + "<br/>");
    i++;
    } while (i < 10);
</script>
```

##### 第七节：Javascript语法-跳转语句
跳转语句和java中的语法也是一样的，分break和continue,break是跳出当前的循环，continue继续下一次循环。


```
break跳转:
<script>
    for (var i = 0; i < 10; i++) {
        if (i == 5) {
            break;
        }
        document.write(i + "+")
    }
</script>
输出结果是：0+1+2+3+4+
如果改成continue:
输出结果则是：0+1+2+3+4+6+7+8+9+
```
### 第五部分：JavaScript DOM对象
##### 5.1：Javascript-DOM简介


> 1、HTML DOM：

当网页被加载时，浏览器会创建页面的文档对象模型（Document Object Module）。![image](http://p1.pstatp.com/large/11100003da296ef22f61)

可以通过文档对象模型，修改元素的内容，以及属性的具体值。
> 2、DOM操作HTML：
- Javascript能够改变页面中的所有HTML元素。
- Javascript能够改变页面中的所有HTML属性。
- Javascript能够改变页面中的所有CSS样式。
- Javascript能够对页面中的所有事件做出反应。

##### 5.2：Javascript-DOM操作HTML

1、改变HTML输出流：
注意：绝对不要在文档加载完成之后使用document.write()。这会覆盖该文档。

2、寻找元素：

通过id找到HTML元素。

通过标签名找到HTML元素。

3、改变HTML内容：

使用属性：innerHTML。

4、改变HTML属性：

使用属性：attribute。

---


代码和运行结果说明：

下面的代码都是在HTML的body标签中演示的。
> 1：代码，
```
<p>Hello</p>
<p>Hello</p>
<p>Hello</p>
<button onclick="ondemo()">按钮</button>
<script>
function ondemo() {
document.write("World !");
}
</script>

```
运行结果：

![image](http://p3.pstatp.com/large/11100003e14e61d7ce54)

文档加载完成的时候页面显示的是三个p标签Hello，在执行document.write("World !")之后，输出的World!覆盖掉了原来的三个Hello。


> 2：通过id来通过id 来改变HTML元素


```
<p id="pid">Hello</p>
<p>Hello</p>
<p>Hello</p>
<button onclick="ondemo()">按钮</button>
<script>
function ondemo() {
document.getElementById("pid").innerHTML = "aaaaa";
}
</script>
```

给第一个p标签指定一个id，
相应运行结果是：
![image](http://p3.pstatp.com/large/110e00046d06f8a795de)

> 3：通过标签名找到HTML元素：


```
<p >Hello One</p>
<p>Hello Two</p>
<p>Hello Three</p>
<button onclick="ondemo()">按钮</button>
<script>
function ondemo() {
        document.getElementsByTagName("p").item(2).innerHTML = "aaaaa";
}
</script>
```

使用document.getElementsByTagName("p")来找到p标签，调用.item(2)方法来得到第三个p标签后赋值aaaaa。

运行结果：


![image](http://p2.pstatp.com/large/110e00046f62eda49140)

上面就是利用innerHTML 来改变标签的值的。

> 4：改变a标签超链接的地址


```
<a id="aid" href="http://www.baidu.com">百度</a>
<button onclick="ondemo()">按钮</button>
<script>
function ondemo() {
        document.getElementById("aid").href="http://www.apkbus.com";
}
</script>

```

运行结果：


![image](http://p3.pstatp.com/large/11100003e8aa2d2c3d06)

点点击按钮的时候把原来的百度网址换成了安卓巴士的网址，这就是改变了原来的a标签的HTML属性

##### 5.3：Javascript-DOM操作CSS

1、通过DOM对象改变CSS
语法：document.getElementById(id).style.property = new style

测试程序：通过DOM来操作CSS，之前我们已经用过，使用DOM来改变CSS的背景。


```
<!DOCTYPE html>
<html>
<head lang="en">
<meta charset="UTF-8">
<title></title>
<link href="style.css" rel="stylesheet" type="text/css">
</head>
<body>
    <div id="div">
        Hello
    </div>
    <button onclick="ondemo()">改变文字颜色和背景颜色</button>
    <script>
        function ondemo() {
            document.getElementById("div").style.backgroundColor = "red";
            document.getElementById("div").style.color = "white";
    }
    </script>
</body>
</html>

```
style.css文件：

```
#div{
    width: 100px;
    height: 100px;
    background-color: aqua;
}

```
运行结果：

![image](http://p3.pstatp.com/large/111100014d377a6d684e)

##### 5.4：Javascript-DOM EventListener


> 1、DOM EventListener:
- 方法：addEventListener():
- removeEventListener():


> 2、addEventListener():

方法用于向指定元素添加事件句柄

> 3、removeEventListener():

移出方法添加的事件句柄


测试程序：
```

<button id="btn">按钮</button>
<script>
document.getElementById("btn").addEventListener("click", hello());//添加句柄
document.getElementById("btn").addEventListener("click", world());
function hello() {
    alert("Hello!");
}
function world() {
    alert("world !");
}
</script>

```
运行结果：

![image](http://p3.pstatp.com/large/111200016b959b0a0aa8)


当移出第二个句柄的时候：
document.getElementById("btn").removeEventListener("click", world);

运行结果：

![image](http://p3.pstatp.com/large/111000040b66f01f3083
)

这里在测试的时候发现一个问题，在添加句柄的时候document.getElementById("btn").addEventListener("click", world())，我们后面调用的world方法有括号，导致添加句柄在运行就会弹出对话框，这里是不应该有括号的，如果加上括号，在移出句柄的时候也有括号，就会导致移出句柄失败，切记，切记。再添加句柄的时候不要有括号。

### 第六部分：JavaScript事件详解
##### 6.1、JS事件详解-事件流：


1、事件流：
描述的是在页面中接受事件的顺序。

2、事件冒泡：
由最具体的元素接收，然后逐级向上传播至最不具体的元素的节点（文档）。

3、事件捕获：
最不具体的节点先接收事件，而最具体的节点应该是最后接收事件。

##### 6.2、JS事件详解-事件处理：


1、HTML事件处理：
直接添加到HTML 结构中

2、DOM0(零)级事件处理：
把一个函数赋值给一个事件处理程序属性

3、DOM2级事件处理：
- addEventListener("事件名","事件处理函数","布尔值")；
- true：事件捕获
- false：事件冒泡
- removeEventListener();

4、IE事件处理程序：
- attachEvent
- detachEvent

测试程序：
```

<div id="div">
<button id="btn1">按钮</button>
</div>
<script>
var btn1 = document.getElementById("btn1");
if (btn1.addEventListener) {
    btn1.addEventListener("click", demo);
} else if (btn1.attachEvent) {
    btn1.attachEvent("onclick", demo);
} else {
    btn1.onclick = demo();
}
function demo() {
    alert("hello");
}
</script>

```
要注意的是一个是click一个事onclick
运行结果都是弹出hello对话框。

##### 6.3、JS事件详解-事件对象：


> 1、事件对象：
在触发DOM事件的时候都会产生一个对象。

> 2、事件对象Event：
- 1）：type：获取事件类型。
- 2）：target：获取事件目标。
- 3）：stopPropagation();阻止事件冒泡。
- 4）：preventDefault：阻止事件默认行为。

测试程序：


```
<div id="div">
<button id="btn1">按钮</button>
<a href="http://www.baidu.com" id="aid">百度</a>
</div>
<script>
    document.getElementById("btn1").addEventListener("click", showType);
    //可以添加这个鼠标移动的事件，效果和点击的事件一样
    // document.getElementById("btn1").addEventListener("mouseover", showType);
    document.getElementById("div").addEventListener("click", showDiv);
    document.getElementById("aid").addEventListener("click",showA);
    function showType(event) {
    alert(event.type);
    // event.stopPropagation();//阻止事件的冒泡
    }
    function showDiv(event) {
        alert(event.target);
    }
    function showA(event){
        event.stopPropagation();
        event.preventDefault();
    }
</script>
当没有阻止btn1事件冒泡的时候，执行结果是这样的：
```
![image](http://p1.pstatp.com/large/106b00054aaa11b5a611)

阻止btn1冒泡的时候运行结果是这样的：

![image](http://p3.pstatp.com/large/11130003ec51ca6e4c28)

测试btn1阻止了事件的冒泡，就不会传给div了。

同样的，在a标签事件中，阻止了a事件的冒泡，且阻止事件默认行为----打开百度网页，所以在点击百度的时候不会有任何反应，结果如下：

![image](http://p1.pstatp.com/large/111000059d5a928b949e)

总结：本节第五第六这两部分，应该算是javascript中比较重要且基础的部分，尤其是第六章javascript事件部分，感觉比较陌生，所以就按照课程，一 一的都写了代码，运行测试。

接下来将继续学习javascript的内置对象、浏览器对象等内容，如果小编的这些javascript基础教程对你有一丝的帮助，请多多评论、收藏、转发。本片文章到此结束，下篇文章再会。


欢迎关注我的个人技术公众号,快速查看我的最新文章。

![这里写图片描述](http://img.blog.csdn.net/20161220174646569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2NnXzIwMTIxNjMyMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

时间：2016-11-11