# NDK开发流程

不同版本的Android Studio可能对于NDK的配置是不一样的，本文记录我在AS2.2.2版本上的配置过程。

## 步骤1：安装配置NDK

（1）打开AS的Project Structure目录，如下:

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fbg2m80dcaj20s00bz41j.jpg)

点击Android NDK location 下方的Download来下载NDK，下载好的NDK会自动安装在SDK的目录里面。

![image](http://ww3.sinaimg.cn/mw690/b0d9a523jw1fbg2m87vkkj20lz0hfgns.jpg)

（2）配置path环境变量，将NDK的根目录配置到环境变量的path里面，如下是我的配置

```
C:\AndroidDevelop\sdk\ndk-bundle
```

## 步骤2：给AS配置关联NDK

（1）在项目的local.properties中添加配置ndk的根目录，默认情况下，经过步骤1的配置已经在local.properties中添加了ndk的根目录，如果没有手动添加，下面是我的配置


```
ndk.dir=C\:\\AndroidDevelop\\sdk\\ndk-bundle
```
（2）gradle.properties中添加配置，目的是兼容老的NDK。

```
android.useDeprecatedNdk=true
```

## 步骤3：编写native方法

我是在新建的类JniKit中创建的native方法

```
public class JniKit {

    /**
     * 定义native方法
     * 调用C代码对应的方法
     * @return
     */
    public native String sayHello();
}
```

## 步骤4：定义对应的JNI

（1）通过命令行，生成native方法对应的JNI函数声明头文件: 

在控制台或者Android studio自带的控制台使用javah -jni的命令，利用class文件生成.h头文件，这里要注意  javah -jni 在使用时有可能会出问题，比如找不到app.activity   找不到类文件，最后总结出来的就是严格按照以下来写：


![image](http://ww1.sinaimg.cn/mw690/b0d9a523jw1fbg40xzk0lj20qr0750vk.jpg)

（2）在项目jni文件夹下创建一个对应的函数文件：Hello.c


```
#include <com_bruce_chang_testndk_JniKit.h>

JNIEXPORT jstring JNICALL Java_com_bruce_chang_testndk_JniKit_sayHello
(JNIEnv *env, jobject jobj) {
    return (*env)->NewStringUTF(env, "Hello from C");
}
```

（3）在jni文件夹下创建一个空的C文件: empty.c。说明: 这是AS的bug, 必须至少2个C文件才能通过编译。


## 步骤5：指定编译的不同CPU。

在app的build.gradle的defaultConfig下添加如下代码：


```
defaultConfig {
        ndk{
            moduleName "Hello" //so文件: lib+moduleName+.so
            abiFilters "armeabi", "armeabi-v7a", "x86" //cpu的类型
        }
    }
```

## 步骤6：编译生成不同平台下的动态链接文件

（1）执行rebuild, 生成so文件

（2）so文件目录: build\intermediates\ndk\debug\lib\.....

## 步骤7：调用native方法

（1）在native方法所在的类JniKit.java中加载so文件


```
 static {
        System.loadLibrary("Hello");
    }
```

（2）在Activity中调用native方法:

```
 String s = new JniKit().sayHello();
tvNdk.setText(s);
```

以上就是NDK从下载到本地配置，以及实现简单一个测试程序的过程。

> 结果：显示C程序中返回的字符串"Hello from C"


![image](http://ww4.sinaimg.cn/mw690/b0d9a523jw1fbg4le5zbbj209406p0su.jpg)


> 总结：使用NDK的大致开发流程主要包含下面几步

1. 在java里面写native代码
2. 在main目录下创建jni目录，写C代码，生成头文件
3. 配置动态链接库的名称
4. 加载动态链接库
5. 代码具体调用

> 注意，使用c代码返回字符串类型的数据的时候，字符串类型是jstring，这里涉及到了java数据类型和C语言数据类型与本地JNI的一个对应关心，如下：


java类型 | JNI别名| C类型
:---:|:---:|:---:
boolean | jboolean| unsigned char
byte | jbyte| signed char
char | jchar| unsigned char
short | jshort| short
int | jint| int
long | jlong| long long
float | jfloat| float
double | jdouble| double


# 详解NDK开发
## Java调用C函数

> 重温一下生成头文件的方法

```
E:\GithubDownload\TestNDK\javacallc\src\main>javah -d jni -classpath C:\AndroidDevelop\sdk\platforms\android-25\android.jar;E:\GithubDownload\TestNDK\javacallc\build\intermediates\classes\debug bruce.chang.javacallc.Jni
```
> 下面四个小程序用到的java类


```
public class Jni {

    static {
        System.loadLibrary("javacallc");
    }

    /**
     * 求两个数字的和
     *
     * @param x
     * @param y
     * @return
     */
    public native int sum(int x, int y);

    /**
     * 将两个字符串拼接后返回
     *
     * @param s
     * @return
     */
    public native String sayHello(String s);

    /**
     * 将数组中的每个元素增加10
     *
     * @param intArray
     * @return
     */
    public native int[] increaseArrayEles(int[] intArray);

    /**
     *  应用: 检查密码是否正确, 如果正确返回200, 否则返回400
     "123456"
     * @param pwd
     * @return
     */
    public native int checkPwd(String pwd);
}
```

### 测试1：将传入的两个int值相加并返回


```
/**
    * 求两个数字的和
    *
    * @param x
    * @param y
    * @return
    */
JNIEXPORT jint JNICALL Java_bruce_chang_javacallc_Jni_sum
        (JNIEnv *env, jobject jobj, jint int1, jint int2) {
    //jint可以直接进行算术运算
    int sum = int1 + int2;
    //可直接将int类型数据作为jint返回
    return sum;
}
```


### 测试2：将两个字符串拼接后返回


```
/**
     * 将两个字符串拼接后返回
     *
     * @param s  I am BruceChang
     *        c  I am SWJ
     * @return  I am BruceChang add I am SWJ
     */
JNIEXPORT jstring JNICALL Java_bruce_chang_javacallc_Jni_sayHello
        (JNIEnv * env, jobject jobj, jstring js){
    //将jstring类型的js转换为char*类型数据
    char * fromJava = _JString2CStr(env,js);
    //c
    char * fromC = "add I am SWJ";
    //将拼接两个char*类型字符串拼接在第一个上
    strcat(fromJava, fromC);
    //将结果转换为jstring类型返回
    return (*env)->NewStringUTF(env, fromJava);
}
```

这里面用到一个函数_JString2CStr 将字符串转换成字符指针


```

/**
 * 工具函数
 * 把一个jstring转换成一个c语言的char* 类型.
 */
char* _JString2CStr(JNIEnv* env, jstring jstr) {

    char* rtn;
    jclass clsstring = (*env)->FindClass(env, "java/lang/String");
    jstring strencode = (*env)->NewStringUTF(env,"GB2312");
    jmethodID mid = (*env)->GetMethodID(env, clsstring, "getBytes", "(Ljava/lang/String;)[B");
    jbyteArray barr = (jbyteArray)(*env)->CallObjectMethod(env, jstr, mid, strencode); // String .getByte("GB2312");
    jsize alen = (*env)->GetArrayLength(env, barr);
    jbyte* ba = (*env)->GetByteArrayElements(env, barr, JNI_FALSE);
    if(alen > 0) {
        rtn = (char*)malloc(alen+1); //"\0"
        memcpy(rtn, ba, alen);
        rtn[alen]=0;
    }
    (*env)->ReleaseByteArrayElements(env, barr, ba,0);
    return rtn;
}
```


### 测试3：将数组的每个元素增加10


```
/**
    * 将数组中的每个元素增加10
    *
    * @param intArray
    * @return
    */
JNIEXPORT jintArray JNICALL Java_bruce_chang_javacallc_Jni_increaseArrayEles
        (JNIEnv * env, jobject jobj, jintArray arr){

    //1. 得到数组的长度
    //jsize       (*GetArrayLength)(JNIEnv*, jarray);
    jsize length = (*env)->GetArrayLength(env, arr);
    //2. 得到数组
    //jint*       (*GetIntArrayElements)(JNIEnv*, jintArray, jboolean*);
    jint* array = (*env)->GetIntArrayElements(env, arr, JNI_FALSE);
    //3. 遍历数组, 并将每个元素+10
    int i;
    for(i=0;i<length;i++) {
        *(array+i) += 10;
    }
    //4. 返回数组
    return arr;
}
```


### 测试4：检查密码是否正确


```
/**
 *  应用: 检查密码是否正确, 如果正确返回200, 否则返回400
 "123456"
 * @param pwd
 * @return
 */
JNIEXPORT jint JNICALL Java_bruce_chang_javacallc_Jni_checkPwd
        (JNIEnv * env, jobject jobj, jstring string){
    //1. 将jString转换为char*
    char* cs = _JString2CStr(env, string);
    char* pwd = "123456";
    //2. 比较两个字符串是否相等
    int result = strcmp(cs, pwd);
    //3. 根据比较的结果返回不同的值
    if(result==0) {
        return 200;
    }
    return 400;
}
```

> 结果：


![image](http://wx2.sinaimg.cn/mw690/b0d9a523ly1fbqzs8a37pj209y08smx5.jpg)

测试2，字符串的拼接，这个结果在不同的手机上显示效果还不太一样，在oppo r7上运行结果就是1、2、3、4、5，这个问题可能是程序不兼容导致的。

## C调用Java方法

C调用java方法和java调用C的方法步骤一样，但C回调java方法的核心思想是利用反射的原理，其次还需要用到java中的javap  -s命令来显示所有方法的签名信息，这个非常的重要，否则无法得到java类中的方法，从而也就无法实现C函数调用java方法。


> 显示方法签名的命令

```
E:\>cd E:\GithubDownload\TestNDK\ccalljava\src\main

E:\GithubDownload\TestNDK\ccalljava\src\main>javah -d jni -classpath C:\AndroidDevelop\sdk\platforms\android-23\android.jar;E:\GithubDownload\TestNDK\ccalljava\build\intermediates\classes\debug com.bruce.chang.ccalljava.Jni

E:\GithubDownload\TestNDK\ccalljava\src\main>javap -s -classpath C:\AndroidDevelop\sdk\platforms\android-23\android.jar;E:\GithubDownload\TestNDK\ccalljava\build\intermediates\classes\debug com.bruce.chang.ccalljava.Jni
Compiled from "Jni.java"
public class com.bruce.chang.ccalljava.Jni {
  public com.bruce.chang.ccalljava.Jni();
    descriptor: ()V

  public native void callbackHelloFromJava();
    descriptor: ()V

  public native void callbackAdd();
    descriptor: ()V

  public native void callbackPrintString();
    descriptor: ()V

  public native void callbackSayHello();
    descriptor: ()V

  public void helloFromJava();
    descriptor: ()V

  public int add(int, int);
    descriptor: (II)I

  public void printString(java.lang.String);
    descriptor: (Ljava/lang/String;)V

  public static void sayHello(java.lang.String);
    descriptor: (Ljava/lang/String;)V

  static {};
    descriptor: ()V
}

E:\GithubDownload\TestNDK\ccalljava\src\main>
```
> 一般调用步骤

1. 加载类得到class对象
2. 得到对应方法的Method对象
3. 创建类对象
4. 调用方法


### 测试1：回调一般方法(无参无返回)

> java端方法


```
   public native void callbackHelloFromJava();
```

> java端被回调方法


```
   public void helloFromJava() {
        Log.e("TAG", "helloFromJava()");
    }
```

> C端函数


```
  void Java_com_bruce_chang_ccalljava_Jni_callbackHelloFromJava
        (JNIEnv * env, jobject obj) {

    //1. 加载类得到jclass对象:
    //jclass      (*FindClass)(JNIEnv*, const char*);
    jclass jc = (*env)->FindClass(env, "com/bruce/chang/ccalljava/Jni");
    //2. 得到对应方法的Method对象 : GetMethodId()
    //jmethodID   (*GetMethodID)(JNIEnv*, jclass, const char*, const char*)
    jmethodID method = (*env)->GetMethodID(env, jc, "helloFromJava", "()V");
    //3. 创建类对象
    //jobject     (*AllocObject)(JNIEnv*, jclass);
    jobject obj2 = (*env)->AllocObject(env, jc);
    //4. 调用方法
    (*env)->CallVoidMethod(env, obj2, method);
}
```

### 测试2：回调带int参数方法

> java端方法


```
   public native void callbackAdd();
```

> java端被回调方法


```
    public int add(int x, int y) {
        Log.e("TAG", "add() x=" + x + " y=" + y);
        return x + y;
    }
```

> C端函数


```
  void  Java_com_bruce_chang_ccalljava_Jni_callbackAdd(JNIEnv * env, jobject obj){
     //1. 加载类得到class对象
     jclass jc = (*env)->FindClass(env, "com/bruce/chang/ccalljava/Jni");
     //2. 得到对应方法的Method对象
     jmethodID method = (*env)->GetMethodID(env, jc, "add", "(II)I");
     //3. 创建类对象
     jobject obj2 = (*env)->AllocObject(env, jc);
     //4. 调用方法
     (*env)->CallIntMethod(env, obj2, method, 3, 4);
 }
```

### 测试3：回调带String参数方法

> java端方法


```
    public native void callbackPrintString();
```

> java端被回调方法


```
   public void printString(String s) {
        Log.e("TAG", "C中输入的：" + s);
    }
```

> C端函数


```
  void  Java_com_bruce_chang_ccalljava_Jni_callbackPrintString(JNIEnv * env, jobject obj){
    //1. 加载类得到class对象
    jclass jc = (*env)->FindClass(env, "com/bruce/chang/ccalljava/Jni");
    //2. 得到对应方法的Method对象
    jmethodID method = (*env)->GetMethodID(env, jc, "printString", "(Ljava/lang/String;)V");
    //3. 创建类对象
    jobject obj2 = (*env)->AllocObject(env, jc);
    //4. 调用方法
    jstring js = (*env)->NewStringUTF(env, "I from C");
    (*env)->CallVoidMethod(env, obj2, method, js);
}
```

### 测试4：回调静态方法

> java端方法


```
   public native void callbackSayHello();
```

> java端被回调方法


```
   public static void sayHello(String s) {
        Log.e("TAG", "我是java代码中的JNI."
                + "java中的sayHello(String s)静态方法，我被C调用了:" + s);
    }
```

> C端函数


```
  void  Java_com_bruce_chang_ccalljava_Jni_callbackSayHello(JNIEnv * env, jobject obj){
    //1. 加载类得到class对象
    jclass jc = (*env)->FindClass(env, "com/bruce/chang/ccalljava/Jni");
    //2. 得到对应方法的Method对象
    jmethodID method = (*env)->GetStaticMethodID(env, jc, "sayHello", "(Ljava/lang/String;)V");
    //3. 调用方法
    jstring js = (*env)->NewStringUTF(env, "I from C");
    (*env)->CallStaticVoidMethod(env, jc, method, js);
}
```

> 结果：

![image](http://wx4.sinaimg.cn/mw690/b0d9a523ly1fbr0iibw6cj20pt0a8dgm.jpg)

### 小应用：回调更新UI的方法

> java端方法，在MainActivity中写


```
    /**
     * 让C代码调用MainActivity中的showToast方法
     */
    public native void callbackShowToast();
```

> java端被回调方法


```
    public void showToast() {
        Toast.makeText(MainActivity.this, "this is a toast!!!", Toast.LENGTH_SHORT).show();
    }
```

> C端函数


```
 /**
 * 让C代码调用MainActivity中的showToast方法
 * obj  谁调用了当前Java_com_bruce_chang_ccalljava_Jni_callbackShowToast对应的java方法就是就是谁的实例，
 * 该方法如果放在Jni这个类中就是是Jni.this，如果放在MainActivity中就是MainActivity.this
 */
void  Java_com_bruce_chang_ccalljava_MainActivity_callbackShowToast(JNIEnv * env, jobject obj){
    //1. 加载类得到class对象
    jclass jc = (*env)->FindClass(env, "com/bruce/chang/ccalljava/MainActivity");
    //2. 通过方法名和方法签名得到对应方法的Method对象
    jmethodID method = (*env)->GetMethodID(env, jc, "showToast", "()V");
    //3. 创建类对象
   // jobject obj2 = (*env)->AllocObject(env, jc);
    //4. 调用方法
    (*env)->CallVoidMethod(env, obj, method);
}
```

> 点击调用callbackShowToast方法


```
    // jni.callbackShowToast();不能调用jni中的callbackShowToast方法，这样会报空指针错误，因为发射得到的是MainActivity的类，
    // 并不是Activity，在执行showToast方法的时候就出错了
    MainActivity.this.callbackShowToast();
```

> 结果：

![image](http://wx2.sinaimg.cn/mw690/b0d9a523ly1fbr1jjuxxlj209z0bq0si.jpg)


> 切记切记

通过本实例可以发现，通过C代码调用活动中的方法的时候，一定要在活动中调用native方法，否则在C回调的时候就会出现空指针的错误。


## JNINativeInterface的相关函数指针


```
//功能: 加载类得到类对象
//返回: jni的class类型
jclass (*FindClass)(JNIEnv*, const char*)
```


```
//功能: 得到对应方法的Method对象
jmethodID (*GetMethodID)(JNIEnv*, jclass, const char*, const char*)
```


```
//功能: 创建对象
jobject (*AllocObject)(JNIEnv*, jclass)
```


```
//功能: 调用没有返回值的方法
void (*CallVoidMethod)(JNIEnv*, jobject, jmethodID, ...)

```

```
//功能: 调用返回int值的方法
jint (*CallIntMethod)(JNIEnv*, jobject, jmethodID, ...)
```
```
//功能: 得到静态方法的method

jmethodID   (*GetStaticMethodID)(JNIEnv*, jclass, const char*, const char*)
```
```
//功能: 调用静态方法
void (*CallStaticVoidMethod)(JNIEnv*, jclass, jmethodID, ...)

```
```
//功能: 将C中的字符串转换为JNI中的jstring类型
//第二个: 被转换的字符串
jstring (*NewStringUTF)(JNIEnv*, const char*)

```
```
//功能: 将第二个字符串连接到第一个字符串上

string.h---char* strcat(char *, const char *);
```
```
//功能: 得到jni类型数组的长度(元素个数)

jsize (*GetArrayLength)(JNIEnv*, jarray);
```
```
//功能: 得到jni数组中所有元素的指针
//第二个: jni中的int数组   第三个: 是否返回一个复制的数组
jint* (*GetIntArrayElements)(JNIEnv*, jintArray, jboolean*);
```
```
//参数为两个字符串
//第一个与第二个相等, 返回0    第一个大于第二个, 返回大于0   第一个小于第二个, 返回小于0
string.h---int strcmp(const char *, const char *)

```


# NDK开发综合案例

## 案例1： 美图秀秀

> 步骤

1. 创建工程MTXX
2. 把对应的.so文件拷贝到java/main/jniLibs目录。。armeabi/libmtimage-jin.so
3. 创建一个类叫做JNI.java并且要在com.mt.mtxx.image包下创建
4. 加载动态链接库，System.loadLibrary("mtimage-jni")
5. 写布局文件和实现各个效果的点击事件
6. 处理图片相关的工作
-  6.1把图片转换成矩阵（数组）
-  6.2把数组传入给C代码处理
-  6.3把处理好的数组重新生成图片
-  6.4把图片显示
      
按照上面的步骤一步一步的来，布局文件非常简单，就不写出来了。

> 布局效果

![image](http://wx3.sinaimg.cn/mw690/b0d9a523ly1fbr4ilttdbj209k0c1762.jpg)

> java程序中调用处理

1. 加载Jni程序

```
  JNI jni = new JNI();
```

2. 高亮效果


```
 public void lomoHDR(View view) {

        //6.1，把图片转换成数组
        Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.mipmap.fbb);
        //装图片的像数
        int[] pixels = new int[bitmap.getWidth() * bitmap.getHeight()];
        /**
         * 参数
         pixels       接收位图颜色值的数组
         offset      写入到pixels[]中的第一个像素索引值
         stride       pixels[]中的行间距个数值(必须大于等于位图宽度)。可以为负数
         x             从位图中读取的第一个像素的x坐标值。
         y             从位图中读取的第一个像素的y坐标值
         width       从每一行中读取的像素宽度
         height 　　读取的行数
         　　异常
         */
        bitmap.getPixels(pixels, 0, bitmap.getWidth(), 0, 0, bitmap.getWidth(), bitmap.getHeight());
        //6.2,把数组传入给C代码处理
        jni.StyleLomoHDR(pixels, bitmap.getWidth(), bitmap.getHeight());
        // 6.3，把处理好的数组重新生成图片
        bitmap = Bitmap.createBitmap(pixels, bitmap.getWidth(), bitmap.getHeight(), Bitmap.Config.ARGB_8888);
        // 6.4,把图片像数
        iv_icon.setImageBitmap(bitmap);


    }
```

3. 黑白效果


```
public void lomoC(View view) {

        //6.1，把图片转换成数组
        Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.mipmap.fbb);
        //装图片的像数
        int[] pixels = new int[bitmap.getWidth() * bitmap.getHeight()];
        bitmap.getPixels(pixels, 0, bitmap.getWidth(), 0, 0, bitmap.getWidth(), bitmap.getHeight());
        //6.2,把数组传入给C代码处理
        jni.StyleLomoC(pixels, bitmap.getWidth(), bitmap.getHeight());
        // 6.3，把处理好的数组重新生成图片
        bitmap = Bitmap.createBitmap(pixels, bitmap.getWidth(), bitmap.getHeight(), Bitmap.Config.ARGB_8888);
        // 6.4,把图片像数
        iv_icon.setImageBitmap(bitmap);

    }
```

4. 怀旧效果


```
public void lomoB(View view) {
        //6.1，把图片转换成数组
        Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.mipmap.fbb);
        //装图片的像数
        int[] pixels = new int[bitmap.getWidth() * bitmap.getHeight()];
        bitmap.getPixels(pixels, 0, bitmap.getWidth(), 0, 0, bitmap.getWidth(), bitmap.getHeight());
        //6.2,把数组传入给C代码处理
        jni.StyleLomoB(pixels, bitmap.getWidth(), bitmap.getHeight());
        // 6.3，把处理好的数组重新生成图片
        bitmap = Bitmap.createBitmap(pixels, bitmap.getWidth(), bitmap.getHeight(), Bitmap.Config.ARGB_8888);
        // 6.4,把图片像数
        iv_icon.setImageBitmap(bitmap);
    }
```

4. 还原


```
 iv_icon.setImageResource(R.mipmap.fbb);
```
剩下的四种效果就不说明了，稍后看一下结果。

> 结果(只能在arm架构的处理器上运行，x86的处理器的手机是不能运行该程序的)：

![image](http://wx1.sinaimg.cn/mw690/b0d9a523ly1fbr4rinlhrg209i0fual7.gif)

## 案例2： 锅炉压力系统

> 步骤

1. 创建一个module名字叫做GuoLu，
1.1 在MainActivity中写得到锅炉压力值的native方法---getPressure()；
1.2 在build.gradle文件中配置生成.so文件的名称


```
defaultConfig {
        applicationId "com.bruce.chang.guolu"
        minSdkVersion 15
        targetSdkVersion 25
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
        ndk{
            moduleName "GuoLu" //so文件: lib+moduleName+.so
            abiFilters "armeabi", "armeabi-v7a", "x86" //cpu的类型
        }
    }
```


2. 分析实现的原理
3. 在C代码中写锅炉的压力值，返回给java


```
/**
 得到锅炉的压力值
*/
int pressure = 20;
int getPressure(){
    int incease = rand()%20;
    pressure += incease;
    return pressure;
}
```
 
4. 在视图中动态绘制


> GuoLu.c代码


```
//
// Created by Administrator on 2016/4/19.
//
#include <stdio.h>
#include <stdlib.h>
#include <jni.h>
/**
 得到锅炉的压力值
*/
int pressure = 20;
int getPressure(){
    int incease = rand()%20;
    pressure += incease;
    return pressure;
}

/**
 * 从锅炉感应器中得到锅炉压力值
 */
jint  Java_com_bruce_chang_guolu_MainActivity_getPressure(JNIEnv *env, jobject instance) {
    int pressur = getPressure();
    return pressur;
}


```

> MainActivity.java中调用


```
public class MainActivity extends AppCompatActivity {
    {
        System.loadLibrary("GuoLu");
    }
    PressureView pressureView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        pressureView = new PressureView(MainActivity.this);
        setContentView(pressureView);
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    SystemClock.sleep(500);
                    int pressure = Math.abs(getPressure());
                    pressureView.setPressure(pressure);
                    //如果压力大于220
                    if (pressure > 220) {
                        break;
                    }
                }
            }
        }).start();
    }

    /**
     * native代码
     * 调用C代码中的对应方法
     *
     * @return
     */
    public native int getPressure();
}

```

> 自定义view---PressureView来控制显示的过程


```
public class PressureView extends View {
    private int pressure;
    private Paint mPaint;

    public PressureView(Context context) {
        super(context);
        mPaint = new Paint();
        mPaint.setAntiAlias(true);
        mPaint.setTextSize(50);
    }

    public void setPressure(int pressure) {
        this.pressure = pressure;
//        invalidate();//在主线程中运行
        postInvalidate();//ondraw()执行
    }
    
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        //1：如果压力值大于220，就绘制文本，显示锅炉爆炸了，快跑
        if (pressure > 220) {
            mPaint.setColor(Color.RED);
            canvas.drawText("要爆炸了，快跑炮", 10, getHeight() / 2, mPaint);
        } else {
            //2：正常和提示的情况
            //设置背景颜色为灰色
            mPaint.setColor(Color.GRAY);
            canvas.drawRect(10, 10, 60, 260, mPaint);
            canvas.drawText("pressure=="+pressure, 10, getHeight() / 2, mPaint);
            //2.1  如果是小于200正常显示并且设置画笔颜色绿色
            if (pressure < 200) {
                mPaint.setColor(Color.GREEN);
                canvas.drawRect(10, 260-pressure, 60, 260, mPaint);
            } else if (pressure>200){
                //2.2  如果是大于200警示显示给看护者，并且设置画笔颜色红色
                mPaint.setColor(Color.YELLOW);
                canvas.drawRect(10, 260-pressure, 60, 260, mPaint);
            }
        }
    }
}
```

> 结果（在x86的模拟器上运行）

![image](http://wx1.sinaimg.cn/mw690/b0d9a523ly1fbrdnr7f8rg207s0f20u3.gif)

以上就是自己对NDK的学习，以后工作中具体使用吧。

# 源码下载地址

https://github.com/201216323/TestNDK

> 欢迎访问[201216323.tech](http://www.201216323.tech)来查看我的CSDN博客。

> 欢迎关注我的个人微信公众号,快速查看我的最新文章。

![我的公众号图片](http://img.blog.csdn.net/20161220174646569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2NnXzIwMTIxNjMyMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "bruce常")
