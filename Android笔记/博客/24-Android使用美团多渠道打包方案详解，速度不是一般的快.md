Android使用美团多渠道打包方案详解，速度不是一般的快

> Andorid渠道市场有多分散呢？分散到比Android碎片化还严重，你还在为多渠道打包而头疼吗？美团提供了速度快到白驹过隙的多渠道打包方案。说的有点夸张，对，虽然夸张，但是确实很快，不夸张不足以形容其快。废话不多说，先讲原理，再讲实践方法。

**新旧打包方法原理对比讲解**

**传统方式**

在AndroidManifest定义渠道的年代，多渠道打包无非以下两种方案：

方案一：完全的重新编译，即在代码重新编译打包之前，在AndroidManifest中修改渠道标示；

方案二：通过ApkTool进行解包，然后修改AndroidManifest中修改渠道标示，最后再通过ApkTool进行打包、签名。
这两种打包方式，不管是哪种，效率都很低，方案一毫无效率可言，而且打包的渠道规模非常小，第二种方案效率稍微高些，打包的渠道规模也还可以，但是这两种方案速度慢的惊人，如果你打个上百的渠道包试试，估计你的电脑能卡一下午。慢，当然也有好处，你可以不用工作了，喝着咖啡，玩着手机慢慢等也很惬意是不？哈哈……

**美团高效的多渠道打包方案**

美团高效的多渠道打包方案是把一个Android应用程序包当作一个zip文件包进行解压，然后发现在签名生成的目录下添加一个空文件，空文件用渠道名来命名，而且不需要重新签名。这种方式不需要重新签名，编译等步骤，使得这种方法非常高效。

**第一步：解压apk文件**

我们直接解压apk，解压后的根目录会有一个META-INF目录，如下图所示：

![image](http://7xsgef.com1.z0.glb.clouddn.com/apk_packaging2.jpg)

如果在META-INF目录内添加空文件，可以不用重新签名应用。因此，通过为不同渠道的应用添加不同的空文件，可以唯一标识一个渠道。

**第二步：用python脚本向apk文件中添加空渠道文件**

我们用python代码来给apk添加空的渠道文件，渠道名的前缀为mtchannel_：

> import zipfile
zipped = zipfile.ZipFile(your_apk, 'a', zipfile.ZIP_DEFLATED) 
empty_channel_file = "META-INF/mtchannel_{channel}".format(channel=your_channel)
zipped.write(your_empty_file, empty_channel_file)

添加完空渠道文件后的目录，META-INFO目录多了一个名为mtchannel_meituan的空文件：

![image](http://7xsgef.com1.z0.glb.clouddn.com/apk_packaging3.jpg)

**第三步：用java代码读取渠道名，并动态设置渠道名**

我们用脚本生成了文件之后，文件的名字是用渠道名来命名的，所以我们在启动程序的时候，可以**用java代码动态读取渠道名**，并动态的去设置。

java代码读取渠道名的方法：

```
public static String getChannel(Context context) {
        ApplicationInfo appinfo = context.getApplicationInfo();
        String sourceDir = appinfo.sourceDir;
        String ret = "";
        ZipFile zipfile = null;
        try {
            zipfile = new ZipFile(sourceDir);
            Enumeration<?> entries = zipfile.entries();
            while (entries.hasMoreElements()) {
                ZipEntry entry = ((ZipEntry) entries.nextElement());
                String entryName = entry.getName();
                if (entryName.startsWith("mtchannel")) {
                    ret = entryName;
                    break;
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (zipfile != null) {
                try {
                    zipfile.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        String[] split = ret.split("_");
        if (split != null && split.length >= 2) {
            return ret.substring(split[0].length() + 1);

        } else {
            return "";
        }
    }
```

读取到了渠道名，我们就可以动态的设置了，比如友盟渠道的动态设置方法是：AnalyticsConfig.setChannel(getChannel(Context context) );这样就好了。这种方式每打一个渠道包只需复制一个apk，在META-INF中添加一个使用渠道号命名的空文件即可。这种打包方式速度非常快，据说900多个渠道不到一分钟就能打完。我亲测的是我用了10秒钟打了32个渠道包，是不是很快**。**

**实践使用**

你可能会说，我看不懂上面的python代码，那个脚本里的内容看不明白，这个没关系。你仔细明白了原理即可，因为有人给你造轮子，我们直接骑就可以了。

**实践方法使用**

**第一步：配置python环境**

我们既然需要使用脚本打包，那么相应的电脑上必须有可以运行python脚本的运行环境。所以我们第一步是要配置python运行环境。
自己去官网下载安装即可，非常简单。官网地址：https://www.python.org/

第二步：设置python脚本并把封装好的类放到工程里**

好心人已经把运行的打包脚本写好了，并且也封装了读取渠道号的实体工具类。大家只需要去github上下载即可。
地址：https://github.com/GavinCT/AndroidMultiChannelBuildTool
当然在github上也有相关的使用介绍，非常简单，一看就懂。

这里简单说下，下载下来有个ChannelUtil.java类，里面封装好了获取渠道号的方法，你只需要在启动应用程序的地方调用友盟的设置代码即可，比如：AnalyticsConfig.setChannel(ChannelUtil.getChannel(this))。

**第三步：配置渠道列表**

我们在github上把轮子下载下来之后，你解压文件，在PythonTool/Info/channel.txt中编辑渠道列表，每写一个渠道名，换行即可。

**第四步：复制签好名的包，运行脚本**

你把你已经签名打包好的apk文件，复制到PythonTool目录下和MultiChannelBuildTool.py这个脚本同级，直接双击点击MultiChannelBuildTool.py即可完成打包。

ok，到这里基本就讲完了，讲了讲原理，又讲了讲实践方式，鉴于别人都给你造好轮子了，所以使用起来非常简单，赶紧去试一试吧。如果不明白的可以留言，欢迎一起交流。

推荐：
[美团Android DEX自动拆包及动态加载简介](http://www.apkbus.com/blog-705730-60247.html)

http://www.apkbus.com/blog-705730-60247.html




