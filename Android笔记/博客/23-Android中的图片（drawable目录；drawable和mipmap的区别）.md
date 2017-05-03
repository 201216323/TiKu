不管是在Eclipse还是在Android studio，存放图片的都有drawable目录，当然Android studio还有mipmap目录,首先介绍drawable的区别，然后在介绍drawable和mipmap的区别

drawable文件夹：

```
我们使用Eclipse创建新项目时，它会帮助我们自动生成六个文件夹(密度不同)：

drawable-ldpi （low:120dip）
drawable
drawable-mdpi （medium:160dip）
drawable-hdpi （high :240dip）
drawable-xhdpi （xhigh :320dip）
drawable-xxhdpi （xxhigh:480dip）
```
一般的做法是，将图片等资源放在drawable-hdip中，将一些和XML文件相关的内容（图片选择器、文字颜色选择器、自定义形状等）放在drawable中。

Google推荐：像素使用dip，文字使用sp。

在mdip文件夹，1dip=1px。

关于dp,dip,sp,pt,px的区别，可参考：**附录一**。

关于图片在不同目录下的显示举例，可参考：**附录二**。

drawable和mipmap的区别

在Android studio中，同时存在drawable目录和mipmap目录，二者没有明显区别，但是工作机制还是存在差别。

谷歌官方： 
drawable/ 
For bitmap files (PNG, JPEG, or GIF), 9-Patch image files, and XML files that describe Drawable shapes or Drawable objects that contain multiple states (normal, pressed, or focused). See the Drawable resource type. 
mipmap/ 
For app launcher icons. The Android system retains the resources in this folder (and density-specific folders such as mipmap-xxxhdpi) regardless of the screen resolution of the device where your app is installed. This behavior allows launcher apps to pick the best resolution icon for your app to display on the home screen. For more information about using the mipmap folders, see Managing Launcher Icons as mipmap Resources.

官方解释： 

mipmap——用于存放原生图片（图ic_launcher.png），缩放上有性能优化; 

drawable——存放图片、xml，和Eclipse没有区别；

附录一：

```
dip : device independent pixels(设备独立像素). 不同设备有不同的显示效果,这个和设备硬件有关，一般我们为了支持WVGA、HVGA和QVGA 推荐使用这个，不依赖像素。

dp : 和dip相同。

px : pixels(像素)，一个像素通常被视为图像的最小的完整采样，不同设备显示效果相同，一般我们HVGA代表320x480像素，这个用的比较多。

pt : point，是一个标准的长度单位，1pt＝1/72英寸，用于印刷业，非常简单易用。

sp :  scaled pixels(放大像素). 主要用于字体显示best for textsize。

in ：（英寸）：长度单位。

分辨率 ：分为显示分辨率（屏幕分辨率）和图像分辨率。

显示分辨率：屏幕图像的精密度，是指显示器所能显示的像素有多少。显示器可 显示的像素越多，画面就越精细。显示分辨率一定的情况下，显示屏越小图像越清晰，反之，显示屏大小固定时，显示分辨率越高图像越清晰。 

图象分辨率 ：单位英寸中所包含的像素点数。

```

换算公式

```
public static float applyDimension(int unit, float value,
   DisplayMetrics metrics){
  switch (unit) {
     case COMPLEX_UNIT_PX:
         return value;
     case COMPLEX_UNIT_DIP:
         return value * metrics.density;
     case COMPLEX_UNIT_SP:
         return value * metrics.scaledDensity;
     case COMPLEX_UNIT_PT:
         return value * metrics.xdpi * (1.0f/72);
     case COMPLEX_UNIT_IN:
         return value * metrics.xdpi;
     case COMPLEX_UNIT_MM:
         return value * metrics.xdpi * (1.0f/25.4f);
     }
   return 0;
}
```
附录二:

```
mdpi与hdpi是2：3的关系 
mdpi与xhdpi是1：2的关系  
ldpi 与mdpi是3：4的关系 

dp与px换算公式：

pixs =dips * (densityDpi/160). 

dips=(pixs*160)/densityDpi
```

现在假设，在一个项目中，你将一张60px *    60px的图片放到mdpi中，它的大小是60*60；

若把它拿到hdpi中，那么它的大小应该是40*40，图片缩小。

参考博客：http://blog.csdn.net/jiangwei0910410003/article/details/40509571

一、Android中的屏幕知识

**像素：**（px）每张图片都是由色点组成的，每个色点就是一个像素，像素的大小是可以变化的，所以也成为“相对长度”；相机所说的像素，实指最大像素数，如200万 = 1600 * 1200 ，像素是由相机里的光电传感器上的光敏元件数目所决定的，一个光敏元件就对应一个像素。因此像素越大，意味着光敏元件越多，相应的成本就越大。

**屏幕分辨率**：就是屏幕每行的像素点数*每列的像素点数；手机的分辨率在出厂时就是已经确定好的，不可改变。

**图像分辨率**：每英寸图像内的像素点数。图像分辨率是有单位的，叫像素每单位（px/in）；

**屏幕大小**：只屏幕对角线的物理长度，不可改变，用英寸（in）表示；

**屏幕像素密度**：（ppi）是指屏幕对角线上每英寸的像素数量；正常距离下，PPI高于300时，人眼已经无法辨别像素点。

二、实例介绍（已荣耀7为例）

![image](http://img.blog.csdn.net/20160815115704442)

![image](http://img.blog.csdn.net/20160815115832836)

在上图可以看出，荣耀7的屏幕分辨率是1920*1080，就是说手机横屏上有1920个像素点，竖屏有1080个像素点；

屏幕尺寸5.2寸（英寸），就是对角线的长度为5.2英寸，1英寸 = 2.54cm，所以对角线长度是13.2厘米。

屏幕像素密度424ppi，是说对角线上每英寸存在的像素数为424，可以根据下列公式计算，对角线像素数约为2202，对角线长度为5.2英寸，故每英寸的像素数约为424。
![image](http://img.blog.csdn.net/20160815125841884)

**屏幕分辨率和屏幕尺寸没有关系。每英寸像素数（PPI）是两者共同决定的，PPI越高，屏幕越清晰。所以我们可以得出：并不是分辨率越高，屏幕越清晰，合适的屏幕分辨率搭配合适的尺寸，才能达到高质量的屏幕质量。**

三、关于像素大小

像素的大小是不固定的，但是，一台电子设备，出厂后的屏幕分辨率和像素的物理大小就是确定的了。

为什么电脑可以调节屏幕分辨率？

电脑的实际分辨率就是最大分辨率，而调节到较小分辨率时，实际是通过填充一些模拟色块，将屏幕分辨率凑到硬件的实际分辨率。

四、开发人员适配

Android中会有dp（dip）、px、dpi、density。

dip：就是dp，设备无关像素；

px：像素，不过多介绍；

dpi：dots per inch ，每英寸的像素点数，也叫像素密度（px/inch）；

density：密度，density = 实际dpi/标准dpi，即 实际dpi /160；



```
根据定义，当dpi为160时，dp等于px，我们得1dp = 160*1inch；

又有dpi定义得：1px = 1dpi*1inch；==>1px = 1dpi*（1dp/160） = （1dpi/160）*1dp；
```
所以，dp和px的换算公式为：

1dp = px/（Dpi/160） = (px*160)/Dpi = px / 密度；

1px = dp * (Dpi/160) = dp * 密度；

总结，

120dpi(ldpi低密度屏)　　 1dp = 0.75px （ 标准： 320*240）(由于像素点是物理点，所以用2个像素点来显示3个dp的内容)

160dpi(mdpi中密度屏)　　 1dp = 1px （ 标准： 480*320）

213dpi(tvdpi电视密度屏)　　1dp = 1.33px

240dpi(hdpi高密度屏)　　 1dp = 1.5px（ 标准： 800*480）

320dpi(xhdpi极高密度屏)　 1dp = 2px（ 标准： 600*124）

图片适配

按照中密度屏给出UI，然后进行放在不同dpi的图片文件夹中；

尺寸适配


在不同dpi的values文件中，按照公式进行换算。
