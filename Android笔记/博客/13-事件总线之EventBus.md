# 1：简介

EventBus 是一个Android端优化的publish（发布者）/subscribe（订阅者）消息总线，简化了应用程序内各组件之间、组件与后台线程间的通信。比如：请求网络，等网络返回时通过Handle或者Broadcast通知UI，两个Fragment之间需要通过Listener通信，这些需求都可以通过EventBus实现。

# 2：EventBus下载地址

Github下载地址是：https://github.com/greenrobot/EventBus， 可以访问查看里面的一些介绍。

# 3：使用步骤

## 3.1：添加依赖

```
compile 'org.greenrobot:eventbus:3.0.0'
```
此时的最新版本是 3.0.0。

## 3.2：注册


```
EventBus.getDefault().register(this);
```

## 3.3：解注册


```
EventBus.getDefault().unregister(this);
```

## 3.4：构造发送消息类


```
public class LocalMessage {

    private String name;
    private int age;

    public LocalMessage(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        String s = "我的名称是：" + name + "\n" + "我的年纪是：" + age;
        return s;
    }
}
```

## 3.5：发布消息

```
EventBus.getDefault().post(new LocalMessage("风清扬", 20));
```

## 3.6：接收消息

1. ThreadMode.MAIN 表示这个方法在主线程中执行
2. ThreadMode.BACKGROUND 表示该方法在后台执行，不能并发处理
3. ThreadMode.ASYNC 也表示在后台执行，可以异步并发处理。
4. ThreadMode.POSTING 表示该方法和消息发送方在同一个线程中执行


```
 //5：接收消息
    @Subscribe(threadMode = ThreadMode.MAIN)
    public void LocalMessage(LocalMessage localMessage) {
        tv_receive_event.setText(localMessage.toString());
    }
```


# 4：粘性事件

第三部分中的使用方法, 都是需要先注册(register), 再post,才能接受到事件; 
如果你使用postSticky发送事件, 那么可以不需要先注册, 也能接受到事件。

## 4.1：构造发送信息类

```
public class LocalMessageStick {

    private String name;
    private int age;

    public LocalMessageStick(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        String s = "我的名称是：" + name + "\n" + "我的年纪是：" + age;
        return s;
    }
}

```

## 4.2：发布消息

```
  EventBus.getDefault().postSticky(new LocalMessageStick("张无忌", 30));
```

## 4.3：接收消息


```
    //注意,和之前的方法一样,只是多了一个 sticky = true 的属性.
    @Subscribe(threadMode = ThreadMode.MAIN, sticky = true)
    public void LocalMessageStick(LocalMessageStick localMessage) {
        tv_receive_event.setText(localMessage.toString());
    }
```

## 4.4：注册


```
 EventBus.getDefault().register(SubscribeActivity.this);
```

## 4.5：解注册


```
  EventBus.getDefault().removeAllStickyEvents();
  EventBus.getDefault().unregister(SubscribeActivity.this);
```

# 5：例子

主要通过两个Activity使用EventBus实现值传递的效果

## 5.1：主程序

> MainActivity.java程序

```
public class MainActivity extends AppCompatActivity {

    Button bt_send, bt_send_stick;
    TextView tv_receive_event;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        bt_send = (Button) findViewById(R.id.bt_send);
        bt_send_stick = (Button) findViewById(R.id.bt_send_stick);
        tv_receive_event = (TextView) findViewById(R.id.tv_receive_event);
        //1：注册
        EventBus.getDefault().register(MainActivity.this);
        //跳转到发送页面
        bt_send.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                startActivity(new Intent(MainActivity.this, SubscribeActivity.class));
            }
        });
        //发送粘性事件跳转到发送页面
        bt_send_stick.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //1:构造消息发送的实体类这里使用LocalMessageStick类
                //2：发送粘性事件消息
                EventBus.getDefault().postSticky(new LocalMessageStick("张无忌", 30));
                startActivity(new Intent(MainActivity.this, SubscribeActivity.class));
            }
        });
    }

    //5：接收消息
    @Subscribe(threadMode = ThreadMode.MAIN)
    public void LocalMessage(LocalMessage localMessage) {
        tv_receive_event.setText(localMessage.toString());
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        //2：解注册
        EventBus.getDefault().unregister(MainActivity.this);
    }
}
```

> SubscribeActivity.java程序


```
public class SubscribeActivity extends AppCompatActivity {

    Button bt_send_to_main, bt_receive_event;
    TextView tv_receive_event;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_subscribe);
        setTitle("Eventbus发送数据页面");
        bt_send_to_main = (Button) findViewById(R.id.bt_send_to_main);
        bt_receive_event = (Button) findViewById(R.id.bt_receive_event);
        tv_receive_event = (TextView) findViewById(R.id.tv_receive_event);


        //发送数据到主线程
        bt_send_to_main.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //4：发送数据
                EventBus.getDefault().post(new LocalMessage("风清扬", 20));
                finish();
            }
        });
        //接收粘性事件
        bt_receive_event.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //4：注册（注册后既可以接收到粘性事件）
                EventBus.getDefault().register(SubscribeActivity.this);
            }
        });
    }


    //3：接收消息
    @Subscribe(threadMode = ThreadMode.MAIN, sticky = true)
    public void LocalMessageStick(LocalMessageStick localMessage) {
        tv_receive_event.setText(localMessage.toString());
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        //5：解注册
        EventBus.getDefault().unregister(SubscribeActivity.this);
    }
}

```


## 5.2：结果

![image](http://wx2.sinaimg.cn/mw690/b0d9a523ly1fb6te5is74g208y0fwjvy0.gif)

# 6：总结

没接触EventBus之前感觉这个东西有点高大上，从单词的字面意思就可以知道，Event是事件，bus是公交车、线，就是用于数据传递的，用了之后发现，确实是这样的，使用EventBus传递数据确实非常的方便，以后可以多多的利用一下。


> 欢迎访问[201216323.tech](http://www.201216323.tech)来查看我的CSDN博客。

> 欢迎关注我的个人技术公众号,快速查看我的最新文章。

![我的公众号图片](http://img.blog.csdn.net/20161220174646569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2NnXzIwMTIxNjMyMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "bruce常")



