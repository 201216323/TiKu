# MVP设计模式

==曾经工作闲的的时候看过mvp，当时迷迷糊糊的，今天就重新来细致的捋一下。==

MVP：Model  View  Presenter

- Model:处理业务逻辑，获得数据
- View：视图，负责加载数据
- Presenter：中间者，（绑定Model和View）

View 持有Presenter，因为要在View（也即Activity）中new Presenter对象

Presenter持有Model和View 的接口类型，

## 看下图，比较MVC和MVP。

![image](http://wx3.sinaimg.cn/mw690/b0d9a523ly1fcuy5ds4z6j20i10crtgl.jpg)

如图片中所说的：

- 在MVP中，Model并不会和View直接通信，Presenter将充当中间人的角色。
- 在MVC中，当Model被Controller更新后，会直接通知View并更新显示。



项目中有两个Module,mvp和mvpyouhua,mvp这个程序是对mvp模式的一个简单的使用，但
这里面涉及到一个内存泄漏的问题。**看下面
**

View（Activity）  --->>    presenter  --->>  Model

Model访问网络获得了数据，执行onComplete方法，如果此时view被销毁了，例如：Activity被finish了，此时model访问view的引用，但view可能已经被回收了，会造成内存泄漏的问题。

为解决上个问题，就有了mvpyouhua这个程序，可以避免内存泄漏，同时通过使用BasePresenter，达到一个封装的目的。

**不懂想要学习的可以参考我的这篇文章。**



1. 欢迎关注我的个人技术公众号,快速查看我的最新文章。
2. 欢迎访问：201216323.tech 查看我的博客。
3. 源程序地址：[点我点我](https://github.com/201216323/TestMVP)

![这里写图片描述](http://img.blog.csdn.net/20161220174646569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2NnXzIwMTIxNjMyMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
