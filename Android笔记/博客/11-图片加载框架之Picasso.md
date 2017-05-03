# 1：简介

Picasso是Square公司出品的一个强大的图片下载和缓存图片库。

## 1.1 Picasso特点

1. 在adapter中需要取消已经不在视野范围的ImageView图片资源的加载，否则会导致图片错位，Picasso已经解决了这个问题。
2. 使用复杂的图片压缩转换来尽可能的减少内存消耗
3. 自带内存和硬盘二级缓存功能

# 2：下载地址

Picasso的下载地址是 https://github.com/square/picasso，在Github上已经获得了12272个星星了，也是非常的受欢迎。

Picasso最新版本是2.5.2。

# 3：功能

## 3.1：基本用法


```
Picasso.with(context).load(imageUrl).into(imageView);
```

## 3.2：图片路径load()

1.  load SD卡资源
```
load("file://"+ Environment.getExternalStorageDirectory().getPath()+"/fbb.jpg")
```
2.  load assets资源

```
load("file:///android_asset/aobona.gif") 
```
3.  load drawable资源

```
load("android.resource://com.frank.glide/drawable/news")
```

4.  load http资源
```
load("http:www.123.com/1234567890.jpg") 
```

## 3.3：资源加载的方法

1. placeholder(xxx)

```
设置资源加载过程中的显示的Drawable。
```

2. error(xxx)
 

```
设置load失败时显示的Drawable。
```

3. into(xxx)


```
设置资源加载到的目标 包括ImageView Target等
```

## 3.4：图片裁剪


```
Picasso.with(this).load("http://n.sinaimg.cn/translate/20160819/9BpA-fxvcsrn8627957.jpg")  
                .resizeDimen(R.dimen.iv_width,R.dimen.iv_height)  
                .into(iv);
```

## 3.5：工具类


```
public class PicassoUtil {
    //加载本地图片
    public static void setImg(Context context, int resId, ImageView imgView){
        Picasso.with(context)
                .load(resId)
                .config(Bitmap.Config.RGB_565)//8位RGB位图
                .fit()
                .into(imgView);
    }
    //按照一定的宽高加载本地图片，带有加载错误和默认图片
    public static void setImg(Context context,int resId,ImageView imgView,int weight,int height){
        Picasso.with(context)
                .load(resId)//加载本地图片
                .config(Bitmap.Config.RGB_565)//8位RGB位图
                .resize(weight,height)//设置图片的宽高
                .into(imgView);//把图片加载到控件上
    }
    //加载网络图片到imgview,带有加载错误和默认图片
    public static void setImg(Context context, String imgurl, int resId, ImageView imgView){
        Picasso.with(context)
                .load(imgurl)//加载网络图片的url
                .config(Bitmap.Config.RGB_565)//8位RGB位图
                .placeholder(resId)//默认图片
                .error(resId)//加载错误的图片
                .fit()//图片的宽高等于控件的宽高
                .into(imgView);//把图片加载到控件上
    }
    public static void setImg(Context context, String imgurl, ImageView imgView){
        Picasso.with(context)
                .load(imgurl)//加载网络图片的url
                .config(Bitmap.Config.RGB_565)//8位RGB位图
                .fit()//图片的宽高等于控件的宽高
                .into(imgView);//把图片加载到控件上
    }
    //加载网络图片到Viewpager
    public static void setImg(Context context, String imgurl, ViewPager imgView){
        Picasso.with(context)
                .load(imgurl)//加载网络图片的url
                .config(Bitmap.Config.RGB_565)//8位RGB位图
                .fit()//图片的宽高等于控件的宽高
                .into((Target) imgView);//把图片加载到控件上
    }
    //加载网络图片到Viewpager，带有加载错误和默认图片
    public static void setImg(Context context, String imgurl, int resId, ViewPager imgView){
        Picasso.with(context)
                .load(imgurl)//加载网络图片的url
                .config(Bitmap.Config.RGB_565)//8位RGB位图
                .placeholder(resId)//默认图片
                .error(resId)//加载错误的图片
                .fit()//图片的宽高等于控件的宽高
                .into((Target) imgView);//把图片加载到控件上
    }
    //按照设定的宽高加载网络图片到imgview
    public static void setImg(Context context, String imgurl,ImageView imgView,int weight,int height){
        Picasso.with(context)
                .load(imgurl)//加载网络图片的url
                .config(Bitmap.Config.RGB_565)//8位RGB位图
                .resize(weight,height)//设置图片的宽高
                .into(imgView);//把图片加载到控件上
    }
    //按照设定的宽高加载网络图片到imgview，带有加载错误和默认图片
    public static void setImg(Context context, String imgurl, int resId,int weight,int height, ImageView imgView){
        Picasso.with(context)
                .load(imgurl)//加载网络图片的url
                .config(Bitmap.Config.RGB_565)//8位RGB位图
                .placeholder(resId)//默认图片
                .error(resId)//加载错误的图片
                .resize(weight,height)//设置图片的宽高
                .into(imgView);//把图片加载到控件上
    }
}
```

# 4：使用步骤

导入jar包或在module的gradle文件中添加compile 'com.squareup.picasso:picasso:2.5.2'

# 5：例子

## 5.1：基本用法

> 普通加载

```
        Picasso.with(PicassoBaseActivity.this)
                .load("http://ww2.sinaimg.cn/mw690/b0d9a523jw1fb272glrqkj20go0d2jrz.jpg")
                .into(iv_picasso1);
```
> 裁剪的方式加载


```
 Picasso.with(PicassoBaseActivity.this)
                .load("http://ww2.sinaimg.cn/mw690/b0d9a523jw1fb272glrqkj20go0d2jrz.jpg")
                .resize(300,300)
                .into(iv_picasso2);
```
> 旋转90度加载

```
        Picasso.with(PicassoBaseActivity.this)
                .load("http://ww2.sinaimg.cn/mw690/b0d9a523jw1fb272glrqkj20go0d2jrz.jpg")
                .rotate(90)
                .into(iv_picasso3);
```
> 效果：

![image](http://ww1.sinaimg.cn/mw690/b0d9a523jw1fb2an7vjm5g208v0h6dpr.gif)

## 5.2：listview中使用

> adapter中加载方式


```
 viewHolder.tv_picasso_item.setText("position:" + i);
        Picasso.with(mContext)
                .load(mStrings[i])
                .placeholder(R.mipmap.fbb)//加载过程中的图片
                .error(R.mipmap.ic_launcher)
                .centerCrop()
                .into(viewHolder.iv_picasso_item);
```
> 效果：

![image](http://ww3.sinaimg.cn/mw690/b0d9a523jw1fb2apmo2pgg208v0h64qp.gif)

## 5.3：转换案例

## 5.3.1：在module的gradle中添加转换库


```
dependencies {
 	compile 'jp.wasabeef:picasso-transformations:2.1.0'
    // If you want to use the GPU Filters
    compile 'jp.co.cyberagent.android.gpuimage:gpuimage-library:1.4.1'
}

repositories {
    jcenter()
}
```
## 5.3.2：在Adapter使用36中变化方式


```
switch (integer) {

            case 1: {
                int width = Utils.dip2px(mContext, 133.33f);
                int height = Utils.dip2px(mContext, 126.33f);
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .resize(width, height)
                        .centerCrop()
                        .transform((new MaskTransformation(mContext, R.drawable.mask_starfish)))
                        .into(viewHolder.iv_picasso_item);
                break;
            }
            case 2: {
                int width = Utils.dip2px(mContext, 150.0f);
                int height = Utils.dip2px(mContext, 100.0f);
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .resize(width, height)
                        .centerCrop()
                        .transform(new MaskTransformation(mContext, R.drawable.mask_chat_right))
                        .into(viewHolder.iv_picasso_item);
                break;
            }
            case 3:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation(300, 100, CropTransformation.GravityHorizontal.LEFT,
                                CropTransformation.GravityVertical.TOP))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 4:
                Picasso.with(mContext).load(R.mipmap.fbb)
                        .transform(new CropTransformation(300, 100)).into(viewHolder.iv_picasso_item);
                break;
            case 5:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation(300, 100, CropTransformation.GravityHorizontal.LEFT,
                                CropTransformation.GravityVertical.BOTTOM))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 6:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation(300, 100, CropTransformation.GravityHorizontal.CENTER,
                                CropTransformation.GravityVertical.TOP))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 7:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation(300, 100))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 8:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation(300, 100, CropTransformation.GravityHorizontal.CENTER,
                                CropTransformation.GravityVertical.BOTTOM))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 9:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation(300, 100, CropTransformation.GravityHorizontal.RIGHT,
                                CropTransformation.GravityVertical.TOP))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 10:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation(300, 100, CropTransformation.GravityHorizontal.RIGHT,
                                CropTransformation.GravityVertical.CENTER))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 11:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation(300, 100, CropTransformation.GravityHorizontal.RIGHT,
                                CropTransformation.GravityVertical.BOTTOM))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 12:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation((float) 16 / (float) 9,
                                CropTransformation.GravityHorizontal.CENTER,
                                CropTransformation.GravityVertical.CENTER))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 13:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation((float) 4 / (float) 3,
                                CropTransformation.GravityHorizontal.CENTER,
                                CropTransformation.GravityVertical.CENTER))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 14:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation(3, CropTransformation.GravityHorizontal.CENTER,
                                CropTransformation.GravityVertical.CENTER))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 15:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation(3, CropTransformation.GravityHorizontal.CENTER,
                                CropTransformation.GravityVertical.TOP))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 16:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation(1, CropTransformation.GravityHorizontal.CENTER,
                                CropTransformation.GravityVertical.CENTER))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 17:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation((float) 0.5, (float) 0.5,
                                CropTransformation.GravityHorizontal.CENTER,
                                CropTransformation.GravityVertical.CENTER))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 18:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation((float) 0.5, (float) 0.5,
                                CropTransformation.GravityHorizontal.CENTER,
                                CropTransformation.GravityVertical.TOP))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 19:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation((float) 0.5, (float) 0.5,
                                CropTransformation.GravityHorizontal.RIGHT,
                                CropTransformation.GravityVertical.BOTTOM))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 20:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropTransformation((float) 0.5, 0, (float) 4 / (float) 3,
                                CropTransformation.GravityHorizontal.CENTER,
                                CropTransformation.GravityVertical.CENTER))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 21:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropSquareTransformation())
                        .into(viewHolder.iv_picasso_item);
                break;
            case 22:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new CropCircleTransformation())
                        .into(viewHolder.iv_picasso_item);
                break;
            case 23:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new ColorFilterTransformation(Color.argb(80, 255, 0, 0)))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 24:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new GrayscaleTransformation())
                        .into(viewHolder.iv_picasso_item);
                break;
            case 25:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new RoundedCornersTransformation(30, 0,
                                RoundedCornersTransformation.CornerType.BOTTOM_LEFT))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 26:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new BlurTransformation(mContext, 25, 1))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 27:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new ToonFilterTransformation(mContext))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 28:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new SepiaFilterTransformation(mContext))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 29:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new ContrastFilterTransformation(mContext, 2.0f))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 30:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new InvertFilterTransformation(mContext))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 31:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new PixelationFilterTransformation(mContext, 20))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 32:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new SketchFilterTransformation(mContext))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 33:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new SwirlFilterTransformation(mContext, 0.5f, 1.0f, new PointF(0.5f, 0.5f)))
                        .into(viewHolder.iv_picasso_item);

                break;
            case 34:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new BrightnessFilterTransformation(mContext, 0.5f))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 35:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new KuwaharaFilterTransformation(mContext, 25))
                        .into(viewHolder.iv_picasso_item);
                break;
            case 36:
                Picasso.with(mContext)
                        .load(R.mipmap.fbb)
                        .transform(new VignetteFilterTransformation(mContext, new PointF(0.5f, 0.5f),
                                new float[]{0.0f, 0.0f, 0.0f}, 0f, 0.75f))
                        .into(viewHolder.iv_picasso_item);
                break;
        }
```

> 效果：

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fb2asg0x6cg208v0h6qv7.gif)

# 6：Picass总结

Picasso和Glide非常的相似，但Picasso这里有36种变换方式，这可以给图片处理带来丰富的效果，但在列表加载图片的时候，我发现图片会重复加载，这可能是我没有配置缓存的原因吧。

这里只是介绍了Picasso的基本使用方式，实际项目中还要根据实际情况进行封装使用。

> 源码下载地址：

https://github.com/201216323/TestPicasso

> 欢迎访问[201216323.tech](http://www.201216323.tech)来查看我的CSDN博客。

> 欢迎关注我的个人技术公众号,快速查看我的最新文章。

![我的公众号图片](http://img.blog.csdn.net/20161220174646569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2NnXzIwMTIxNjMyMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "bruce常")