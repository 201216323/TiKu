# **友情提示：**

此文档列出的相关操作都是基于Windows操作系统，在Android Studio的 keymap=default环境下的。因为使用默认的keymap可以使用更多的快捷键，本文档只为分享学习，欢迎转载。

## Part1:快捷键

1. **ctrl+f12**：对于没有设置快捷键或者忘记快捷键的菜单或者动作（Action），可能通过输入其名字快速调用，输入debug即可启动debug功能，输入setting即可打开设置页面
2. **alt+shift+up/down**： 上下移动行，这个没什么好说的，肯定会用到
3. **ctrl+y**：删除这一行
4. **ctrl+x**：删除这一行并复制行
5. **ctrl+d**：复制行并粘贴
6. **ctrl+p**：在调用一些方法的时候免不了会忘记或者不知道此方法需要哪些参数。ctrl+p可以显示出此方法需要的参数。必备技能之一
7. **shift+f6**：重命名变量或者方法名。
8. **Alt+J**：多行编辑 
9.  Alt+`：(是1左边的那个键)此快捷键会显示一个版本管理常用的一个命令，可以通过命令前面的数字或者模糊匹配来快速选择命令
10. **f11**：将当前位置添加到书签中或者从书签中移除
11. **shift+f11**：显示有哪些书签
12. **ctrl+f12**：此快捷键可以调出当前文件的大纲，并通过模糊匹配快速跳转至指定的方法。
勾选上“show anonymous classes”后其功能相当于Eclipse中的ctrl+o

> 以上这些快捷键大家可以试试，下面推荐几个开发中方便快捷的Android Studio 插件，也是自己在开发中经常使用的：
> 

## Part2:Android Studio插件

#### 以下插件，个人在开发中经常使用：
1. **ADB Wifi**：该插件可以在我们缺少数据线的时候使用wifi模式来运行我们的程序，当然 也可以debug,但使用此插件要注意电脑和手机要连接同一个无线网络。
2. **GsonFormat**:在开发使用Gson解析Json数据的时候，我们可以使用beJson来校验生成对应的实体类，但存在的问题是生成的实体类需要我们手动复制到代码中，且代码中没有对应字段的注释，，但是使用此插件可以直接生成对应的实体类，且生成的实体类中保存当前的json串，也有相应字段的说明，特别方便。![image](http://www.jcodecraeer.com/uploads/20151009/1444358561971442.gif)
3. **Android Methods Count**:一个可以得到项目中所用的libraries中有多少方法的插件。
4. **Android ButterKnife Zelezny**：将XML中的id直接生成相应字段，但要保证XML中不能含有中文，这插件很大程度上是歪果仁开发出来的。![image](http://www.ezlippi.com/images/butterknife.gif)
5. **ECTranslation**：English&Chinese Translation，顾名思义中英文翻译的，这个插件是利用有道词典来实现翻译功能的，使用非常简单，选中相应英文按设置的快捷键即可。![image](http://p3.pstatp.com/large/e9600052869b9640c7b)
6. **Lifecycler Sorter**：更具Activity或者Fragment的生命周期排序的插件，但不是特别的准，可以参考。![image](http://www.jcodecraeer.com/uploads/20151009/1444358722597969.gif)
7. **JsonOnlineViewer**：校验接口数据的插件，可以使用GET、POST、等方式，可以配置Header和Body，展示结果和浏览器中的一款JSON插件一样.![image](http://www.jcodecraeer.com/uploads/20151009/1444358760462935.gif)
8. **CodeGlance**：可用于快速定位代码，会在AS编辑器右侧生成预览效果。![image](http://www.jcodecraeer.com/uploads/20151009/1444358792122227.gif)

#### 以下插件，是从网上看到的，但本人不经常使用，看到了就记录下来：
1. **Android Postfix Completion**：可根据后缀快速完成代码，这个属于拓展吧，系统已经有这些功能，如sout、notnull等，这个插件在原有的基础上增添了一些新的功能。![image](http://www.jcodecraeer.com/uploads/20151009/1444358608885034.gif)
2. **AndroidAccessors**：快速生成get和set方法的插件，其实系统的也挺快的，当然这个个人感觉更快。![image](http://www.jcodecraeer.com/uploads/20151009/1444358684105948.gif)
3. **adb-idea**: 支持直接在AS面板中进行ADB操作。![image](http://www.ezlippi.com/images/adb.png)
4. **SelectorChapek**：按照命名规范自动生成Selector，在资源文件夹下右击，比如’drawable_xhdpi’下：   
![image](http://www.ezlippi.com/images/selector1.png)      
选择Generate Android Selectors： 
![image](http://www.ezlippi.com/images/selector2.png)
5. **ParcelableGenerator**：Android中的序列化有两种方式，分别是实现Serializable接口和Parcelable接口，但在Android中是推荐使用Parcelable，只不过我们这种方式要比Serializable方式要繁琐,这个插件帮助我们解决繁琐的事情。![image](http://www.ezlippi.com/images/parcelable_generator.png)
6. **idea-markdown**：在AS中直接写MarkDown笔记。![image](http://www.ezlippi.com/images/preview.png)
7. **Codota**：搜索代码的插件，他的搜索源，不仅只有Github，而且还有知名博客和开发者网站，让你搜索一个东西，不用在找上半天；
除了搜索功能，首页的下方还罗列比较流行的类库，还提供保存代码的CodeBox，同时还提供了Chrome 插件和Android Studio 插件，最后通过Google，Github，Facebook 任意一个授权登录即可使用；
而且当你点击搜索的结果（Java class）的时候，右侧会显示UML 视图，而且左边的代码如果点击会有高亮现实，而且还会显示Doc，并提供了API Doc 的链接
![image](http://www.ezlippi.com/images/codota.png)



> 欢迎访问[201216323.tech](http://www.201216323.tech)来查看我的CSDN博客。

> 欢迎关注我的个人微信公众号,快速查看我的最新文章。

![我的公众号图片](http://img.blog.csdn.net/20161220174646569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2NnXzIwMTIxNjMyMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "bruce常")