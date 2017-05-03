> 借用聚美优品的广告词来开始今天的文章之旅：

> 从未年轻过的人，一定无法体会这个世界 的偏见。我们被世俗拆散，也要为爱情勇 往直前；我们被房价羞辱，也要让简陋的 现实变的温暖；我们被权威漠视，也要为 自己的天分保持骄傲；我们被平庸折磨， 也要开始说走就走的冒险。所谓的光辉岁 月，并不是后来闪耀的日子，而是无人问 津时，你对梦想的偏执，你是否有勇气， 对自己忠诚到底，我是Bruce常，我为自己加油。

![image](http://p2.pstatp.com/large/f730007f9b3d037f8c1)

> 平常开发中，在做倒计时效果的时候，经常需要用到定时器，今天看到一篇文章，专门写 定时器的，我就仔细阅读了一下，写的很好，很实用，自己平常工作中也都用到，但无奈没有总结。

原文章地址：http://www.apkbus.com/home.php?mod=space&uid=682543&do=blog&id=62012

#### 在Android开发中，我们一般用3个简单的方法来实现定时器的效果：
1. 采用Handler与线程的sleep(long)方法,通过线程休眠的方法来定时利用handler来发送数据，并在handler的handlerMessage()方法中进行处理。
2. 采用Handler的postDelayed(Runnable, long)方法，这也是我在开发中经常用到的方法，例如：启动页跳转引导页和主页面的时候，我就经常使用这种方法，还有ViewPager做轮播图的时候等等。
3. 采用Handler与timer及TimerTask结合的方法，这种方法在我的开发中，有一次要做直播页面，需要模仿观看回放时候的点赞效果，虚拟模仿观看者点赞效果，在1秒之内随机点多少个赞，就是用的这种方法实现的。

#### 下面详细介绍一下三个方法该怎么使用：

下面逐一介绍：

##### 一、采用Handle与线程的sleep(long)方法。

1. 定义一个Handler类，用于处理接受到的Message。
```
Handler handler = new Handler(){
@Override
public void handleMessage(Message msg) {
super.handleMessage(msg);
//这里写handler要处理的工作，例如页面的文字的改变，
}
};
```

2.  新建一个实现Runnable接口的线程类。
```
public class MyHandlerThread implements Runnable{
    @Override
    public void run() {
        while (true){
            try {
            //线程休眠一秒钟
            Thread.sleep(1000);
            //通过Message对象来发送消息
            Message message = new Message();
            message.what=1;
            handler.sendMessage(message);
            } catch (InterruptedException e) {
            e.printStackTrace();
            }
        }
    }
}
```
3.  在需要启动线程的地方通过Thread的start()方法来启动线程：
```
new Thread(new MyHandlerThread()).start();
```
4.  一旦通过3中的方法启动线程后，该线程将会每隔一秒钟发送一个消息，handler在接受到消息之后就可以进行数据的处理操作了。

##### 二、采用Handler的postDelayed(Runnable, long)方法。

这个实现比较简单，在实际开发中也经常用到。

1. 定义一个Handler类：
```
Handler handler = new Handler();
    Runnable runnable = new Runnable() {
    @Override
    public void run() {
    //这里写处理的逻辑
    handler.postDelayed(this, 2000);
    }
};
```
2.  启动计时器：
```
调用 handler.postDelayed(this, 2000);方法，会每隔两秒执行一个runnable中的run()方法，达到定时的效果。
```
3.  停止计时器：
```
handler.removeCallbacks(runnable);
```

##### 三、采用Handler与timer及TimerTask结合的方法，这种方法在用的时候稍微有点复杂，这里涉及到Timer和TImerTask这两个类的使用

1. 定义定时器、定时器任务及Handler句柄：
```
private Timer timer;
private TimerTask timerTask;
Handler handler = new Handler(){
    @Override
    public void handleMessage(Message msg) {
    //这里写要处理的逻辑
    super.handleMessage(msg);
    }
};
```
2.  初始化计时器任务：
```
timerTask = new TimerTask() {
    @Override
    public void run() {
    Message message = new Message();
    message.what = 1;
    handler.sendMessage(message);
    }
};
```
3.  启动定时器：
```
最后调用timer的schedule（）方法来启动定时器。
```
schedule（）如下所示：
![image](http://p1.pstatp.com/large/f790005a03ce866b44b)

> 我们一般调用timer的schedule(TimerTask task,long delay)和schedule(TimerTask task,long delay,long period)这两个方法，第一个方法，timerTask将会在dalay/1000秒后执行.且只执行一次。第二个方法会在dalay/1000秒后执行，且经过period/1000秒后将会重新执行该timeTask。

4.  关闭定时器：
```
timer.cancel();调用timer的cancle(）方法即可关闭该定时器。
```

5.  总结：

> 首先非常感谢原作者风吹过wu在安卓巴士网站的分享。其次掌握这三个方法不仅仅加深对Handle、Thread、Timer和TimerTask这几个类的理解，更重要的是可以提高我们在开发中的工作效率。

> 今日工作中需要实现一个类似淘宝的时分秒倒计时的效果，此效果就可以用上述所述的方法来完成，不过需要仔细计算而已。但是，开发中讲究的是高效率，高质量，于是自己就从GitHub上serach了一份代码用着挺好的，推荐给大家。

项目地址，可以下载原代码来学习：https://github.com/201216323/RushBuyCountDownTimerView， 下面简要说明一下用法。

这里主要是一个自定义的View-------RushBuyCountDownTimerView，

```
package com.qust.widght;
import java.util.Timer;
import java.util.TimerTask;
import android.annotation.SuppressLint;
import android.content.Context;
import android.os.Handler;
import android.os.Message;
import android.util.AttributeSet;
import android.view.LayoutInflater;
import android.view.View;
import android.widget.LinearLayout;
import android.widget.TextView;
import android.widget.Toast;
import com.qust.rushbuycountdowntimerview.R;
@SuppressLint("HandlerLeak")
public class RushBuyCountDownTimerView extends LinearLayout {
// 小时，十位
private TextView tv_hour_decade;
// 小时，个位
private TextView tv_hour_unit;
// 分钟，十位
private TextView tv_min_decade;
// 分钟，个位
private TextView tv_min_unit;
// 秒，十位
private TextView tv_sec_decade;
// 秒，个位
private TextView tv_sec_unit;
private Context context;
private int hour_decade;
private int hour_unit;
private int min_decade;
private int min_unit;
private int sec_decade;
private int sec_unit;
// 计时器
private Timer timer;
private Handler handler = new Handler() {
    public void handleMessage(Message msg) {
    countDown();
    };
};
public RushBuyCountDownTimerView(Context context, AttributeSet attrs) {
    super(context, attrs);
    this.context = context;
    LayoutInflater inflater = (LayoutInflater) context
    .getSystemService(Context.LAYOUT_INFLATER_SERVICE);
    View view = inflater.inflate(R.layout.view_countdowntimer, this);
    tv_hour_decade = (TextView) view.findViewById(R.id.tv_hour_decade);
    tv_hour_unit = (TextView) view.findViewById(R.id.tv_hour_unit);
    tv_min_decade = (TextView) view.findViewById(R.id.tv_min_decade);
    tv_min_unit = (TextView) view.findViewById(R.id.tv_min_unit);
    tv_sec_decade = (TextView) view.findViewById(R.id.tv_sec_decade);
    tv_sec_unit = (TextView) view.findViewById(R.id.tv_sec_unit);
}
/**
*
* @Description: 开始计时
* @param
* @return void
* @throws
*/
public void start() {
    if (timer == null) {
    timer = new Timer();
    timer.schedule(new TimerTask() {
    @Override
    public void run() {
    handler.sendEmptyMessage(0);
    }
    }, 0, 1000);
    }
}
/**
*
* @Description: 停止计时
* @param
* @return void
* @throws
*/
public void stop() {
    if (timer != null) {
    timer.cancel();
    timer = null;
    }
}
/**
* @throws Exception
*
* @Description: 设置倒计时的时长
* @param
* @return void
* @throws
*/
public void setTime(int hour, int min, int sec) {
    if (hour >= 60 || min >= 60 || sec >= 60 || hour < 0 || min < 0|| sec < 0) {
    throw new RuntimeException("Time format is error,please check out your     code");
}
    hour_decade = hour / 10;
    hour_unit = hour - hour_decade * 10;
    min_decade = min / 10;
    min_unit = min - min_decade * 10;
    sec_decade = sec / 10;
    sec_unit = sec - sec_decade * 10;
    tv_hour_decade.setText(hour_decade + "");
    tv_hour_unit.setText(hour_unit + "");
    tv_min_decade.setText(min_decade + "");
    tv_min_unit.setText(min_unit + "");
    tv_sec_decade.setText(sec_decade + "");
    tv_sec_unit.setText(sec_unit + "");
}
/**
*
* @Description: 倒计时
* @param
* @return boolean
* @throws
*/
private void countDown() {
    if (isCarry4Unit(tv_sec_unit)) {
        if (isCarry4Decade(tv_sec_decade)) {
            if (isCarry4Unit(tv_min_unit)) {
                if (isCarry4Decade(tv_min_decade)) {
                    if (isCarry4Unit(tv_hour_unit)) {
                        if (isCarry4Decade(tv_hour_decade)) {
                            Toast.makeText(context,         "时间到了",Toast.LENGTH_SHORT).show();
                            stop();
                        }
                    }
                }
            }
        }
    }
}
/**
*
* @Description: 变化十位，并判断是否需要进位
* @param
* @return boolean
* @throws
*/
private boolean isCarry4Decade(TextView tv) {
    int time = Integer.valueOf(tv.getText().toString());
    time = time - 1;
    if (time < 0) {
        time = 5;
        tv.setText(time + "");
        return true;
    } else {
        tv.setText(time + "");
    return false;
    }
}
/**
*
* @Description: 变化个位，并判断是否需要进位
* @param
* @return boolean
* @throws
*/
private boolean isCarry4Unit(TextView tv) {
    int time = Integer.valueOf(tv.getText().toString());
    time = time - 1;
    if (time < 0) {
        time = 9;
        tv.setText(time + "");
        return true;
    } else {
        tv.setText(time + "");
        return false;
        }
    }
}
```
从上面代码中即可看到，此倒计时效果就是使用上面所提到的第三种方法来完成的，其中所用到的view_countdowntimer布局文件如下：

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:background="@android:color/white"
android:orientation="horizontal"
android:padding="10dp" >
    <TextView
        android:id="@+id/tv_hour_decade"
        style="@style/RushBuyCountDownTimerViewStyle" />
    <TextView
        android:id="@+id/tv_hour_unit"
        style="@style/RushBuyCountDownTimerViewStyle"
        android:layout_marginLeft="1dp" />
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:background="@android:color/white"
        android:gravity="center"
        android:text=":"
        android:textColor="#4F4242"
        android:textSize="30sp" />
    <TextView
        android:id="@+id/tv_min_decade"
        style="@style/RushBuyCountDownTimerViewStyle" />
    <TextView
        android:id="@+id/tv_min_unit"
        style="@style/RushBuyCountDownTimerViewStyle"
        android:layout_marginLeft="1dp" />
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:background="@android:color/white"
        android:gravity="center"
        android:text=":"
        android:textColor="#4F4242"
        android:textSize="30sp" />
    <TextView
        android:id="@+id/tv_sec_decade"
        style="@style/RushBuyCountDownTimerViewStyle" />
    <TextView
        android:id="@+id/tv_sec_unit"
        style="@style/RushBuyCountDownTimerViewStyle"
        android:layout_marginLeft="1dp" />
</LinearLayout>
```
RushBuyCountDownTimerViewStyle这个Style文件如下，在这个文件中主要定义字体大小、颜色、背景色等：

```
<style name="RushBuyCountDownTimerViewStyle">
    <item name="android:layout_width">wrap_content</item>
    <item name="android:layout_height">wrap_content</item>
    <item name="android:background">@drawable/bg_view</item>
    <item name="android:gravity">center</item>
    <item name="android:text">0</item>
    <item name="android:textColor">@android:color/white</item>
    <item name="android:textSize">35sp</item>
    <item name="android:textStyle">bold</item>
</style>
```
上面的布局效果默认是这样的：

```
00：00：00 //分别对应时、分、秒
```

最后：
    假如需要在MainActivity.java中使用此倒计时控件，只需要在MainActivityjava的布局文件activity_main.xml中加入此控件，设置倒计时的时候，然后在代码中调用start(）方法即可实现倒计时的效果。

activity_main.xml文件如下：

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
    <com.qust.widght.RushBuyCountDownTimerView
        android:id="@+id/timerView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" >
    </com.qust.widght.RushBuyCountDownTimerView>
</LinearLayout>
```
MainActivity.java文件如下：

```
package com.qust.widght;
import android.app.Activity;
import android.os.Bundle;
import com.qust.rushbuycountdowntimerview.R;
public class MainActivity extends Activity {
private RushBuyCountDownTimerView timerView;
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    timerView = (RushBuyCountDownTimerView) findViewById(R.id.timerView);
    //设置倒计时的时、分、秒
    timerView.setTime(10, 0, 10);
    // 开始倒计时
    timerView.start();
    }
}
```
最后：通过上面的几个步骤即可完成倒计时效果，且rushbuycountdowntimerview中也加入了倒计时结束时候的逻辑处理，一般我们需要在这里给用户提示信息，或者对View进行处理等，详细的就不在这里说了。

> 已经晚上十一点了，哥们我也已经累了，虽然这些文章不全是我的原创，我也是在看别人的文章结合自己的能力做一些个人总结而已，希望大家，喜欢的做个转发，不喜欢的勿喷勿吐槽，我又没有碍着你什么事情。

拜拜，洗洗睡觉去。。。

欢迎关注我的个人技术公众号,快速查看我的最新文章。

 ![image](http://a3.qpic.cn/psb?/V10Llwbb1wSOar/BtfTjBTRyI559tdUkmGNeFyh0rTwFW3FSL2oyva.Lqs!/b/dGYBAAAAAAAA&ek=1&kp=1&pt=0&bo=AgECAQIBAgEFACM!&tm=1482058800&sce=60-2-2&rf=viewer_311)


                                                    时间：2016-10-17 23:00

