前言：
上一篇Android 5.0的文章，小编仔细学习使用了TextInputLayout和Snackbar，不清楚的可以查看[原文链接](http://note.youdao.com/noteshare?id=832547f1f6b12fbfbeda7421e7e6017a)，这一篇文章中将会全部介绍5.0中其它比较常用的控件，下面是目录：

1. Material Dialog
1. FloatingActionButton
1. SwipeRefreshLayout
1. LinearLayoutCompat
1. ListPopupWindow
1. PopupMenu
1. Toolbar
1. TabLayout(选项卡布局)
1. AppBarLayout（程序栏布局）&& CoordinatorLayout（协作布局）
1. CollapsingToolbarLayout（折叠工具栏布局）
1. NestedScrollView的使用


## Material Dialog

Dialog我们在开发中经常用到，但不经常使用V7包里面的Material 风格的对话框，建议以后使用这种Dialog，效果太酷了。

```
 private void showMaterialDialog() {
        android.support.v7.app.AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setMessage("我爱你 爱着你就像老鼠爱大米")
                .setCancelable(false)
                .setNegativeButton("取消", null)
                .setPositiveButton("确定", null)
                .setTitle("this is a Material Design Dialog")
                .show();
}
```
效果如下：
![image](http://ww4.sinaimg.cn/mw690/b0d9a523jw1fa4el7pdnfg20be07lwh3.gif)

## FloatingActionButton（悬浮的按钮）

在android.support.design.widget包下面，可以使用FloatingActionButton来做悬浮按钮，虽然这种效果我们可以使用ImageView来实现，但谷歌给我们提供了做悬浮按钮的控件我们就不用再自己创造了，查看FloatingActionButton的源代码可以发现，此控件是继承自ImageView的，所以可是使用ImageView的一切属性，但它也有属于自己及的专有属性：


```
1. app:fabSize="normal"设置大小，有两种赋值分别是 “mini” 和 “normal”，默认是“normal”
2. app:borderWidth="10dp"设置边框的宽度
3. app:backgroundTint="@color/colorPrimary"设置FloatingActionButton的背景颜色，默认的背景颜色是Theme主题中的<item name="colorAccent">#ff0000</item>
4. app:elevation="20dp"设置FloatingActionButton阴影的深度
```
效果如下：
![image](http://ww3.sinaimg.cn/mw690/b0d9a523jw1fa4flgfpb1g20bd07l75a.gif)

布局文件如下：

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <android.support.design.widget.FloatingActionButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:layout_margin="20dp"
        app:borderWidth="5dp"
        app:backgroundTintMode="src_in"
        app:backgroundTint="@color/colorPrimary"
        app:elevation="20dp"
        />

    <android.support.design.widget.FloatingActionButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:layout_margin="20dp"
        app:fabSize="normal"
        app:borderWidth="10dp"
        app:elevation="20dp"
        />

    <android.support.design.widget.FloatingActionButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:layout_margin="20dp"
        app:fabSize="mini"
        app:borderWidth="5dp"
        app:backgroundTintMode="src_in"
        app:backgroundTint="@color/colorPrimary"
        app:elevation="20dp"
        />

</LinearLayout>
```
## SwipeRefreshLayout（下拉刷新）

SwipeRefreshLayout使用这种控件做出的刷新效果现在非常见，印象比较深的就是直播APP上，它是在android.support.v4.widget包下，SwipeRefreshLayout组件下必须包裹一个可滑动的组件才可实现下拉刷新效果。

布局代码如下：

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.SwipeRefreshLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/swipeContainer"
    android:orientation="vertical">

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical"
            >

            /*这里放多个TextView来测试*/
        </LinearLayout>

    </ScrollView>

</android.support.v4.widget.SwipeRefreshLayout>
```
java程序：

```
public class SwipeRefreshActivity extends AppCompatActivity implements SwipeRefreshLayout.OnRefreshListener {

    SwipeRefreshLayout swipeContainer;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_swiprefresh);
        swipeContainer = (SwipeRefreshLayout) findViewById(R.id.swipeContainer);
        //设置下拉刷新监听事件
        swipeContainer.setOnRefreshListener(this);
        //设置进度条的颜色
        swipeContainer.setColorSchemeColors(Color.RED, Color.BLUE, Color.GREEN);
        //设置圆形进度条大小
        swipeContainer.setSize(SwipeRefreshLayout.DEFAULT);
        //设置进度条背景颜色
//        swipeContainer.setProgressBackgroundColorSchemeColor(Color.DKGRAY);
        //设置下拉多少距离之后开始刷新数据
//        swipeContainer.setDistanceToTriggerSync(50);

    }
    @Override
    public void onRefresh() {
        new Handler().postDelayed(new Runnable() {
            @Override
            public void run() {
                Snackbar.make(swipeContainer, "刷新结束", Snackbar.LENGTH_SHORT).show();
                swipeContainer.setRefreshing(false);
            }
        }, 2000);
    }
}

```
SwipeRefreshLayout的一些常用的设置都写在代码中了，在不设置进度条颜色的时候默认是黑色的，SwipeRefreshLayout只有下拉刷新的功能，但在实际开发中还需要上拉加载功能，这都可以去Github上search 。

效果如下：
![image](http://ww1.sinaimg.cn/mw690/b0d9a523jw1fa4fv5y7xpg20aq0hrac7.gif)

## LinearLayoutCompat

V7包中有一个LinearLayoutCompat组件,可以给LinerLayout 中的子元素item之间设置分割线。

布局文件：


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <!--divider必须放在drawable中才能生效-->
    <android.support.v7.widget.LinearLayoutCompat
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:divider="@drawable/line"
        app:dividerPadding="10dp"
        app:showDividers="middle"
        >

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="#ffffff"
            android:gravity="center"
            android:padding="10dp"
            android:text="text"
            android:textAllCaps="false"
            android:textColor="@color/colorAccent"
            android:textSize="14sp"/>

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="#ffffff"
            android:gravity="center"
            android:padding="10dp"
            android:text="text"
            android:textAllCaps="false"
            android:textColor="@color/colorAccent"
            android:textSize="14sp"/>

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="#ffffff"
            android:gravity="center"
            android:padding="10dp"
            android:text="text"
            android:textAllCaps="false"
            android:textColor="@color/colorAccent"
            android:textSize="14sp"/>

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="#ffffff"
            android:gravity="center"
            android:padding="10dp"
            android:text="text"
            android:textAllCaps="false"
            android:textColor="@color/colorAccent"
            android:textSize="14sp"/>
    </android.support.v7.widget.LinearLayoutCompat>

</LinearLayout>

```
LinearLayoutCompat控件有几个属性：


```
1. app:divider=”@drawable/line” 给分隔线设置颜色，这里你需要在drawable在定义shape资源，否则将没有效果，不信的小伙伴can try。
1. app:dividerPadding=”25dp” 给分隔线设置填充大小。
1. app:showDividers=”middle|beginning|end” 分隔线显示的位置，有四种参数值：middle 每个item之间，beginning最顶端显示分隔线，end 最底端显示分隔线，none不显示间隔线。
```
效果如下;

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fa4go9cu6kg20aq0hr0tz.gif)

## PopupWindow
PopupWindow这个控件在开发中经常用到，但V7包中还有一个PopupWindow，可以实现简单的PopupWindow的效果。
测试程序：

```
public void showListPopup(View view) {
        String items[] = {"item1", "item2", "item3", "item4", "item5"};
        final ListPopupWindow listPopupWindow = new ListPopupWindow(this);

        //设置ListView类型的适配器
        listPopupWindow.setAdapter(new ArrayAdapter<String>(ListPopupWindowActivity.this, android.R.layout.simple_list_item_1, items));

        //给每个item设置监听事件
        listPopupWindow.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                Snackbar.make(lineContainer, "the position is" + position, Snackbar.LENGTH_SHORT)
                        .setCallback(new Snackbar.Callback() {
                            @Override
                            public void onDismissed(Snackbar snackbar, int event) {
                                super.onDismissed(snackbar, event);
                                new android.os.Handler().postDelayed(new Runnable() {
                                    @Override
                                    public void run() {
                                        listPopupWindow.dismiss();
                                    }
                                }, 500);
                            }
                        })
                        .show();
            }
        });
        //设置ListPopupWindow的锚点,也就是弹出框的位置是相对当前参数View的位置来显示，
        listPopupWindow.setAnchorView(view);
        //ListPopupWindow 水平偏移量
        listPopupWindow.setHorizontalOffset(100);
        listPopupWindow.setVerticalOffset(100);
        //设置对话框的宽高
        listPopupWindow.setWidth(lineContainer.getLayoutParams().width);
        listPopupWindow.setHeight(600);
        listPopupWindow.setModal(false);
        //设置点击外边区域不可取消
        listPopupWindow.setForceIgnoreOutsideTouch(true);
        listPopupWindow.show();
    }
```
效果图如下：
![image](http://ww3.sinaimg.cn/mw690/b0d9a523jw1fa4gv2482ig20aq0hrdin.gif)
## PopupMenu 
这是一个菜单式弹出框的控件，可以用这个控件来使用Menu中的文件，简单来使用一下。

测试程序：

```
public void showPopupMenu(View view) {
        //参数View 是设置当前菜单显示的相对于View组件位置，具体位置系统会处理
        PopupMenu popupMenu = new PopupMenu(this, view);
        //加载menu布局
        popupMenu.getMenuInflater().inflate(R.menu.menu_popmenu, popupMenu.getMenu());

        //设置menu中的item点击事件
        popupMenu.setOnMenuItemClickListener(new PopupMenu.OnMenuItemClickListener() {
            @Override
            public boolean onMenuItemClick(MenuItem item) {
                switch (item.getItemId()) {
                    case R.id.share:
                        Snackbar.make(line1Container, "share", Snackbar.LENGTH_SHORT).show();
                        break;
                    case R.id.setting:
                        Snackbar.make(line1Container, "setting", Snackbar.LENGTH_SHORT).show();
                        break;
                    case R.id.cancle:
                        Snackbar.make(line1Container, "cancle", Snackbar.LENGTH_SHORT).show();
                        break;
                }
                return true;
            }
        });
        popupMenu.show();
    }
```
menu代码：

```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/share"
        android:icon="@mipmap/mine_login_account"
        android:orderInCategory="0"
        android:title="分享"
        />
    <item
        android:id="@+id/setting"
        android:icon="@mipmap/mine_login_account"
        android:orderInCategory="1"
        android:title="设置"
        />
    <item
        android:id="@+id/cancle"
        android:icon="@mipmap/mine_login_account"
        android:orderInCategory="2"
        android:title="取消"
        />
</menu>
```
效果图如下：

![image](http://ww3.sinaimg.cn/mw690/b0d9a523jw1fa4h1ngehxg20aq0hrmzr.gif)

## ToolBar

ToolBar是谷歌在Android5.0的时候推出的，是在V7包中，是为了替换之前的ActionBar，Toolbar的出现解决了Actionbar的各种限制，Toolbar可以完全自定义和配置，下面详细说明一下具体使用。

为了能在你的Activity中使用Toolbar，你必须在工程里修改styles.xml文件里的主题风格，系统默认如下

```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
```
这种Theme表示使用系统之前的ActionBar，那么我们想要使用Toolbar怎么办呢？使用下面这个主题。

```
<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
```

下面具体来说一下Toolba的使用

主题文件：

```
<style name="AppThemeA" parent="Theme.AppCompat.Light.NoActionBar">
        <!--导航栏底色-->
        <item name="colorPrimary">@color/colorPrimary</item>
        <!--状态栏的颜色-->
        <item name="colorPrimaryDark">#0000ff</item>
        <!--导航栏上的标题颜色-->
        <item name="android:textColorPrimary">#FF4081</item>
        <!--Activity窗口的背景颜色-->
        <item name="android:windowBackground">@color/highlighted_text_material_dark</item>

        <!--按钮选中或者点击获得焦点后的颜色-->
        <item name="colorAccent">#00ff00</item>
        <!--和 colorAccent相反，正常状态下按钮的颜色-->
        <item name="colorControlNormal">#ff0000</item>
        <!--Button按钮正常状态颜色-->
        <item name="colorButtonNormal">#00ff00</item>
        <!--EditText 输入框中字体的颜色-->
        <item name="editTextColor">#00ff00</item>
        <item name="android:textColorHint">#00ff00</item>
    </style>
```
manifests中使用此Style：

```
<activity
    android:name=".ToolbarActivity"
    android:theme="@style/AppThemeA"/>
```

布局文件：

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <!--actionBarSize默认高度是56dp&&colorPrimary对应颜色@color/colorPrimary-->
    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:background="?attr/colorPrimary"></android.support.v7.widget.Toolbar>


    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="男"
        android:textColor="@android:color/white" />

    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="男"
        android:textColor="@android:color/white" />

    <RadioGroup
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <RadioButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="男"
            android:textColor="@android:color/white" />

        <RadioButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="男"
            android:textColor="@android:color/white" />
    </RadioGroup>

    <android.support.v7.widget.SwitchCompat
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="男"
        android:textColor="@android:color/white" />

    <android.support.v7.widget.SwitchCompat
        android:id="@+id/switchCompatBoy"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="女"
        android:textColor="@android:color/white" />

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="请输入密码" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="模式切换" />

</LinearLayout>
```
Res下menu文件夹中的menu文件：

```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/action_edit"
        android:icon="@drawable/abc_ic_menu_share_mtrl_alpha"
        android:orderInCategory="0"
        android:title="A"
        app:showAsAction="always" />

    <item
        android:id="@+id/action_share"
        android:icon="@drawable/abc_ic_menu_share_mtrl_alpha"
        android:orderInCategory="2"
        android:title="B"
        app:showAsAction="always" />

    <item
        android:id="@+id/action_settings"
        android:orderInCategory="3"
        android:title="C"
        app:showAsAction="never" />

    <item
        android:id="@+id/action_other"
        android:icon="@drawable/abc_ic_menu_share_mtrl_alpha"
        android:orderInCategory="4"
        android:title="D"
        app:showAsAction="never" />
</menu>
```

java代码中调用

```
public class ToolbarActivity extends AppCompatActivity implements Toolbar.OnMenuItemClickListener {
    Toolbar toolbar;
    Button button;
    SwitchCompat switchCompatBoy;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_titlebar);
        button = (Button) findViewById(R.id.button);
        toolbar = (Toolbar) findViewById(R.id.toolbar);
        switchCompatBoy = (SwitchCompat) findViewById(R.id.switchCompatBoy);
        setSupportActionBar(toolbar);
//        上面代码用来隐藏系统默认的Title。
        getSupportActionBar().setDisplayShowTitleEnabled(false);
        toolbar.setTitle("主标题");
        toolbar.setSubtitle("副标题");
        toolbar.setSubtitleTextColor(getResources().getColor(R.color.colorAccent));
        toolbar.setLogo(R.mipmap.mine_login_account);
        toolbar.setNavigationIcon(android.R.drawable.ic_notification_clear_all);
        toolbar.setNavigationOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                finish();
            }
        });

        toolbar.setOnMenuItemClickListener(this);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(ToolbarActivity.this, "aaa", Toast.LENGTH_SHORT).show();
            }
        });

        switchCompatBoy.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton compoundButton, boolean b) {
                if (b) {
                    Toast.makeText(ToolbarActivity.this, "checked", Toast.LENGTH_SHORT).show();
                } else {
                    Toast.makeText(ToolbarActivity.this, "no checked", Toast.LENGTH_SHORT).show();
                }
            }
        });
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_titlebar, menu);
        return super.onCreateOptionsMenu(menu);
    }

    @Override
    public boolean onMenuItemClick(MenuItem item) {
        switch (item.getItemId()) {
            case R.id.action_edit:
                Toast.makeText(ToolbarActivity.this, "查找按钮", Toast.LENGTH_SHORT).show();
                break;
            case R.id.action_share:
                Toast.makeText(ToolbarActivity.this, "分享按钮", Toast.LENGTH_SHORT).show();
                break;
        }
        return false;
    }
}
```
效果如下：

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fa4i8s8owig20aq0hrh2s.gif)

上面就是程序部分，里面有很清楚的注释，Toolbar可以自定义，这个在新建的带侧边栏的Activity中就有，这里不多说明，这里以一张图片来说明上面为什么是这种效果。
![image](http://img.blog.csdn.net/20150611165509495)


```
1. colorPrimary: Toolbar导航栏的底色。 
1. colorPrimaryDark：状态栏的底色，注意这里只支持Android5.0以上的手机。 
1. textColorPrimary：整个当前Activity的字体的默认颜色。 
1. android:windowBackground：当前Activity的窗体颜色。 
1. colorAccent：CheckBox，RadioButton，SwitchCompat等控件的点击选中颜色 
1. colorControlNormal：CheckBox，RadioButton，SwitchCompat等默认状态的颜色。 
1. colorButtonNormal：默认状态下Button按钮的颜色。 
1. editTextColor：默认EditView输入框字体的颜色。
```
参考文章链接：http://blog.csdn.net/feiduclear_up/article/details/46457433

## TabLayout(选项卡布局)
实现Tabs选项卡的效果有很多方法，例如，我一般使用PageSlidingTab来完成，但是，现在谷歌中有一个TabLayout，我们以后直接就可以使用此布局来直接完成选项卡的效果，不管是使用自定义的View还是使用谷歌给的TabLayout，他们都是继承自HorizontalScrollView的。

先看一下效果：
![image](http://ww1.sinaimg.cn/mw690/b0d9a523jw1fa7musle38g20ah0hbgps.gif)

布局activity_tablayout.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <android.support.design.widget.TabLayout
        android:id="@+id/tab"
        android:layout_width="match_parent"
        android:layout_height="40dp"
        app:tabBackground="@color/colorAccent"
        app:tabIndicatorColor="@android:color/holo_blue_bright"
        app:tabMode="scrollable"
        app:tabSelectedTextColor="@android:color/holo_blue_bright"
        app:tabTextColor="@android:color/white"
        ></android.support.design.widget.TabLayout>

    <android.support.v4.view.ViewPager
        android:id="@+id/vpContent"
        android:layout_width="match_parent"
        android:layout_height="match_parent"></android.support.v4.view.ViewPager>
</LinearLayout>
```
布局文件很简单，只有TabLayout和ViewPager两个控件，但在使用TabLayout的属性的时候，需要引入命名空间 xmlns:app="http://schemas.android.com/apk/res-auto"。

Adapter代码TabFragmentAdapter.java：

```
public class TabFragmentAdapter extends FragmentStatePagerAdapter {

    private List<Fragment> mFragments;
    private List<String> mTitles;

    public TabFragmentAdapter(FragmentManager fm,List<Fragment> mFragments,List<String> mTitles) {
        super(fm);
        this.mFragments = mFragments;
        this.mTitles = mTitles;
    }

    @Override
    public Fragment getItem(int position) {
        return mFragments.get(position);
    }

    @Override
    public int getCount() {
        return mFragments.size();
    }

    @Override
    public CharSequence getPageTitle(int position) {
        return mTitles.get(position);
    }
}
```
主程序TabLayoutActivity.java

```
public class TabLayoutActivity extends AppCompatActivity {

    TabLayout tab;
    ViewPager vpContent;
    List<Fragment> fragmentList;
    List<String> mTitles;
    TabFragmentAdapter mTabFragmentAdapter;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_tablayout);
        tab = (TabLayout) findViewById(R.id.tab);

        vpContent = (ViewPager) findViewById(R.id.vpContent);
        fragmentList = new ArrayList<>();
        mTitles = new ArrayList<>();

        mTitles.add("one");
        mTitles.add("two");
        mTitles.add("three");
        mTitles.add("four");
        mTitles.add("five");
        mTitles.add("six");
        mTitles.add("severn");
        mTitles.add("eight");
        mTitles.add("night");
        mTitles.add("ten");

        tab.addTab(tab.newTab().setText(mTitles.get(0)));
        tab.addTab(tab.newTab().setText(mTitles.get(1)));
        tab.addTab(tab.newTab().setText(mTitles.get(2)));
        tab.addTab(tab.newTab().setText(mTitles.get(3)));
        tab.addTab(tab.newTab().setText(mTitles.get(4)));
        tab.addTab(tab.newTab().setText(mTitles.get(5)));
        tab.addTab(tab.newTab().setText(mTitles.get(6)));
        tab.addTab(tab.newTab().setText(mTitles.get(7)));
        tab.addTab(tab.newTab().setText(mTitles.get(8)));
        tab.addTab(tab.newTab().setText(mTitles.get(9)));
        for (int i = 0; i < mTitles.size(); i++) {
            fragmentList.add(TabFragment.newInstance("hello " + mTitles.get(i)));
        }
        mTabFragmentAdapter = new TabFragmentAdapter(getSupportFragmentManager(), fragmentList, mTitles);
        vpContent.setAdapter(mTabFragmentAdapter);
        tab.setupWithViewPager(vpContent);
    }
}

```
主程序中用到的TabFragment.java程序就非常简单了，通过效果图就知道怎么做了，这里就不多说明了。

##### -说明：

TabLayout常用的属性有：

1. app:tabSelectedTextColor：Tab被选中字体的颜色
1. app:tabTextColor：Tab未被选中字体的颜色
1. app:tabIndicatorColor：Tab指示器下标的颜色

TabLayout常用的方法如下： 
- addTab(TabLayout.Tab tab, int position, boolean setSelected) 增加选项卡到 layout 中 
- addTab(TabLayout.Tab tab, boolean setSelected) 同上 
- addTab(TabLayout.Tab tab) 同上 
- getTabAt(int index) 得到选项卡 
- getTabCount() 得到选项卡的总个数 
- getTabGravity() 得到 tab 的 Gravity 
- getTabMode() 得到 tab 的模式 
- getTabTextColors() 得到 tab 中文本的颜色 
- newTab() 新建个 tab 
- removeAllTabs() 移除所有的 tab 
- removeTab(TabLayout.Tab tab) 移除指定的 tab 
- removeTabAt(int position) 移除指定位置的 tab 
- setOnTabSelectedListener(TabLayout.OnTabSelectedListener onTabSelectedListener) 为每个 tab 增加选择监听器 
- setScrollPosition(int position, float positionOffset, boolean updateSelectedText) 设置滚动位置 
- setTabGravity(int gravity) 设置 Gravity 
- setTabMode(int mode) 设置 Mode,有两种值：TabLayout.MODE_SCROLLABLE和TabLayout.MODE_FIXED分别表示当tab的内容超过屏幕宽度是否支持横向水平滑动，第一种支持滑动，第二种不支持，默认不支持水平滑动。 
- setTabTextColors(ColorStateList textColor) 设置 tab 中文本的颜色 
- setTabTextColors(int normalColor, int selectedColor) 设置 tab 中文本的颜色 默认 选中 
- setTabsFromPagerAdapter(PagerAdapter adapter) 设置 PagerAdapter 
- setupWithViewPager(ViewPager viewPager) 和 ViewPager 联动 

**最后要强调的是，TabLayout要配合Theme.AppCompat。。。。。。来使用，否则会报错的哦，不明白的可以试一下**

## AppBarLayout（程序栏布局）&&CoordinatorLayout（协作布局）

AppBarLayout继承自LinearLayout，它是为了Material Design设计的App Bar，支持手势滑动操作，默认的AppBarLayout是**垂直方向**的，它的作用是把AppBarLayout包裹的内容都作为AppBar，AppBarLayout是支持手势滑动效果的，不过要跟CoordinatorLayout配合使用。

从源码可以看到，CoordinatorLayout是一个增强型的FrameLayout。它的作用有两个：

1. 作为一个布局的根布局
1. 作为一个子视图之间相互协调手势效果的一个协调布局

首先来看布局文件layout_appbar.xml

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/coor_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <android.support.design.widget.AppBarLayout
        android:id="@+id/appbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            app:layout_scrollFlags="scroll|enterAlways">

        </android.support.v7.widget.Toolbar>

        <android.support.design.widget.TabLayout
            android:id="@+id/tabs"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabGravity="fill"
            app:tabIndicatorColor="@color/colorAccent"
            app:tabSelectedTextColor="@color/colorAccent"
            app:tabTextColor="@color/colorPrimary"></android.support.design.widget.TabLayout>

    </android.support.design.widget.AppBarLayout>
    <!--可滑动的布局内容-->
    <android.support.v4.view.ViewPager
        android:id="@+id/vpContent"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior" />
</android.support.design.widget.CoordinatorLayout>
```
说明：

CoordinatorLayout协调布局中包裹了两个布局，一个是ViewPager，一个是AppBarLayout

我们来看看CoordinatorLayout是怎么来协调这两个子视图手势操作的:

1.由于CoordinatorLayout是FrameLayout布局，我们可以通过

```
android:layout_gravity="bottom|end"
```
属性来控制组件在整个布局中的位置。

2.为了达到上面效果图的手势动画效果，我们必须做如下设置，通过app:layout_scrollFlags=”scroll|enterAlways” 属性来确定哪个组件是可滑动的

设置的layout_scrollFlags有如下几种选项：

1. scroll: 所有想滚动出屏幕的view都需要设置这个flag- 没有设置这个flag的view将被固定在屏幕顶部。
1. enterAlways: 这个flag让任意向下的滚动都会导致该view变为可见，启用快速“返回模式”。
1. enterAlwaysCollapsed: 当你的视图已经设置minHeight属性又使用此标志时，你的视图只能已最小高度进入，只有当滚动视图到达顶部时才扩大到完整高度。
1. exitUntilCollapsed: 滚动退出屏幕，最后折叠在顶端。

我们上面的布局中 给Toolbar设置了app:layout_scrollFlags属性，因此，Toolbar是可以滚动出屏幕，且向下滚动有可以出现。

3.为了使得Toolbar可以滑动，我们必须还得有个条件，就是CoordinatorLayout布局下包裹一个可以滑动的布局，比如 RecyclerView，NestedScrollView(经过测试，ListView，ScrollView不支持)具有滑动效果的组件。并且给这些组件设置如下属性来告诉CoordinatorLayout，该组件是带有滑动行为的组件，然后CoordinatorLayout在接受到滑动时会通知AppBarLayout 中可滑动的Toolbar可以滑出屏幕了。

```
app:layout_behavior="@string/appbar_scrolling_view_behavior"
```

4.实际开发中发现一个问题，当AppBarLayout的高度太高的时，滑动也是有问题的，不懂的码友可以试试。

主程序：AppBarLayoutActivity.java：

```
public class AppBarLayoutActivity extends AppCompatActivity {

    private CoordinatorLayout coor_main;
    private AppBarLayout appbar;
    private Toolbar toolbar;
    private TabLayout tab;
    private ViewPager vpContent;
    List<Fragment> fragmentList;
    List<String> mTitles;
    TabFragmentAdapter mTabFragmentAdapter;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.layout_appbar);
        coor_main = (CoordinatorLayout) findViewById(R.id.coor_main);
        appbar = ((AppBarLayout) findViewById(R.id.appbar));
        toolbar = ((Toolbar) findViewById(R.id.toolbar));
        tab = ((TabLayout) findViewById(R.id.tabs));
        vpContent = ((ViewPager) findViewById(R.id.vpContent));
        initToolBar();
        initTab();
    }

    private void initToolBar() {
        setSupportActionBar(toolbar);
        getSupportActionBar().setDisplayShowTitleEnabled(false);
        toolbar.setTitle("风清扬");
        toolbar.setLogo(R.mipmap.ic_launcher);
    }

    private void initTab() {
        fragmentList = new ArrayList<>();
        mTitles = new ArrayList<>();

        mTitles.add("one");
        mTitles.add("two");
        mTitles.add("three");
        mTitles.add("four");

        tab.addTab(tab.newTab().setText(mTitles.get(0)));
        tab.addTab(tab.newTab().setText(mTitles.get(1)));
        tab.addTab(tab.newTab().setText(mTitles.get(2)));
        tab.addTab(tab.newTab().setText(mTitles.get(3)));
        for (int i = 0; i < mTitles.size(); i++) {
            fragmentList.add(AppBarFragment.newInstance(""+i));
        }
        mTabFragmentAdapter = new TabFragmentAdapter(getSupportFragmentManager(), fragmentList, mTitles);
        vpContent.setAdapter(mTabFragmentAdapter);
        tab.setupWithViewPager(vpContent);
    }
}

```

说明：
因为用到了ToolBar，所以需要去掉APP中默认的ActionBar，即需要给此Activity添加theme，如下
```
<style name="AppThemeB" parent="Theme.AppCompat.Light.NoActionBar"></style>
```
里面别的代码都比较简单，将会在下面奉上源代码。


**总结**： 为了使得Toolbar有滑动效果，必须做到如下三点：

1. CoordinatorLayout必须作为整个布局的父布局容器。
1. 给需要滑动的组件设置 app:layout_scrollFlags=”scroll|enterAlways” 属性。
1. 给你的可滑动的组件，也就是RecyclerView 或者 NestedScrollView 设置如下属性：
app:layout_behavior="@string/appbar_scrolling_view_behavior"

## CollapsingToolbarLayout（折叠工具栏布局）

CollapsingToolbarLayout包裹 Toolbar 的时候提供一个可折叠的 Toolbar，一般作为AppbarLayout的子视图使用。

效果如下：
![image](http://ww3.sinaimg.cn/mw690/b0d9a523jw1fa7ypvd0trg20ah0hbdtb.gif)

布局文件activity_collapsing.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <android.support.design.widget.AppBarLayout
        android:id="@+id/appbar"
        android:layout_width="match_parent"
        android:layout_height="160dp">

        <android.support.design.widget.CollapsingToolbarLayout
            android:id="@+id/collapsing_toolbar"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:contentScrim="?attr/colorPrimary"
            app:expandedTitleMarginEnd="64dp"
            app:expandedTitleMarginStart="48dp"
            app:layout_scrollFlags="scroll|exitUntilCollapsed"
            app:statusBarScrim="?attr/colorPrimary"

            >

            <ImageView
                android:id="@+id/image"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:scaleType="fitXY"
                android:src="@mipmap/html5"
                app:layout_collapseMode="parallax"
                app:layout_collapseParallaxMultiplier="0.6" />

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:layout_collapseMode="pin"
                >
                <TextView
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:gravity="left|center_vertical"
                    android:textColor="#ffffff"
                    android:text="自定义标题"

                    />
            </android.support.v7.widget.Toolbar>
        </android.support.design.widget.CollapsingToolbarLayout>
    </android.support.design.widget.AppBarLayout>

    <!--可滑动的内容-->
    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">

            <Button
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_margin="5dp"
                android:text="one" />

        ......若干个button......

        </LinearLayout>
    </android.support.v4.widget.NestedScrollView>
</android.support.design.widget.CoordinatorLayout>

```
主程序CollapsingToolbarLayoutActivity.java：

```
public class CollapsingToolbarLayoutActivity extends AppCompatActivity {
    private Toolbar toolbar;
    private CollapsingToolbarLayout collapsingToolbar;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_collapsing);
        toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        getSupportActionBar().setDisplayShowTitleEnabled(false);
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
        collapsingToolbar = (CollapsingToolbarLayout) findViewById(R.id.collapsing_toolbar);
//        collapsingToolbar.setTitle("bruce常");
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        if (item.getItemId() == android.R.id.home) {

            Toast.makeText(this, "aaa", Toast.LENGTH_SHORT).show();
            return true;
        }
        return super.onOptionsItemSelected(item);
    }
}

```
说明：

CollapsingToolbarLayout 提供以下属性和方法是用：

1. Collapsing title：ToolBar的标题，当CollapsingToolbarLayout全屏没有折叠时，title显示的是大字体，在折叠的过程中，title不断变小到一定大小的效果。你可以调用setTitle(CharSequence)方法设置title。
1. Content scrim：ToolBar被折叠到顶部固定时候的背景，你可以调用setContentScrim(Drawable)方法改变背景或者 在属性中使用 app:contentScrim=”?attr/colorPrimary”来改变背景。
1. Status bar scrim：状态栏的背景，调用方法setStatusBarScrim(Drawable)。还没研究明白，不过这个只能在Android5.0以上系统有效果。
1. Parallax scrolling children：CollapsingToolbarLayout滑动时，子视图的视觉差，可以通过属性app:layout_collapseParallaxMultiplier=”0.6”改变。
1. CollapseMode ：子视图的折叠模式，有两种“pin”：固定模式，在折叠的时候最后固定在顶端；“parallax”：视差模式，在折叠的时候会有个视差折叠的效果。我们可以在布局中使用属性app:layout_collapseMode=”parallax”来改变。

特别注意的是要使用NoActionBar的主题，且AppBarLayout的布局超过手机屏幕的时候，滑动也是有问题的。 

## NestedScrollView的使用:

NestedScrollView是support-v4包提供的控件,继承至FrameLayout, 
并实现了NestedScrollingParent,NestedScrollingChild, ScrollingView接口. 
它的作用类似于Android.widget.ScrollView,不同点在于NestedScrollView支持嵌套滑动.

效果图如下：

![image](http://ww1.sinaimg.cn/mw690/b0d9a523jw1fa7yqvx1fug20ah0hbk3t.gif)

布局文件activity_netscroll.xml：

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    android:id="@+id/main_content"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".NestedScrollViewActivity">


    <android.support.design.widget.AppBarLayout
        android:id="@+id/appbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <!--标题栏-->
        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            app:layout_scrollFlags="scroll|enterAlways">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:layout_gravity="center"
                android:gravity="center"
                android:text="自定义标题"/>
        </android.support.v7.widget.Toolbar>
        <!--大图-->
        <ImageView
            android:layout_width="match_parent"
            android:layout_height="200dp"
            android:background="@mipmap/html5"

            />
        <!--选项卡-->
        <android.support.design.widget.TabLayout
            android:id="@+id/tabLayout"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="#80ffffff"
            app:tabIndicatorColor="@color/colorAccent"
            app:tabMode="scrollable"
            app:tabSelectedTextColor="@color/colorAccent"
            app:tabTextColor="@android:color/black"/>
    </android.support.design.widget.AppBarLayout>

    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:overScrollMode="never"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <LinearLayout
            android:id="@+id/ll_sc_content"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical">
        </LinearLayout>
    </android.support.v4.widget.NestedScrollView>
</android.support.design.widget.CoordinatorLayout>
```
主程序NestedScrollViewActivity.java：

```
public class NestedScrollViewActivity extends AppCompatActivity {
    List<String> mData;
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_netscroll);
        initData(1);
        initView();
    }

    private void initData(int pager) {
        mData = new ArrayList<>();
        for (int i = 1; i < 50; i++) {
            mData.add("pager" + pager + " 第" + i + "个item");
        }
    }

    private void initView() {
        //设置ToolBar
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        toolbar.setTitle("");
        toolbar.setNavigationIcon(R.mipmap.ic_launcher);
        setSupportActionBar(toolbar);//替换系统的actionBar

        //设置TabLayout
        TabLayout tabLayout = (TabLayout) findViewById(R.id.tabLayout);
        for (int i = 1; i < 20; i++) {
            tabLayout.addTab(tabLayout.newTab().setText("TAB" + i));
        }
        //TabLayout的切换监听
        tabLayout.setOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
            @Override
            public void onTabSelected(TabLayout.Tab tab) {
                initData(tab.getPosition() + 1);
                setScrollViewContent();
            }

            @Override
            public void onTabUnselected(TabLayout.Tab tab) {

            }

            @Override
            public void onTabReselected(TabLayout.Tab tab) {

            }
        });
        setScrollViewContent();
    }

    /**
     * 刷新ScrollView的内容
     */
    private void setScrollViewContent() {
        //NestedScrollView下的LinearLayout
        LinearLayout layout = (LinearLayout) findViewById(R.id.ll_sc_content);
        layout.removeAllViews();
        for (int i = 0; i < mData.size(); i++) {
            View view = View.inflate(NestedScrollViewActivity.this, R.layout.adapter_appbar, null);
            ((TextView) view.findViewById(R.id.text)).setText(mData.get(i));
            //动态添加 子View
            layout.addView(view, i);
        }
    }

}

```
5.0中的新特性基本上就这么多，希望以后开发中多多使用。



本篇文章源码下载地址：[点我](https://github.com/201216323/Test5.0)

欢迎访问201216323.tech来查看我的CSDN博客。

欢迎关注我的个人技术公众号,快速查看我的最新文章。

![我的公众号图片](http://img.blog.csdn.net/20161220174646569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2NnXzIwMTIxNjMyMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "bruce常")


参考链接：
1. http://blog.csdn.net/feiduclear_up/article/details/46514791
2. http://blog.csdn.net/mchenys/article/details/51541306