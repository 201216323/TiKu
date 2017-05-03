今天是周六，也没有干什么事情，原计划下午四五点去会所健身的，睡醒之后感觉特别的困，又累又饿，所以也没有出去健身，出去吃个晚饭，回来就继续javascript的学习。

-----2016年11月12日

![image](http://p1.pstatp.com/large/ef4001018e9ac5db9ed)


首先还是javascript的目录：

- 1、JavaScript基础教程
- 2、JavaScript语法详解
- 3、JavaScript函数
- 4、JavaScript异常处理和事件处理
- 5、JavaScript DOM对象
- 6、JavaScript事件详解
- 7、JavaScript内置对象
- 8、JavaScript DOM对象控制HTML元素详解
- 9、JavaScript浏览器对象
- 10、JavaScript面向对象详解
- 11、Javascript瀑布流
- 12、本节学习剩下的内容。


# 第七部分：JavaScript内置对象
## 7.1、JS内置对象-什么是对象

### 概要：
1. 什么是对象。
1. String字符串对象。
1. Date日期对象。
1. Array数组对象。
1. Math对象。


> 1、什么是对象：

javascript中的所有事物都是对象：字符串、数值、数组、函数……每个对象带有属性和方法。
> 2、自定义对象：

1）：定义并创建对象实例。

2）：使用函数来定义对象，然后创建新的对象实例。

> 测试程序：



```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>JS内置对象-什么是对象</title>
</head>
<body>
<!--创建对象-->
<script>
    people = new Object();
    people.name = "Bruce常";
    people.age = "24";
    document.write("name:" + people.name + ", age:" + people.age);
    // 另一种方式创建对象
    person = {name: "Bruce Chang", age: "25"};
    document.write("<br/>" + "name:" + person.name + ", age:" + person.age);
    //使用函数来创建对象
    function Son(name, age) {
        this.name = name;
        this.age = age;
    }
    son = new Son("Bruce Lee", 20);
    document.write("<br/>name:" + son.name + ",age" + son.age);
</script>
</body>
</html>
```
> 运行结果：

![image](http://p3.pstatp.com/large/e59000fb34fb0fca62c)

## 7.2、JS内置对象-String字符串对象

> 1、String对象：String对象用于处理已有的字符串。字符串可以使用单引号或者双引号。

> 2、在字符串中查找字符串：indexOf()

> 3、内容匹配：match()

> 4、替换内容：replace()

> 5、字符串大小写转换：toUpperCase()/toLowerCase                                        

>  6、字符串转为数组：String>>split()

> 测试程序：


```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>JS内置对象-String字符串对象</title>
</head>
<body>
<script>
    var str = "hello  world!";
    var str1 = "a,b,c,d,e,f,g";
    document.write("字符串长度(带空格的)："+str.length);
    document.write("<br/>索引字符串是否存在，输出的是所在位置，从0开始，没有返回-1"+str.indexOf("world"));
    document.write("<br/>内容匹配（有返回该字符串，没有返回null）"+str.match("world"));
    document.write("<br/>替换内容"+str.replace("hello","nihao"));
    document.write("<br/>大小写转换"+str.toUpperCase());
    var arr = str1.split(",");
    document.write("<br/>字符串转数组后输出："+arr[2]);
</script>
</body>
</html>
```

> 运行结果：

![image](http://p1.pstatp.com/large/ef4001018ea2e67315f)

> 1、字符串属性和方法：

1）属性：length，prototype，constructor。

2）方法：charAt()、charCodeAt()、concat()、fromCharCode()、indexOf()、lastIndexOf()、match()、replace()、search()、slice()、substring()、substr()、valueOf()、toLowerCase()、toUpperCase()、split()

因为小编有java的基础，上面提到的这些方法和属性在java中也大部分都有，毕竟都是语言，都需要处理一些相同的逻辑问题。


## 7.3、JS内置对象-Date日期对象
> 1、Date对象：
日期对象由于处理日期和时间。

> 2、获得当日的日期：
var date = new Date();
document.write(date);

> 3、常用方法：
getFullYear()：获取年份，
getTime()：获取毫秒，
setFullYear()：设置具体的日期，
getDay()：获取当前日期的日，

> 测试程序：



```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>JS内置对象-Date日期对象</title>
</head>
<body onload="startTime()">

<script>
    var date = new Date();
    document.write("<p></p>当前日期：" + date);
    document.write("<p></p>获取年份getFullYear：" + date.getFullYear());
    document.write("<p></p>获取时间毫秒数：" + date.getTime());
    date.setFullYear(1994, 03, 11);
    document.write("<p></p>当前日期：" + date);
    document.write("<p></p>获取日期的日getDate：" + date.getDate());

    function startTime() {
        var today = new Date();
        var h = today.getHours();
        var m = today.getMinutes();
        var s = today.getSeconds();
        m = checkTime(m);
        s = checkTime(s);
        document.getElementById("timetxt").innerHTML = "<p></p>当前时间：" + h + ":" + m + ":" + s;
        t = setTimeout(function () {
            //1000毫秒调一次startTime方法
            startTime();
        }, 1000);
    }
    function checkTime(i) {
        if (i < 10) {
            i = "0" + i;
        }
        return i;
    }
</script>

<div id="timetxt"></div>
</body>
</html>
```
> 运行结果：

![image](http://p1.pstatp.com/large/ef5000fd8fdd2f179a6)


## 7.4、JS内置对象-Array数组对象


> 1、Array对象：
使用单独的变量名来存储一系列的值。

> 2、数组的创建：
例如：var myArray = [“A”，“B”，“C”];

> 3、数组的访问：
通过指定数组名以及索引号码，你可以访问某个特定的元素。
注意：【0】是数组的第一个元素，【1】是数组的第二个元素。

> 4、数组常用方法：
concat()：合并数组
sort()：排序
push()：末尾追加元素
reverse()：数组元素翻转

> 测试程序：


```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>JS内置对象-Array数组对象</title>
</head>
<body>
<p id="p1">p1</p>
<p id="p2">p1</p>
<p id="p3">p3</p>
<p id="p4">p4</p>
<script>
    var a = ["1", "2", "3"];
    var b = ["4", "5", "6"];
    var c = a.concat(b);
    document.getElementById("p1").innerHTML = "合并数组:" + c;
    document.getElementById("p2").innerHTML = "排序:" + c.sort(function (a, b) {
        return b-a;//a-b是正序，b-a是倒序
    });
    c.push("7")
    document.getElementById("p3").innerHTML = "末尾追加元素hou正序:" + c.sort();
    document.getElementById("p4").innerHTML = "数组元素翻转:" + c.reverse();
</script>
</body>
</html>
```
![image](http://p1.pstatp.com/large/ef3001017923998956a)

## 7.5、JS内置对象-Math对象


> 1、Math对象：
执行常见的算数任务。

> 2、常用方法：
round()：四舍五入
random()：返回0~1之间的随机数
max()：返回最高值
min()：返回中的最低值
abs()：返回绝对值

> 测试程序：


```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>JS内置对象-Math对象</title>
</head>
<body>
<p id="p1">p1</p>
<p id="p2">p1</p>
<p id="p3">p3</p>
<p id="p4">p4</p>
<p id="p5">p5</p>
<script>
    document.getElementById("p1").innerHTML="2.49四舍五入："+Math.round(2.49)+"<br/>2.51四舍五入："+Math.round(2.51);
    document.getElementById("p2").innerHTML="返回0~1之间的随机数："+Math.random();
    document.getElementById("p3").innerHTML="返回最高值："+Math.max(10,11,22,44,65,435);
    document.getElementById("p4").innerHTML="返回最低值："+Math.max(10,11,22,44,65,435);
    document.getElementById("p5").innerHTML="返回绝对值："+Math.abs(-5);
</script>
</body>
</html>
```
> 运行结果：

![image](http://p3.pstatp.com/large/e59000fb350d5204378)


第七部分中这些内置对象，就像是java中自带的类一样，每一个类都有相应的属性和方法，以后在实际中希望多多用一下。

# 第八部分：JavaScript DOM对象控制HTML元素详解
## 8.1、JSDOM对象控制HTML元素详解-1

![image](http://p1.pstatp.com/large/e59000fb351712eeb52)

offsetHeight不包含滚动条的高度，scrollHeight包含滚动条的高度。

> 测试程序：


```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>JSDOM对象控制HTML元素详解-1</title>
</head>
<body>
<p name="pn">hello</p>
<p name="pn">hello</p>
<p name="pn">hello</p>
<p name="pn">hello</p>
<p name="pn">hello</p>
<a id="aid" title="得到了a标签的属性">hello</a>
<ul><li>1</li><li>2</li><li>3</li></ul>
<script>
    function getName() {
//        这里getElementsByTagName和getElementsByName效果一样
//        var count = document.getElementsByTagName("p");
        var count = document.getElementsByName("pn");
//        alert(count.length);长度是4
        var p = count[0];
        p.innerHTML = "World";
    }
//        getName();    //方法1：
    function getAttr() {
        var anode = document.getElementById("aid");
        var attr = anode.getAttribute("title");
        alert(attr);
    }
//       getAttr();    // 方法2

    function setAttr() {
        var anode = document.getElementById("aid");
        anode.setAttribute("title", "动态设置a的title属性");
        var attr = anode.getAttribute("title");
        alert(attr);
    }
//        setAttr();    //方法3

    function getChildNode() {
        var childnode = document.getElementsByTagName("ul")[0].childNodes;
//        alert(childnode.length)
        alert(childnode[1].nodeType);
    }
    getChildNode();   //方法4
</script>
</body>
</html>
```
> 运行结果：

1：只执行方法1的时候：  第一行的hello变成了World。

![image](http://p1.pstatp.com/large/ef7000fc15a87698518)

2：只执行方法2的时候：

![image](http://p1.pstatp.com/large/e59000fb352e15bcea5)

3：只执行方法3的时候：

![image](http://p1.pstatp.com/large/ef5000fd8fe4b1216dc)

4：只执行方法4的时候：

![image](http://p3.pstatp.com/large/ef4001018eb5c116152)

这里不明白节点类型是什么意思，百度一下，
- 元素节点的 nodeType 属性值是 1。
- 属性节点的 nodeType 属性值是 2。
- 文本节点的 nodeType 属性值是 3。


## 8.2、JSDOM对象控制HTML元素详解-2
> 测试程序：


```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>JSDOM对象控制HTML元素详解-2</title>
</head>
<body>
<div id="div">
    <p id="pid">div 的p元素</p>
</div>
<p>hahah</p>
<p>hahah</p>
<p>hahah</p>
<p>hahah</p>
<script>
    //访问父节点。
    function getParentNode() {
        var div = document.getElementById("pid");
        alert(div.parentNode.nodeName);
    }
//        getParentNode();  //方法1

    //创建元素节点。
    function creatNode() {
        var body = document.body;
        var input = document.createElement("input");
        input.type = "button";
        input.value = "按钮";
        body.appendChild(input);
    }
//        creatNode();      //方法2

    //插入节点
    function addNode() {
        var div = document.getElementById("div");
        var node = document.getElementById("pid");
        var newnode = document.createElement("p");
        newnode.innerHTML = "动态添加的第一个p元素";
        div.insertBefore(newnode, node);
    }
//        addNode();        //方法3

    //删除节点
    function removeNode() {
        var div = document.getElementById("div");
        var node = document.getElementById("pid");
        div.removeChild(node);
    }
//        removeNode();     //方法4

    //网页尺寸
    function getSize() {
        var offsetHeight = document.body.offsetHeight;
        //也可以这样写，为了兼容
        var offsetWidth = document.body.offsetWidth || document.documentElement.offsetWidth;
        alert("宽：" + offsetWidth + "高" + offsetHeight);
    }
    getSize();        //方法5
</script>
</body>
</html>
```

> 运行结果：

只执行方法1的时候：

![image](http://p3.pstatp.com/large/ef5000fd8ff17b3b88b)

只执行方法2的时候：

![image](http://p3.pstatp.com/large/ef600101fe0455249c8)

只执行方法3的时候：

![image](http://p3.pstatp.com/large/e59000fb3531a99d103)


只执行方法4的时候：

![image](http://p3.pstatp.com/large/ef7000fc15b86e12869)

只执行方法5的时候：

![image](http://p3.pstatp.com/large/ef300101793360e8436)

> 小结：

javascript是web 前端的一门重要的技术，所以小编在学习和写下这篇文章的时候，尽可能的详细，让基础不好的人看到我写的文章就知道相应的程序结果，这也是我粘贴大量程序运行结果的原因，同时也方便以后我的复习。


# 第九部分：JavaScript浏览器对象
## 9.1、JS浏览器对象-window对象

1. Window对象
1. 计数器
1. History对象
1. Location对象
1. Screen对象
1. Navigator对象
1. 弹出窗口
1. Cookies

> 1、window对象：

- window对象是BOM的核心，window对象指当前的浏览器窗口，所有的Javascript全局对象、函数以及变量均自动成为window对象的成员。
- 全局变量是window对象的属性。
- 全局函数是window对象的方法。
- 甚至HTML DOM的document也是window对象的属性之一。

> 2、window尺寸：
- window.innerHeight - 浏览器窗口的内部高度。
- window.innerWidth - 浏览器窗口的内部宽度。
> 3、window方法：
- window.open() - 打开新窗口
- window.close() - 关闭当前窗口

> 测试程序得到窗口内部的宽高：

![image](http://p1.pstatp.com/large/ef40010703fb6a113c6)

运行结果得到浏览器的宽和高：

![image](http://p3.pstatp.com/large/ef7001018f7674b6210)

> 测试程序打开窗口和关闭窗口：

![image](http://p3.pstatp.com/large/ef7001018f850413f5a)


> obindex.html文件：

![image](http://p1.pstatp.com/large/ef400107041f126e209)


> 运行结果：

![image](http://p3.pstatp.com/large/ef300106ebf821c88fa)



## 9.2、JS浏览器对象-计时器
### 1、计时事件：
通过使用javascript，我们有能力做到在一个设定的时间间隔之后来执行代码，而不是在 函数被调用后立即执行，我们称之为计时事件。
### 2、计时方法：
1）：setInterval() - 间隔指定的毫秒数，不停的执行指定的代码。
clearInterval() 方法用于停止setInterval()方法执行的代码。
2）：setTimeout() - 暂停指定的毫秒数后执行指定的代码。
clearTimeout() 方法用于停止执行setTimeout()方法的函数代码。

> 测试setInterval()方法代码：

![image](http://p3.pstatp.com/large/ef7001018f96a4e0fe8)

> 运行结果是：

![image](http://p3.pstatp.com/large/e5900100ab759b2787b)

当点击停止放的时候，调用clearInterval(mytime)方法，停止执行setInterval()方法。

> 测试setTimeout()方法代码：

![image](http://p1.pstatp.com/large/ef7001018faab73fe9e)


> 运行结果：

![image](http://p1.pstatp.com/large/ef40010704297fa8087)

## 9.3、JS浏览器对象-History对象
### 1、History对象：
window.history对象包含浏览器的历史（url）的集合。
### 2、History方法：
- history.back() - 与在浏览器点击回退按钮相同。
- history.forward() - 与在浏览器中点击按钮向前相同。
- history.go() - 进入历史中的某个页面。
> 测试程序：


![image](http://p3.pstatp.com/large/ef40010ba17c363327f)

> 运行结果：

![image](http://p1.pstatp.com/large/ef6001077d908bc744c)

## 9.4、JS浏览器对象-Location对象
### 1、Location对象：
window.location 对象用于获得当前页面的地址（url），并把浏览器重定向到新的页面。
### 2、Location对象的属性。
- location.hostname返回web主机的域名。
- location.pathname返回当前页面的路径和文件名。
- location.port返回web主机的端口
- location.protocol返回所使用的web协议（http://或者https::///）
- location.href属性返回当前页面的url。
- location.assign()方法加载新的文档。
> 测试程序：

![image](http://p3.pstatp.com/large/ef7001018fbbc9a283d)


> 运行结果：

![image](http://p1.pstatp.com/large/ef7001018fcf0fdba8c)

## 9.5、JS浏览器对象-Screen对象
### 1、Screen对象：
window.screen对象包含有关用户屏幕的信息。
### 2、属性：
- screen.availWidth - 可用的屏幕宽度。
- screen.availHeight - 可用的屏幕高度。
- screen.Height - 屏幕高度。
- screen.Width - 屏幕宽度。
> 测试程序：

![image](http://p3.pstatp.com/large/ef40010704304b202d1)


> 运行结果：

![image](http://p3.pstatp.com/large/ef6001077da97fcc533)


# 第十部份、JavaScript面向对象详解
## 10.1、JS面向对象-认识面向对象
> 课程概要：
1、认识面向对象
2、基本面向对象
3、函数构造器构造对象
4、深入Javascript面向对象

1、面向对象的概念

- 1）：一切事物姐对象。
- 2）：对象具有封装和继承特性。
- 3）：信息隐藏。

> 简单的测试程序：
Literal.js文件：

![image](http://p3.pstatp.com/large/ef4001070442e17b9e6)

> 在index.html 中引入此js文件

![image](http://p3.pstatp.com/large/ef500103127623eb23c)

执行结果是输出age 30。

![image](http://p2.pstatp.com/large/ef50010312889ee957d)


## 10.2、JS面向对象-JS面向对象(1)
> 测试程序：
app1.js代码

![image](http://p1.pstatp.com/large/ef7001018fd72b692eb)

> 在index.html中调用：

![image](http://p3.pstatp.com/large/ef4001070451e05b23b)

> 运行结果：

![image](http://p1.pstatp.com/large/ef6001077db07bd694a)

## 10.3、JS面向对象-JS面向对象(2)
> 测试程序：
app2.js文件：

![image](http://p1.pstatp.com/large/ef4001070467f9fbfee)


![image](http://p1.pstatp.com/large/ef50010312921ea5277)


> 运行结果：

![image](http://p3.pstatp.com/large/ef7001062dffb8b6c83)


> 小结：

总结：面向对象这块，通过今天的学习感觉还不是非常的明白，虽然我学过java，以后多多加强这方面的知识。


# 第十一部分：Javascript瀑布流
## 11.1、JS瀑布流效果-布局：
> 测试程序：
index.html代码：


```
<!DOCTYPE html>
<html>
<head lang="en">
<meta charset="UTF-8">
<title>瀑布流</title>
<link href="Style.css" type="text/css" rel="stylesheet">
</head>
<body>
    <div id="container">
        <div class="box">
            <div class="box_img">
                <img src="img/1.jpg">
            </div>
        </div>
        <div class="box">
            <div class="box_img">
                <img src="img/2.jpg">
            </div>
        </div>
        <div class="box">
            <div class="box_img">
                <img src="img/3.jpg">
            </div>
        </div>
        <div class="box">
            <div class="box_img">
                <img src="img/4.jpg">
            </div>
        </div>
        <div class="box">
            <div class="box_img">
                <img src="img/5.jpg">
            </div>
        </div>
        <div class="box">
            <div class="box_img">
                <img src="img/6.jpg">
            </div>
        </div>
        <div class="box">
            <div class="box_img">
                <img src="img/7.jpg">
            </div>
        </div>
        <div class="box">
            <div class="box_img">
                <img src="img/1.jpg">
            </div>
        </div>
        <div class="box">
            <div class="box_img">
                <img src="img/8.jpg">
            </div>
        </div>
        <div class="box">
            <div class="box_img">
                <img src="img/10.jpg">
            </div>
        </div>
    </div>
</body>
</html>
```
> Style.css代码：


```
*{
    margin: 0px;
    padding: 0px;
}
#container{
    position: relative;
}
.box{
    padding: 5px;
    float: left;
}
.box_img{
    padding: 5px;
    border-width: 1px;
    border-style: solid;
    border-color: #ccc;
    border-radius: 5px;
    background-color: #cccccc;
}
.box_img img{
    width: 150px;
    height: auto;
}
```
> 运行结果：

![image](http://p3.pstatp.com/large/111100073d41dcb3d2f7)

这里面，有十张图片，css文件中最外层的div使用的是相对布局，所以在浏览器窗口发生变化的时候，图片的位置也会相对的发生变化。

## 11.2、JS瀑布流效果-1：
接下来在上面的基础上，根据图片的宽度与浏览器的宽度，来动态的确定显示的列数，并将剩下的图片依次排在高度最低的位置下面，实现瀑布流的效果。
> 测试程序：
Style.css里面的代码不变，
index.html里面多放入点数据，并添加引用app.js文件。


```
<!DOCTYPE html>
<html>
<head lang="en">
<meta charset="UTF-8">
<title>瀑布流</title>
<link href="Style.css" type="text/css" rel="stylesheet">
<script src="js/app.js"></script>
</head>
<body>
    <div id="container">
    <div class="box">
    <div class="box_img">
    <img src="img/1.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/2.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/3.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/4.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/5.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/6.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/7.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/1.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/8.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/10.jpg">
    </div>
    </div>
    </div>
</body>
</html>
```
> app.js代码：

```
/**
* Created by Administrator on 2016/11/14.
*/
window.onload = function () {
    imgLocation("container", "box");
}
function imgLocation(parent, content) {
        //将parent下所有的content全部取出来
        //cparent是container
        var cparent = document.getElementById(parent);
        var ccontent = getChildElement(cparent, content);
        //取出第一条图片的宽
        var imgWidth = ccontent[0].offsetWidth;
        //得到浏览器的宽，宽屏下，小编的宽是1349
        var clientWidth = document.documentElement.clientWidth;
        //浏览器的宽度除以图片的宽，得到显示的列数
        var num = Math.floor(clientWidth / imgWidth);
        //动态的设置style的宽，左右居中，上下自适应
        cparent.style.cssText = "width:" + imgWidth * num + "px;margin: 0px auto";
        var BoxHeightArr = [];
        for (var i = 0; i < ccontent.length; i++) {
        if (i < num) {
            BoxHeightArr[i] = ccontent[i].offsetHeight;
            //console.log(BoxHeightArr[i]);
        } else {
            //取出BoxHeightArr中最小高度
            var minHeight = Math.min.apply(null, BoxHeightArr);
            //得到BoxHeightArr数组中，最小高度所在的位置
            var minIndex = getminheightLocation(BoxHeightArr, minHeight);
            //设置相应的style，包括句对位置，距离上和左边的距离
            ccontent[i].style.position = "absolute";
            ccontent[i].style.top = minHeight + "px";
            ccontent[i].style.left = ccontent[minIndex].offsetLeft + "px";
            //此时的高度 = 最小高度+图片的高度
            BoxHeightArr[minIndex] = BoxHeightArr[minIndex] + ccontent[i].offsetHeight;
        }
    }
}
//取出BoxHeightArr数组中最小的高度
function getminheightLocation(BoxHeightArr, minHeight) {
    for (var i in BoxHeightArr) {
        if (BoxHeightArr[i] == minHeight) {
            return i;
        }
    }
}
// 得到container 这个父布局中类名是 box的div，返回值是一个数组。
function getChildElement(parent, content) {
    var contentArr = [];
    //取出container中className为box的标签，为一个数组 ，allContent的大小是10
    var allContent = parent.getElementsByClassName(content);
    for (var i = 0; i < allContent.length; i++) {
    if (allContent[i].className = content) {
        contentArr.push(allContent[i]);
    }
    }
    //contentArr的大小也是10
    return contentArr;
}
```

> 运行结果2是：

![image](http://p3.pstatp.com/large/111100073f1926cc42fa)


可以看到，浏览器的宽度已经固定了，当我们缩放浏览器的时候，也不会出现"运行结果1"中的效果，而且，多出来的三张图片已经自动排列在最低的高度下面，实现瀑布流的效果，接下来可以多添加的数据，查看运行结果。
> index.html中添加大量的数据后（见文章最后）：

> 运行结果3是：

![image](http://p3.pstatp.com/large/119a0004ddb725e71be8)

到此一个类似瀑布流的效果就已经完成了，接下来会增加无限上拉加载更多的功能。

## 11.3：3、JS瀑布流效果-2：
> 此时的app.js文件是：


```
/**
 * Created by Administrator on 2016/11/14.
 */
window.onload = function () {
    imgLocation("container", "box");
    //数据源
    var imgData = {"data": [{"src": "2.jpg"}, {"src": "4.jpg"}, {"src": "5.jpg"}, {"src": "6.jpg"}, {"src": "8.jpg"}]};
    window.onscroll = function () {

        if (checkFlag()) {
            //动态的creat相应的元素，来实现加载更多图片的功能
            var cparent = document.getElementById("container");
            for (var i = 0; i < imgData.data.length; i++) {
                var ccontent = document.createElement("div");
                ccontent.className = "box";
                cparent.appendChild(ccontent);
                var box_img = document.createElement("div");
                box_img.className = "box_img";
                ccontent.appendChild(box_img);
                var img = document.createElement("img");
                img.src = "img/" + imgData.data[i].src;
                box_img.appendChild(img);
            }
            //此时运行在滑动的时候会有重叠的效果，重新调一下imgLocation方法。
            imgLocation("container", "box");
        }

    }
}

//判断是否可以加载
function checkFlag() {
    var cparent = document.getElementById("container");
    var ccontent = getChildElement(cparent, "box");
    var lastContentHeight = ccontent[ccontent.length - 1].offsetTop;
    console.log(lastContentHeight);
    //未显示被隐藏掉的高度
    var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
    //console.log(scrollTop);
    //当前页面的高度
    var pageHeight = document.documentElement.clientHeight || document.body.clientHeight;
    console.log("最后一条数据的高度：" + lastContentHeight + "未显示的高度：" + scrollTop + "当前页面的高度：" + pageHeight);
    if (lastContentHeight < scrollTop + pageHeight) {
        //当总共的高度<未显示的高度+当前页面的高度的时候,返回true允许加载
        return true;
    }
}

function imgLocation(parent, content) {
    //将parent下所有的content全部取出来
    //cparent是container
    var cparent = document.getElementById(parent);
    var ccontent = getChildElement(cparent, content);
    //取出第一条图片的宽
    var imgWidth = ccontent[0].offsetWidth;
    //得到浏览器的宽，宽屏下，小编的宽是1349
    var clientWidth = document.documentElement.clientWidth;
    //浏览器的宽度除以图片的宽，得到显示的列数
    var num = Math.floor(clientWidth / imgWidth);
    //动态的设置style的宽，左右居中，上下自适应
    cparent.style.cssText = "width:" + imgWidth * num + "px;margin: 0px auto";

    var BoxHeightArr = [];
    for (var i = 0; i < ccontent.length; i++) {
        if (i < num) {
            BoxHeightArr[i] = ccontent[i].offsetHeight;
            //console.log(BoxHeightArr[i]);
        } else {
            //取出BoxHeightArr中最小高度
            var minHeight = Math.min.apply(null, BoxHeightArr);
            //得到BoxHeightArr数组中，最小高度所在的位置
            var minIndex = getminheightLocation(BoxHeightArr, minHeight);
            //设置相应的style，包括句对位置，距离上和左边的距离
            ccontent[i].style.position = "absolute";
            ccontent[i].style.top = minHeight + "px";
            ccontent[i].style.left = ccontent[minIndex].offsetLeft + "px";
            //此时的高度 = 最小高度+图片的高度
            BoxHeightArr[minIndex] = BoxHeightArr[minIndex] + ccontent[i].offsetHeight;
        }
    }
}

//取出BoxHeightArr数组中最小的高度
function getminheightLocation(BoxHeightArr, minHeight) {
    for (var i in BoxHeightArr) {
        if (BoxHeightArr[i] == minHeight) {
            return i;
        }
    }
}


// 得到container 这个父布局中类名是 box的div，返回值是一个数组。
function getChildElement(parent, content) {
    var contentArr = [];
    //取出container中className为box的标签，为一个数组 ，allContent的大小是10
    var allContent = parent.getElementsByClassName(content);
    for (var i = 0; i < allContent.length; i++) {
        if (allContent[i].className = content) {
            contentArr.push(allContent[i]);
        }
    }
    //contentArr的大小也是10
    return contentArr;
}
```

上面，新增一个判断是否可以加载的方法checkFlag，当返回true的时候我们动态的creat相应的标签，来加载更多的图片，但最后还需要重新对这些图片定位，所以最后在重新调用一下imgLocation方法，就可以实现图片无限加载更多的功能了。
> 最终的运行结果：

![image](http://p3.pstatp.com/large/111200063a0f2301a77d)

当鼠标不行向上滑动的时候，图片会无限加载，当然加载的都是本地var imgData = {"data": [{"src": "2.jpg"}, {"src": "4.jpg"}, {"src": "5.jpg"}, {"src": "6.jpg"}, {"src": "8.jpg"}]}这里面的图片。

> 总结：

小编是做Android的，但是感觉js的部分还是比较难的，以后小编也会多多在这方面下功夫，本次的瀑布流效果总体来说还是不错的，如果能够加载网络资源那就完美了。



欢迎关注我的个人技术公众号,快速查看我的最新文章。

![这里写图片描述](http://img.blog.csdn.net/20161220174646569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2NnXzIwMTIxNjMyMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


时间：2016-11-12 23:38



> index.html中添加大量的数据后

```
<!DOCTYPE html>
<html>
<head lang="en">
<meta charset="UTF-8">
<title>瀑布流</title>
<link href="Style.css" type="text/css" rel="stylesheet">
<script src="js/app.js"></script>
</head>
<body>
    <div id="container">
    <div class="box">
    <div class="box_img">
    <img src="img/1.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/2.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/3.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/4.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/5.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/6.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/7.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/1.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/8.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/10.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/1.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/2.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/3.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/4.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/5.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/6.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/7.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/1.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/8.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/10.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/1.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/2.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/3.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/4.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/5.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/6.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/7.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/1.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/8.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/10.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/1.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/2.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/3.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/4.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/5.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/6.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/7.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/1.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/8.jpg">
    </div>
    </div>
    <div class="box">
    <div class="box_img">
    <img src="img/10.jpg">
    </div>
    </div>
    </div>
</body>
</html>
```
