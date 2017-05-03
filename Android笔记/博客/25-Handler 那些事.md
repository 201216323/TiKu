Handler 对于 Android

开发来说简直就是家常便饭，它的原理自然都很熟悉，这篇文章不会宏观地去介绍它的原理，而是细节深入到各个组成。

**关系**

开始深入细节的时候，我们可以先复习下 Handler 、Looper 和 MessageQueue 三者的关系。

1. Handler 必须在 Looper.prepare() 之后才能创建使用
1. Looper 与当前线程关联，并且管理着一个 MessageQueue
1. Message 是实现 Parcelable 接口的类
1. 以一个线程为基准，他们的数量级关系是：Handler(N) : Looper(1) : MessageQueue(1) : Thread(1)

他们的调用关系可以参考这张图：

![image](http://ww1.sinaimg.cn/large/7853084cgw1f7x2g6lzkbj20k60b9t9j.jpg)

**分析**

## 0x01


```
public Handler(Callback callback, boolean async) {
    // 代码省略
    mLooper = Looper.myLooper();
    if (mLooper == null) {
        throw new RuntimeException(
            "Can't create handler inside thread that has not called Looper.prepare()");
    }
    mQueue = mLooper.mQueue;
    mCallback = callback;
    mAsynchronous = async;
}
```
从 Handler 默认的构造函数我们可以看到，Handler 内部会通过 Looper.myLooper() 来获取 Looper 对象，从而与之关联。

## 0x02

我们之前已经知道 Looper 管理着消息队列，从这里深入进去看看是如何跟 MessageQueue 建立联系。

```
public static @Nullable Looper myLooper() {
    return sThreadLocal.get();
}
public static void prepare() {
    prepare(true);
}
private static void prepare(boolean quitAllowed) {
    if (sThreadLocal.get() != null) {
        throw new RuntimeException("Only one Looper may be created per thread");
    }
    sThreadLocal.set(new Looper(quitAllowed));
}
public static void prepareMainLooper() {
    prepare(false);
    synchronized (Looper.class) {
        if (sMainLooper != null) {
            throw new IllegalStateException("The main Looper has already been prepared.");
        }
        sMainLooper = myLooper();
    }
}
```
在 Looper.myLooper() 里我们看到，Looper 是通过 sThreadLocal.get() 来获取，那么我们又是何时将 Looper 设置给 sThreadLocal 的呢？答案就在 prepare() 方法里。

我们看到 sThreadLocal.set(new Looper(quitAllowed)); 实例化了一个 Looper 对象给 sThreadLocal 并且一个线程只有一个 Looper 。

同时我也贴出了 prepareMainLooper() 方法，根据名字大家都可以猜到，这个方法就是在 Android 主线程(UI)线程调用的方法，而在这个方法里也调用了 prepare(false) 我们看到这里传入的是 false ，表明主线程这里的 Looper 是无法执行 quit() 方法。

我在这里贴出 ActivityThread 的 Main() 方法的部分代码，这也是我们程序的入口：


```
public static void main(String[] args) {
    // 代码省略
    Looper.prepareMainLooper(); // 创建消息循环 Looper
    ActivityThread thread = new ActivityThread();
    thread.attach(false);
    if (sMainThreadHandler == null) {
        sMainThreadHandler = thread.getHandler(); // UI 线程的 Handler
    }
    if (false) {
        Looper.myLooper().setMessageLogging(new
                LogPrinter(Log.DEBUG, "ActivityThread"));
    }
    Looper.loop(); // 执行消息循环
}
```
在这里我们更清楚了为什么可以直接在主线程创建 Handler ，而不会发生异常。

以上，我们明白了 Looper 是通过 prepare() 方法与线程建立联系，同时不同线程是无法访问对方的消息队列。

#### 为什么 Handler 要在主线程创建才能更新 UI 呢？

因为 Handler 要与主线程的消息队列关联上，这样 handleMessage() 才会执行在 UI 线程。

## 0x03

Looper 的核心其实是它循环取出消息的代码：


```
public static void loop() {
    final Looper me = myLooper();
    if (me == null) {
        throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
    }
    final MessageQueue queue = me.mQueue;
    // 死循环
    for (;;) {
        Message msg = queue.next(); // might block
        if (msg == null) {
            // No message indicates that the message queue is quitting.
            return;
        }
        /// Handler msg.target
        msg.target.dispatchMessage(msg); // 派发消息
        // 代码省略
        msg.recycleUnchecked();
    }
}
```
从上面代码我们可以看到，Looper 在 loop() 方法里建立了一个死循环，通过消息队列里不断的取出消息，交给 Handler 去处理。

这个时候你可能会有一个问题：

#### Android 中为什么主线程不会因为 Looper.loop() 里的死循环卡死？

回到我们这里，在循环中是通过 msg.target.dispatchMessage(msg); 派发消息。其中 msg 是 Message 类型，简单看看它的成员：


```
public final class Message implements Parcelable {
    Handler target;
    Runnable callback;
    Message next;
    public Object obj;
    public int arg1;
    public int arg2;
    // 代码省略
}
```

可以知道消息队列是链表实现的，并且 target 是 Handler 类型。

现在就可以连通了，通过 Handler 将 Message 投递给消息队列（链表），Looper.loop() 循环从消息队列里取出消息，又将消息分发给 Handler 去处理。通过这个 target 我们也可以知道一个小细节，Handler 只能处理自己所发出的消息。

## 0x04

理解清楚之后我们跟着顺序，看看 Handler 是如何处理和分发消息的。


```
// 处理消息方法，交给子类复写
public void handleMessage(Message msg) {
}
public void dispatchMessage(Message msg) {
    if (msg.callback != null) {
        handleCallback(msg);
    } else {
        if (mCallback != null) {
            if (mCallback.handleMessage(msg)) {
                return;
            }
        }
        handleMessage(msg);
    }
}
private static void handleCallback(Message message) {
    message.callback.run();
}
```

我们看到 dispatchMessage() 只是一个分发方法，如果 Runnable 类型的 callback 为空，则执行handleMessage(msg) 处理信息，该方法为空，是交给子类进行复写，并且执行线程是在 Handler 所创建的线程。

如果 callback 不为空，则会执行 handleCallback(msg) 来处理信息，该方法会调用 callback 的 run()方法。

其实说简单一点，就是 Handler 的两种分发类型。
一种是 post(r) 另一种是 sendMessage(msg)。
我们具体看看这两个方法：

```
public final boolean post(Runnable r){
   return  sendMessageDelayed(getPostMessage(r), 0);
}
private static Message getPostMessage(Runnable r){
    Message m = Message.obtain();
    m.callback = r;
    return m;
}
public final boolean sendMessage(Message msg){
    return sendMessageDelayed(msg, 0);
}
public final boolean sendMessageDelayed(Message msg, long delayMillis){
    if (delayMillis < 0) {
        delayMillis = 0;
    }
    return sendMessageAtTime(msg, SystemClock.uptimeMillis() + delayMillis);
}
public boolean sendMessageAtTime(Message msg, long uptimeMillis) {
    MessageQueue queue = mQueue;
    if (queue == null) {
        RuntimeException e = new RuntimeException(
                this + " sendMessageAtTime() called with no mQueue");
        Log.w("Looper", e.getMessage(), e);
        return false;
    }
    return enqueueMessage(queue, msg, uptimeMillis);
}
private boolean enqueueMessage(MessageQueue queue, Message msg, long uptimeMillis) {
    msg.target = this; // 与当前 Handler 绑定
    if (mAsynchronous) {
        msg.setAsynchronous(true);
    }
    return queue.enqueueMessage(msg, uptimeMillis);
}
```

做了一个导图，方便理解下：
![image](http://ww4.sinaimg.cn/large/7853084cgw1f7xsolnxzmj21kw0ihdh4.jpg)

从中我们可以看到，在 post(r) 时，会将 Runnable 包装成 Message 对象，并且赋值给 Message 的 callback 字段，最后跟 sendMessage(msg) 方法一样将消息插入队列。
根据代码和导图，无论是 post(r) 还是 sendMessage(msg) 都会最终调用sendMessageAtTime(msg,time)

## 总结

Handler 最终将消息追加到 MessageQueue 中，而 Looper 不断的从 MessageQueue 中读取消息，并且调用 Handler 的 dispatchMessage 分发消息，最后交给上层处理消息。