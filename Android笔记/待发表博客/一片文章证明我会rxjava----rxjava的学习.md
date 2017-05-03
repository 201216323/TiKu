经常浏览关于android的网站的同行们就会发现，现在越来越多的项目都用到了**RxJava**技术，为了提高自己的技术，我也搜索了一下RxJava，但，RaJava上手稍微难点，逻辑性非常的强，不过一旦学习这个了就会发现它的妙处。所以写份笔记，记录一下这个学习过程，本篇文章主要结合其它技术来说明RxJava的使用方法。**（文章比较长，感兴趣的要收藏起来学习哦！）**

> 学习这篇文章前你需要会的基础有以下几点：

1. 会使用Retrofit，结合OkHtttp来做网络访问
2. 理解观察者模式的概念，毕竟RxJava是基于观察者模式的


# 什么是RxJava

RxJava 是一个响应式编程框架，采用观察者设计模式。支持Java 6+和Android 2.3+，可以使用Java 8中的Java lambda表达式。

RxJava 是一个开源项目，地址：https://github.com/ReactiveX/RxJava

RxJava在Github上的解释是：

```
RxJava is a Java VM implementation of Reactive Extensions:

a library for composing asynchronous and event-based programs by using observable sequences.

It extends the observer pattern to support sequences of data/events and adds operators that allow you to compose sequences together declaratively while abstracting away concerns about things like low-level threading, synchronization, thread-safety and concurrent data structures.
```
大概就是说RxJava是Java VM上一个灵活的、使用可观测序列来组成的一个异步的、基于事件的库。扩展了观察者模式等等…………

还有好多的解释，都是English，大致浏览看一下知道什么意思就可以了。

# RxJava基础

RxJava在使用时候的最大作用就是**异步**,其核心的两个东西是

```
1. Observables（被观察者，事件源）

2. Observer/Subscriber（观察者）
```
Observables可以发出一系列的 事件，这里的事件可以是任何东西，例如网络请求、复杂计算处理、数据库操作、文件操作等等，事件执行结束后交给 Observer/Subscriber 的回调处理。


一个Observable可以发出零个或者多个事件，直到结束或者出错。每发出一个事件，就会调用它的Subscriber的onNext方法，最后调用Subscriber.onCompleted()或者Subscriber.onError()结束。


Rxjava的起来虽然很像设计模式中的观察者模式，但是有一点明显不同，那就是如果一个Observerble没有任何的的Subscriber，那么这个Observable是不会发出任何事件的。

RxJava是一种响应式编程，顾名思义，就是“你变化，我响应”。举个栗子，a = b + c; 这句代码将b+c的值赋给a，而之后如果b和c的值改变了不会影响到a，然而，对于响应式编程，之后b和c的值的改变也动态影响着a，意味着a会随着b和c的变化而变化。

响应式编程的组成为Observable/Operator/Subscriber，RxJava在响应式编程中的基本流程如下：


```
Observable -> Operator 1 -> Operator 2 -> Operator 3 -> Subscriber
```

1. Observable 发出一系列事件，他是事件的产生者；
2. Subscriber 负责处理事件，他是事件的消费者；
3. Operator 是对 Observable 发出的事件进行修改和变换；
4. 若事件从产生到消费不需要其他处理，则可以省略掉中间的 Operator，从而流程变为 Obsevable -> Subscriber；
5. Subscriber 通常在主线程执行，所以原则上不要去处理太多的事务，而这些复杂的事务处理则交给 Operator


> 从上面可以看出：

RxJava的优势可以概括为四个字，那就是 逻辑简洁。然而，逻辑简洁并不意味着代码简洁，但是，由于链式结构，一条龙，你可以从头到尾，从上到下，很清楚的看到这个连式结构的执行顺序。对于开发人员来说，代码质量并不在于代码量，而在于逻辑是否清晰简洁，可维护性如何，代码是否健壮！


# RxJava依赖

在 Android Studio 项目下，为 module 增加 Gradle 依赖。
```
compile 'io.reactivex:rxandroid:1.1.0'
compile 'io.reactivex:rxjava:1.1.5'
```

写此篇文章时候的版本如下：随后解释为什么不用下面的版本。


```
compile 'io.reactivex.rxjava2:rxjava:2.0.6'
compile 'io.reactivex.rxjava2:rxandroid:2.0.1'

```


> 学习rxjava，为什么还导入了rxandroid的库，有没有人和我有同样的疑问？？

android是用java语言，但安卓主体部分还是要android的类的，这就是需要rxandroid的原因。

与RxJava相比较，rxandroid主要增加了如下的几个java文件：

1. AndroidSchedulers
2. BuildConfig
3. HandlerScheduler
4. MainThreadSubscription
5. RxAndroidPlugins：
6. RxAndroidSchedulersHook：

对于上面6个类可以参考[此链接](http://www.bkjia.com/Androidjc/1080660.html)，有详细的解释。

# RxJava 入门

前面讲了那么多，大家在概念上对RxJava有一个初步的认识就好，接下来，将为您解开RxJava神秘的面纱。



无需过分纠结于“事件”这个词，暂时可以简单的把“事件”看成是一个值，或者一个对象。

1. 事件产生，就是构造要传递的对象；
2. 事件处理变换，就是改变传递的对象，可以改变对象的值，或是干脆创建个新对象，新对象类型也可以与源对象不一样；
3. 事件处理，就是接收到对象后要做的事；


> 事件产生

RxJava创建一个事件比较简单，由 Observable 通过 create 操作符来创建。举个栗子，还是经典的 HelloWorld~~


```
//创建一个Observable(被观察者)
    Observable<String> observable = Observable.create(new Observable.OnSubscribe<String>() {
        @Override
        public void call(Subscriber<? super String> subscriber) {
            // 发送一个 Hello World 事件
            subscriber.onNext("Hello World!");

            // 事件发送完成
            subscriber.onCompleted();
        }
    });
```


这段代码可以理解为， Observable 发出了一个类型为 String ，值为 “Hello World!” 的事件，仅此而已。


上面这段代码，也可以通过just操作符进行简化。RxJava常用操作符后面会详细介绍，这里先有个了解。


```
// 创建对象,just里面的每一个参数，相当于调用一次Subscriber#OnNext()
Observable<String> observable = Observable.just("Hello World!");
```

这样，是不是简单了许多？

> 事件消费

有事件产生，自然也要有事件消费。RxJava 可以通过 subscribe 操作符，对上述事件进行消费。首先，先创建一个观察者。


```
// 创建一个Observer
    Observer<String> observer = new Observer<String>() {
        @Override
        public void onCompleted() {
            Log.e(TAG, "complete");
        }

        @Override
        public void onError(Throwable e) {

        }

        @Override
        public void onNext(String s) {
            Log.e(TAG, s);
        }
    };
```
或者：


```
// 创建一个Subscriber
Subscriber<String> subscriber = new Subscriber<String>() {
    @Override
    public void onCompleted() {
        Log.i(TAG, "complete");
    }

    @Override
    public void onError(Throwable e) {

    }

    @Override
    public void onNext(String s) {
        Log.i(TAG, s);
    }
};
```

在 Subscriber 实现的三个方法中，顾名思义，对应三种不同状态： 
1. onComplete(): 事件全部处理完成后回调 
2. onError(Throwable t): 事件处理异常回调 
3. onNext(T t): 每接收到一个事件，回调一次

对于 Subscriber 来说，通常onNext()可以多次调用，最后调用onCompleted()表示事件发送完成。

1. Observer 是观察者， Subscriber 也是观察者，Subscriber 是一个实现了Observer接口的抽象类，对 Observer 进行了部分扩展，在使用上基本没有区别；
2. Subscriber 多了发送之前调用的 onStart() 和解除订阅关系的 unsubscribe() 方法。
3. 并且，在 RxJava 的 subscribe 过程中，Observer 也总是会先被转换成一个 Subscriber 再使用。所以在这之后的示例代码，都使用 Subscriber 来作为观察者。


> 事件订阅

最后，我们可以调用 subscribe 操作符， 进行事件订阅。

```
// 订阅事件
observable.subscribe(subscriber);
```

> 输出结果（日志信息）

```
E/MainActivity: Hello World!
E/MainActivity: complete
```

成功输出相应信息，由于没有遇到错误，所以onError()方法是不会执行的。

> 区分回调动作

对于事件消费与事件订阅来说，好像为了打印一个“Hello World！”要费好大的劲… 其实，RxJava 自身提供了精简回调方式，我们可以为 Subscriber 中的三种状态根据自身需要分别创建一个回调动作 Action：


```
// onComplete()
    Action0 onCompleteAction = new Action0() {
        @Override
        public void call() {
            Log.e(TAG, "complete");
        }
    };

    // onNext(T t)
    Action1<String> onNextAction = new Action1<String>() {
        @Override
        public void call(String s) {
            Log.e(TAG, s);
        }
    };

    // onError(Throwable t)
    Action1<Throwable> onErrorAction = new Action1<Throwable>() {
        @Override
        public void call(Throwable throwable) {

        }
    };
```

那么，RxJava 的事件订阅支持以下三种不完整定义的回调。


```
1：observable.subscribe(onNextAction);

2：observable.subscribe(onNextAction, onErrorAction);
  //注意顺序
3：observable.subscribe(onNextAction, onErrorAction, onCompleteAction);
```
我们可以根据当前需要，传入对应的 Action， RxJava 会相应的自动创建 Subscriber。

> 1. Action0 表示一个无回调参数的Action；
> 1. Action1 表示一个含有一个回调参数的Action；
> 1. 当然，还有Action2 ~ Action9，分别对应2~9个参数的Action；
> 1. 每个Action，都有一个 call() 方法，通过泛型T，来指定对应参数的类型；

这里使用observable.subscribe(onNextAction, onErrorAction, onCompleteAction)来完成注册

运行结果还是输出相应信息：如下

```
E/MainActivity: Hello World!
E/MainActivity: complete
```















# RxJava基础

# RxJava基础

# RxJava基础









# 参考博客

```
RxJava 从入门到出轨
http://blog.csdn.net/yyh352091626/article/details/53304728

深入浅出RxJava（一：基础篇）
http://blog.csdn.net/lzyzsd/article/details/41833541

深入浅出RxJava(二：操作符)
http://blog.csdn.net/lzyzsd/article/details/44094895
 
深入浅出RxJava三--响应式的好处
http://blog.csdn.net/lzyzsd/article/details/44891933
 
深入浅出RxJava四-在Android中使用响应式编程
http://blog.csdn.net/lzyzsd/article/details/45033611
 
RxJava 的使用入门
http://www.cnblogs.com/halzhang/p/4458095.html

RxJava漫谈-RxAndroid使用
http://www.bkjia.com/Androidjc/1080660.html
```



# 源码附录：

> 附录一：