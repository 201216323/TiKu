Android5.0出来已经老长时间了，但是这里面好多控件小编使用的还不是很熟练，本篇文章就主要学习TextInputLayout和Snackbar。

### TextInputLayout&&Snackbar

#### 1：来看谷歌官方给出的TextInputLayout的定义。
这里删除了一些作用不大的代码，只留下来小编需要说明的。

```
package android.support.design.widget;

/**
 * Layout which wraps an {@link android.widget.EditText} (or descendant) to show a floating label
 * when the hint is hidden due to the user inputting text.
 */
public class TextInputLayout extends LinearLayout {

    private static final int ANIMATION_DURATION = 200;
    private static final int INVALID_MAX_LENGTH = -1;

    private static final String LOG_TAG = "TextInputLayout";

    private final FrameLayout mInputFrame;

```
从谷歌的官方程序中可以得到：TextInputLayout继承自LinearLayout，且在android.support.design包下，因此要想使用此控件必须要在程序中导入design包。如
```
compile 'com.android.support:design:24+'
```

根据上面的英文解释可以知道，TextInputLayout可以在Edittext的提示信息隐藏的时候显示一个浮动的标签，现在好多APP的登陆页面都是这样设计的，今天小编就详细使用以下此控件，来看运行结果。

![image](http://ww3.sinaimg.cn/mw690/b0d9a523jw1fa323dgwjsg20aa0hg41o.gif)

##### 首先：
新建一个EmptyActivity的项目，小编这里命名是TestTextInputLayout，并在build.gradle(Mode app)中添加design包的依赖。
```
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
        exclude group: 'com.android.support', module: 'support-annotations'
    })
    compile 'com.android.support:appcompat-v7:24.2.1'
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:design:24+'
}
```
##### 其次：
在activity_main.xml中写效果图的布局文件

最外层布局需要加上：
xmlns:app="http://schemas.android.com/apk/res-auto"才能使用TextInputLayout的一些属性。

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:focusable="true"
    android:focusableInTouchMode="true"
    android:gravity="center"
    android:orientation="vertical"
    tools:context="bruce.chang.testtextinputlayout.MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"
        android:background="@drawable/mine_login_editor_bg"
        android:orientation="vertical"
        >

        <android.support.design.widget.TextInputLayout
            android:id="@+id/usernameWrapper"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="5dp"
            >

            <EditText
                android:id="@+id/username"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:drawableLeft="@mipmap/mine_login_account"
                android:drawablePadding="10dp"
                android:hint="Username"
                android:inputType="textEmailAddress"
                android:textColor="#333333"
                android:textSize="14sp"/>

        </android.support.design.widget.TextInputLayout>

        <android.support.design.widget.TextInputLayout
            android:id="@+id/passwordWrapper"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="5dp"
            app:counterEnabled="false"
            app:counterMaxLength="10"
            app:counterOverflowTextAppearance="@android:style/TextAppearance.DeviceDefault.Small"
            app:hintAnimationEnabled="true"
            app:passwordToggleEnabled="true"
            >

            <EditText
                android:id="@+id/password"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:drawableLeft="@mipmap/mine_login_password"
                android:drawablePadding="10dp"
                android:hint="Password"
                android:inputType="textPassword"
                android:textColor="#333333"

                android:textSize="14sp"/>

        </android.support.design.widget.TextInputLayout>


    </LinearLayout>

    <TextView
        android:id="@+id/tvLogin"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"
        android:background="@color/colorPrimary"
        android:gravity="center"
        android:padding="10dp"
        android:text="login"
        android:textColor="#ffffff"
        />

</LinearLayout>

```
这里在最外层的LinearLayout中通过 **android:focusable="true"** 和
    **android:focusableInTouchMode="true"**来获得焦点。每一个 **TextInputLayout**中 **只能**包含一个 **EditText** 的控件，这里可以简单设置一下EditText的属性。
    
在TextInputLayout中，重要的属性有以下几个：

> 1. counterEnabled：是否启用计数器
> 1. counterMaxLength：启用计数器时，最大字数限制（仅仅用做显示）
> 1. counterOverflowTextAppearance：当字数超出计数器的最大限制时的字体格式
> 1. hintAnimationEnabled：是否启用hint动画效果
> 1. errorEnabled：是否显示错误信息
> 1. errorTextAppearance：错误信息的字体格式
> 1. app:passwordToggleEnabled="true" 查看密码开关是否启用，默认是启用的

小编虽然在第二个**TextInputLayout**中添加了这些属性，但因为设置了 **app:counterEnabled="false"** 这个属性，所以不会有计数器的效果。这里的两个 **EditText**都没有设置其**background**，如果设置了背景颜色，当输入错误的时候将会有不一样的提示效果。最后还有要给模拟登陆按钮的 **TextView**,用以在其点击事件中处理上面的两个 **EditText**。

##### 最后：
MainActivity.java代码参考附件1，下面说明实现过程.

###### 第一步：初始化View，代码中设置一下提示信息：

```
usernameWrapper = (TextInputLayout) findViewById(R.id.usernameWrapper);
passwordWrapper = (TextInputLayout) findViewById(R.id.passwordWrapper);
tvLogin = (TextView) findViewById(R.id.tvLogin);
tvLogin.setOnClickListener(this);
activity_main = (LinearLayout) findViewById(R.id.activity_main);
activity_main.setOnClickListener(this);
usernameWrapper.setHint("用户名");
passwordWrapper.setHint("密码");
```
###### 第二步：编写一个判断EditText输入信息的方法，这里仅仅判断输入的字符串的长度，当长度大于5的时候才认定为合法的字符串。


```
  public boolean validatePassword(String password) {
        return password.length() > 5;
}
```

###### 第三步：编写点击login时候调用的方法，弹出一个提示信息，Toast的形式我们用的很多了，这里使用Snackbar来弹出提示信息。


```
public void doLogin() {
        Snackbar.make(activity_main, "you have login,", Snackbar.LENGTH_SHORT)
                .show();

}
```
###### 第四步：编写一个隐藏软键盘的方法，点击空白处，收起软键盘，这一点没有太大必要，但是小编个人习惯而已。

```
private void hideKeyboard() {
        View view = getCurrentFocus();
        if (view != null) {
            ((InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE)).
                    hideSoftInputFromWindow(view.getWindowToken(), InputMethodManager.HIDE_NOT_ALWAYS);
        }
}
```
###### 第五步：在onclick方法中根据id来调用不同的事件，这里通过 **TextInputLayout** 的 **setError** 方法来设置错误的提示信息，通过 **setErrorEnabled** 方法来清楚相应的错误提示信息。


```
public void onClick(View v) {
        switch (v.getId()) {
            case R.id.tvLogin:
                hideKeyboard();
                String username = usernameWrapper.getEditText().getText().toString();
                String password = passwordWrapper.getEditText().getText().toString();

                if (!validatePassword(username)) {
                    usernameWrapper.setError("Not a valid email address!");
                    usernameWrapper.getEditText().getText().clear();
                } else if (!validatePassword(password)) {
                    passwordWrapper.setError("Not a valid password!");//可以通过setError()显示输入错误提示，
                    passwordWrapper.getEditText().getText().clear();//输入错误的时候，清空此EditText
                } else {
                    usernameWrapper.setErrorEnabled(false);//setErrorEnabled(fasle)清除错误提示
                    passwordWrapper.setErrorEnabled(false);
                    doLogin();
                    break;

                }
            case R.id.activity_main:

                hideKeyboard();
                break;
        }
}
```
###### 此时的运行结果如下图：
![image](http://ww1.sinaimg.cn/mw690/b0d9a523jw1fa341h8u42g20an0i7aev.gif)



上图就是运行的结果，看到上面的结果有以下几个疑问：
1. 输入错误的时候的提示信息的颜色(A)、输入时候浮动lable的颜色(B)、以及Edittext默认的背景（即那条线）的颜色(C)为什么是红色的？
2. Snackbar弹出的时候背景为什么是黑色的，能否修改这个黑色的背景颜色？
3.如果新建可以侧滑的Activity，当弹出Snackbar的时候可以向右滑动关闭，为什么这里不可以？

解决疑问1：


```
经过在网上查找一番，发现提示信息的颜色与系统所在的Style有关，默认的Style如下：

<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
</style>

1中ABC的颜色都是默认的colorAccent的颜色，但修改此颜色，BC会发生变化，A是不会发生变化的。
新建一个Style:
<style name="AppThemeA" parent="Theme.AppCompat.Light.NoActionBar">
        <item name="colorPrimary">#00ff00</item>
        <item name="colorPrimaryDark">#0000ff</item>
        <item name="colorAccent">#333333</item>
</style>
当时用AppThemeA作为APP的style的时候，1中颜色B和颜色C变成了#333333的颜色，A的颜色仍然是AppTheme中的colorAccent的颜色，

```
解决疑问2：

```
Snackbar默认的颜色就是黑色的，但可以修改成任意的颜色，接下来将会详细的说明Snackbar的使用，如何修改Snackbar的字体颜色和背景颜色。

```

解决疑问3：

```
要想支持Swipe手势的话,这个view需要是一个CoordinatorLayout,默认的带侧边栏的Activity中布局里面就有CoordinatorLayout,而这里使用的是LinearLayout所以不支持向右滑动关闭Snackbar。
```

##### 修改Snackbar的自己颜色和背景颜色

参考文章链接

http://www.jcodecraeer.com/plus/view.php?aid=3187

##### 字体颜色设置

对于Action可以通过Snack的bar的公开API
**snackbar.setActionTextColor(int color)**
设置。

但是使用的时候不太好用，没有找到设置消息文字颜色的API，但是在查看Snackbar.class的时候找到了一个方法:

```
 public View getView() {
        return this.mView;
 }
```
当去查看setActionTextColor(int color)的时候，发现了这个方法也被调用了，用于获取Snackbar的用于显示Action的TextView实例。继续查看，可以发现，每一个Snackbar的内容通过其内部类SnackbarLayout来呈现，其具体声明如下:


```
public static class SnackbarLayout extends LinearLayout
```
而在Snackbar(ViewGroup parent)这个包级声明的方法中发现上面的getView()返回的mView就是SnackbarLayout实例，这个类的布局最终是

```
design_layout_snackbar_include.xml
```

，这个是Android Support Design Library库中的布局文件,具体内容如下:

```
<merge xmlns:android="http://schemas.android.com/apk/res/android">

    <TextView
            android:id="@+id/snackbar_text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:paddingTop="@dimen/snackbar_padding_vertical"
            android:paddingBottom="@dimen/snackbar_padding_vertical"
            android:paddingLeft="@dimen/snackbar_padding_horizontal"
            android:paddingRight="@dimen/snackbar_padding_horizontal"
            android:textAppearance="@style/TextAppearance.Design.Snackbar.Message"
            android:maxLines="@integer/snackbar_text_max_lines"
            android:layout_gravity="center_vertical|left|start"
            android:ellipsize="end"/>

    <TextView
            android:id="@+id/snackbar_action"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="@dimen/snackbar_extra_spacing_horizontal"
            android:layout_marginStart="@dimen/snackbar_extra_spacing_horizontal"
            android:layout_gravity="center_vertical|right|end"
            android:background="?attr/selectableItemBackground"
            android:paddingTop="@dimen/snackbar_padding_vertical"
            android:paddingBottom="@dimen/snackbar_padding_vertical"
            android:paddingLeft="@dimen/snackbar_padding_horizontal"
            android:paddingRight="@dimen/snackbar_padding_horizontal"
            android:visibility="gone"                    android:textAppearance="@style/TextAppearance.Design.Snackbar.Action"/>
</merge>
```
其实到这里，剩下的问题就可以很快解决了。从上面可以看出来，具体Snackbar的内容就是布局中的两个TextView,因此只要能够获取这两个个TextView实例，那么修改属性就非常简单了。而上面的getView()则解决了上面这个问题，因此可以写下面的帮助方法实现设置消息文本的颜色。

```
public void setSnackbarMessageTextColor(Snackbar snackbar, int color) {
        View view = snackbar.getView();
        ((TextView) view.findViewById(R.id.snackbar_text)).setTextColor(color);
}
```

使用下面的代码调用：


```
Snackbar snackbar = Snackbar.make(container, "SnackbarTest", Snackbar.LENGTH_LONG)
                .setAction("Action", new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                       //点击事件
                    }
                });
        setSnackbarMessageTextColor(snackbar, Color.parseColor("#FFFFFF"));
        snackbar.show();
```

setAction方法的源代码：

```
 /**
     * Set the action to be displayed in this {@link Snackbar}.
     *
     * @param text     Text to display
     * @param listener callback to be invoked when the action is clicked
     */
    @NonNull
    public Snackbar setAction(CharSequence text, final View.OnClickListener listener) {
        final TextView tv = mView.getActionView();

        if (TextUtils.isEmpty(text) || listener == null) {
            tv.setVisibility(View.GONE);
            tv.setOnClickListener(null);
        } else {
            tv.setVisibility(View.VISIBLE);
            tv.setText(text);
            tv.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    listener.onClick(view);
                    // Now dismiss the Snackbar
                    dispatchDismiss(Callback.DISMISS_EVENT_ACTION);
                }
            });
        }
        return this;
    }
```
从上面源代码可以看到，在设置action的点击事件的时候我们没有写任何的事件处理操作，但是从源码中可以看到默认执行了让snackBar消失的操作,所以当在使用的过程中才会发现点击Action的文字，Snackbar将会消失。


##### 如何为Snackbar添加背景颜色

可以通过getView() 方法获取Snackbar的核心视图，然后就可以设置颜色了，当然也可以设置字体的颜色,注意设置字体颜色的时候的id必须是snackbar_text。。

比如：
```
snackbar.getView().setBackgroundColor(colorId);
snackbar.getView().findViewById(R.id.snackbar_text)).setTextColor(colorId)
```
下面附件2是ColoredSnackbar类，它封装了一些方法，可以根据用户指定的类型显示不同背景颜色。

如何使用呢？

```
Snackbar snackbar = Snackbar.make("对应的view", R.string.hello_snackbar, Snackbar.LENGTH_SHORT);
ColoredSnackBar.alert(snackbar).show();
```

###### Snackbar的特点：
> 1. 当它显示一段时间后或用户与屏幕交互时它会自动消失。
> 1. 可以自定义action-可选操作。
> 1. swiping it off the screen可以让其消失
> 1. 它是context sensitive message(自己理解吧),所以这些消息是UI screen的一部分并且它是显示在所有屏幕其它元素之上(屏幕最顶层)，并不是像Toast一样覆盖在屏幕上。
> 1. 同一时间只能显示一个snackbar。



> 本篇文章，小编主要是使用TextInputLayout和Snackbar，Android7.0都发布很长世间了，作为开发者不能还停留在5.0之前的控件上，练习使用这些新的控件不仅仅提高自己的技术，更重要可以接触新的Android知识，开发将会变得更简单，小编说了这么多，最重要的还是自己动手操作一下。






附件1：

```
package bruce.chang.testtextinputlayout;

import android.content.Context;
import android.os.Bundle;
import android.support.design.widget.Snackbar;
import android.support.design.widget.TextInputLayout;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.view.inputmethod.InputMethodManager;
import android.widget.LinearLayout;
import android.widget.TextView;


public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    TextInputLayout usernameWrapper;
    TextInputLayout passwordWrapper;
    TextView tvLogin;
    LinearLayout activity_main;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        usernameWrapper = (TextInputLayout) findViewById(R.id.usernameWrapper);
        passwordWrapper = (TextInputLayout) findViewById(R.id.passwordWrapper);
        tvLogin = (TextView) findViewById(R.id.tvLogin);
        tvLogin.setOnClickListener(this);
        activity_main = (LinearLayout) findViewById(R.id.activity_main);
        activity_main.setOnClickListener(this);
        usernameWrapper.setHint("用户名");
        passwordWrapper.setHint("密码");

    }

    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.tvLogin:
                hideKeyboard();
                String username = usernameWrapper.getEditText().getText().toString();
                String password = passwordWrapper.getEditText().getText().toString();

                if (!validatePassword(username)) {
                    usernameWrapper.setError("Not a valid email address!");
                    usernameWrapper.getEditText().getText().clear();
                } else if (!validatePassword(password)) {
                    passwordWrapper.setError("Not a valid password!");//可以通过setError()显示输入错误提示，
                    passwordWrapper.getEditText().getText().clear();//输入错误的时候，清空此EditText
                } else {
                    usernameWrapper.setErrorEnabled(false);//setErrorEnabled(fasle)清除错误提示
                    passwordWrapper.setErrorEnabled(false);
                    doLogin();
                    break;

                }
            case R.id.activity_main:

                hideKeyboard();
                break;
        }
    }

    public void doLogin() {
        Snackbar snackbar = Snackbar.make(usernameWrapper, "OK! I'm performing login.", Snackbar.LENGTH_LONG);
        snackbar.setAction("取消", new View.OnClickListener() {
            @Override
            public void onClick(View v) {

            }
        });
        snackbar.setActionTextColor(getResources().getColor(R.color.colorAccent));
        setSnackbarMessageTextColor(snackbar, getResources().getColor(R.color.colorAccent));
        snackbar.show();

    }

    private void hideKeyboard() {
        View view = getCurrentFocus();
        if (view != null) {
            ((InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE)).
                    hideSoftInputFromWindow(view.getWindowToken(), InputMethodManager.HIDE_NOT_ALWAYS);
        }
    }

    public boolean validatePassword(String password) {
        return password.length() > 5;
    }


    public void setSnackbarMessageTextColor(Snackbar snackbar, int color) {
        View view = snackbar.getView();
        view.setBackgroundColor(getResources().getColor(R.color.colorPrimaryDark));
        ((TextView) view.findViewById(R.id.snackbar_text)).setTextColor(color);
    }
}


```

附件2：
```
public class ColoredSnackbar {

    private static final int red = 0xfff44336;
    private static final int green = 0xff4caf50;
    private static final int blue = 0xff2195f3;
    private static final int orange = 0xffffc107;

    private static View getSnackBarLayout(Snackbar snackbar) {
        if (snackbar != null) {
            return snackbar.getView();
        }
        return null;
    }

    private static Snackbar colorSnackBar(Snackbar snackbar, int colorId) {
        View snackBarView = getSnackBarLayout(snackbar);
        if (snackBarView != null) {
            snackBarView.setBackgroundColor(colorId);
        }

        return snackbar;
    }

    public static Snackbar info(Snackbar snackbar) {
        return colorSnackBar(snackbar, blue);
    }

    public static Snackbar warning(Snackbar snackbar) {
        return colorSnackBar(snackbar, orange);
    }

    public static Snackbar alert(Snackbar snackbar) {
        return colorSnackBar(snackbar, red);
    }

    public static Snackbar confirm(Snackbar snackbar) {
        return colorSnackBar(snackbar, green);
    }
}
```