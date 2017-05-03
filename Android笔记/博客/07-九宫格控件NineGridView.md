
> NineGridView介绍：

类似QQ空间，微信朋友圈，微博主页等，展示图片的九宫格控件，自动根据图片的数量确定图片大小和控件大小，使用Adapter模式设置图片，对外提供接口回调，支持任意的图片加载框架,如 Glide,ImageLoader,Fresco,xUtils3,Picasso 等，支持点击图片全屏预览大图。

原作地址：https://github.com/jeasonlzy/NineGridView  ，此项目是在原NineGridView项目上改进完成的，在原项目基础上增加了2张图片的适配，从而可以在1张、2张、4张图片的时候显示不同的布局效果，并分享一下自己的使用心得。

> 原项目有刷新的效果，但加载出来的数据不尽人意，我为了测试，将数据替换成本地了，分别展示1张、2张……11张图片的效果，这个更便于预览。


演示：点击图片都可以查看大图效果

![image](http://ww4.sinaimg.cn/mw690/b0d9a523jw1fay2qngz3wg209g0hi7wj.gif)

> 原作优点：

- 图片大图预览效果是一个库工程，方便导入使用。
- 对不同张数图片进行的适配。
- 根据NineGridView的属性来控制不同的显示模式
- 向QQ空间那样，大于9张图片的时候以+号显示
- 图片加载非常的灵活。
- 大图加载的时候有预览动画，大图的尺寸根据原图的尺度

> 原作存在的问题：
- 没有增加两张图片的适配
- 大图的时候如果需要进行图片保存的操作，不是很方便，所以还是以自定义View的形势来在项目中使用。

> 注意事项：

1. 原作的NineGridView默认是无论几张图片，宽高都是按照总宽度的1/3处理的，但也可以动态的按照宫格的宽度来处理，代码被注释掉了，打开这句注释就行了。
```
            // 宫格宽度
                gridWidth = gridHeight = (totalWidth - gridSpacing * (columnCount - 1)) / columnCount;
                //这里无论是几张图片，宽高都按总宽度的 1/3
//                gridWidth = gridHeight = (totalWidth - gridSpacing * 2) / 3;
```

2. 增加两张图片的适配，只需再写一个else if条件就可以了，这样当图片是两张的时候就可以平分整个可用空间了。
```
                else if (mImageInfo.size() == 2) {
                // 宫格宽度
                gridWidth = gridHeight = (totalWidth - gridSpacing * (2 - 1)) / 2;
               } 
```


> 以上就是对NineGridView使用时候的一些个人看法，主要就是增+两张图片的适配效果。

欢迎访问201216323.tech来查看我的CSDN博客。

欢迎关注我的个人技术公众号,快速查看我的最新文章。

![这里写图片描述](http://img.blog.csdn.net/20161220174646569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2NnXzIwMTIxNjMyMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)