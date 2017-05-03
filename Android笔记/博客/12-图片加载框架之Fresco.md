# 1：简介

Fresco是Facebook最新推出的一款用于Android应用中展示图片的强大图片库，可以从网络、本地存储和本地资源中加载图片。相对于ImageLoader，拥有更快的图片下载速度以及可以加载和显示gif图等诸多优势，是个很好的图片框架。


# 2：特点

## 2.1：内存管理

在5.0以下系统，Fresco将图片放到一个特别的内存区域。当然，在图片不显示的时候，占用的内存会自动被释放。这会使得APP更加流畅，减少因图片内存占用而引发的OOM。  

内存分配采用：系统匿名共享内存

## 2.2：渐进式呈现图片

渐进式图片格式先呈现大致的图片轮廓，然后随着图片下载的继续，呈现逐渐清晰的图片，这对于移动设备，尤其是慢网络有极大的利好，可带来更好的用户体验。

## 2.3：支持加载Gif图，支持WebP格式

## 2.4：图像的呈现

1. 自定义居中焦点(对人脸等图片显示非常有帮助)。
2. 圆角图，当然圆圈也行。
3. 下载失败之后，点击重新下载。
4. 自定义占位图，自定义overlay, 或者进度条。
5. 指定用户按压时的overlay。

## 2.5：图像的加载

1. 为同一个图片指定不同的远程路径，或者使用已经存在本地缓存中的图片。
2. 先显示一个低解析度的图片，等高清图下载完之后再显示高清图。
3. 加载完成回调通知。
4. 对于本地图，如有EXIF缩略图，在大图加载完成之前，可先显示缩略图。
5. 缩放或者旋转图片。
6. 处理已下载的图片。

# 3：下载地址

Github下载地址是：https://github.com/facebook/fresco

官方使用网址：http://fresco-cn.org/docs/index.html

中文文档地址：https://www.fresco-cn.org/docs/index.html

# 4：支持的URI



类型 | Scheme| 示例
---|---|---
远程图片 | http://, https://|HttpURLConnection
本地文件| file://| FileInputStream
Content provider | content://| ContentResolver
asset目录下的资源 | asset://| AssetManager
res目录下的资源 | res://| Resources.openRawResource

需要注意的是：Fresco中所有的URI都必须是绝对路径，并且带上该URI的scheme 

例如，要显示res/drawable下的一张图片可以使用下面的方式：


```
Uri uri = Uri.parse("res://aaaaa/"+R.mipmap.pic);
```


上面的额”aaaaa”可以使用随意的字符串，不过还是建议使用包名来书写。
		
# 5：常用API

## 1：宽度不支持wrap_content， 如果要设置宽高比, 需要在Java代码中指定setAspectRatio(float ratio);


```
android:layout_width="20dp"
```

## 2：高度不支持wrap_content

```
android:layout_height="20dp" 
```
 
## 3：下载成功之前显示的图片

```
fresco:placeholderImage="@color/wait_color"
```

## 4：设置图片缩放. 通常使用focusCrop,该属性值会通过算法把人头像放在中间

```
fresco:placeholderImageScaleType="fitCenter"
```

## 5：加载失败的时候显示的图片

```
fresco:failureImage="@drawable/error"
```
## 6：加载失败的时候图片的缩放类型


```
fresco:failureImageScaleType=“centerInside"
```

## 7：加载失败,提示用户点击重新加载的图片(会覆盖failureImage的图片)


```
fresco:retryImage="@drawable/retrying"
```

## 8：设置圆形方式显示图片


```
fresco:roundAsCircle="true"
```

## 9：圆角设置


```
    fresco:roundedCornerRadius="1dp"
    fresco:roundTopLeft="true"
    fresco:roundTopRight="false"
    fresco:roundBottomLeft="false"
    fresco:roundBottomRight="true"
    fresco:roundWithOverlayColor="@color/corner_color"
    fresco:roundingBorderWidth="2dp"
    fresco:roundingBorderColor="@color/border_color"

```

# 6：使用步骤

## 6.1：添加依赖


```
dependencies {
  // 在 API < 14 上的机器支持 WebP 时，需要添加
  compile 'com.facebook.fresco:animated-base-support:0.14.1'

  // 支持 GIF 动图，需要添加
  compile 'com.facebook.fresco:animated-gif:0.14.1'

  // 支持 WebP （静态图+动图），需要添加
  compile 'com.facebook.fresco:animated-webp:0.14.1'
  compile 'com.facebook.fresco:webpsupport:0.14.1'

  // 仅支持 WebP 静态图，需要添加
  compile 'com.facebook.fresco:webpsupport:0.14.1'
  // 其他依赖
  compile 'com.facebook.fresco:fresco:0.14.1'
}
```
fresco最新版本是：1.0.0

## 6.2：在application中初始化Fresco


```
Fresco.initialize(this);
```
## 6.3：配置网络权限

```
<uses-permission android:name="android.permission.INTERNET"/>
```

## 6.4：在xml布局文件中,加入命名空间


```
<!-- 其他元素-->
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:fresco="http://schemas.android.com/apk/res-auto"
    android:layout_height="match_parent"
    android:layout_width="match_parent">
```


## 6.5：在xml中引入SimpleDraweeView


```
<com.facebook.drawee.view.SimpleDraweeView
    android:id="@+id/my_image_view"
    android:layout_width="130dp"
    android:layout_height="130dp"
    fresco:placeholderImage="@drawable/my_drawable"
  />
```


## 6.6：在Java代码中开始加载图片


```
Uri uri = Uri.parse("hhttp://p1.so.qhmsg.com/bdr/_240_/t01ffd622bffeabb5e1.jpg");
SimpleDraweeView draweeView = (SimpleDraweeView) findViewById(R.id.my_image_view);
draweeView.setImageURI(uri);
```


## 6.7：注意事项

如果项目中使用了OkHttp需要进行替换,

> For OkHttp2:

```
compile "com.facebook.fresco:imagepipeline-okhttp:0.12.0+"
```
> For OkHttp3:

```
compile "com.facebook.fresco:imagepipeline-okhttp3:0.12.0+"
```

# 7：例子

## 7.1：带进度条的图片

> 布局


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:fresco="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_fresco_spimg"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.bruce.chang.testfresco.FrescoSpimgActivity">

    <com.facebook.drawee.view.SimpleDraweeView
        android:id="@+id/sdv_fresco"
        android:layout_width="150dp"
        android:layout_height="150dp"
        fresco:placeholderImage="@mipmap/fbb"
        />
    <Button
        android:id="@+id/bt_load"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="加载"
        />
</LinearLayout>

```

> 代码


```
setTitle("带进度条的图片");
        bt_load.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 设置带进度条样式
                GenericDraweeHierarchyBuilder builder = new GenericDraweeHierarchyBuilder(getResources());
                GenericDraweeHierarchy hierarchy = builder.setProgressBarImage(new ProgressBarDrawable()).build();
                sdv_fresco.setHierarchy(hierarchy);
                Uri uri = Uri.parse("http://bizhi.zhuoku.com/bizhi/200706/4/20070625/bingbing/012.jpg");
                // 设置显示图片
                sdv_fresco.setImageURI(uri);
            }
        });
```

> 结果，因为布局中设置了placeholderImage，所以会加载一张默认图片

![image](http://ww1.sinaimg.cn/mw690/b0d9a523jw1fb4jmachcbg207i0ecacy.gif)

## 7.2：图片的不同裁剪

> 布局


```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:fresco="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_horizontal"
    android:orientation="vertical">

    <com.facebook.drawee.view.SimpleDraweeView
        android:id="@+id/sdv_fresco_crop"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_marginTop="48dp"
        android:background="@android:color/black"
        fresco:placeholderImage="@mipmap/fbb" />
    <!--裁剪方式的描述信息-->
    <TextView
        android:id="@+id/tv_fresco_explain"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:gravity="center"
        android:textSize="16sp" />

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="48dp"
            android:orientation="vertical">
            <!--9个代表不同裁剪样式的button，这里省略-->
         
        </LinearLayout>
    </ScrollView>

</LinearLayout>

```

> 代码设置

1. GenericDraweeHierarchyBuilder的初始化

```
 GenericDraweeHierarchyBuilder builder = new GenericDraweeHierarchyBuilder(getResources());
```
2. imageDisplay（加载图片的方法）

```
//显示图片
    private void (GenericDraweeHierarchy hierarchy) {
        sdv_fresco_crop.setHierarchy(hierarchy);
        Uri uri = Uri.parse("http://bizhi.zhuoku.com/bizhi/200706/4/20070625/bingbing/012.jpg");
        sdv_fresco_crop.setImageURI(uri);
    }
```
3. 9个按钮的方法


```
@Override
    public void onClick(View view) {
        switch (view.getId()) {
            //CENTER
            case R.id.bt_fresco_center:
                tv_fresco_explain.setText("居中，无缩放");
                //样式设置
                GenericDraweeHierarchy hierarchy = builder.setActualImageScaleType(ScalingUtils.ScaleType.CENTER).build();
                //显示图片
                imageDisplay(hierarchy);
                break;
            //CENTER_CROP
            case R.id.bt_fresco_centercrop:
                tv_fresco_explain.setText("保持宽高比缩小或放大，使得两边都大于或等于显示边界。居中显示");
                //样式设置
                GenericDraweeHierarchy hierarchy1 = builder.setActualImageScaleType(ScalingUtils.ScaleType.CENTER_CROP).build();
                //显示图片
                imageDisplay(hierarchy1);
                break;
            //FOCUS_CROP
            case R.id.bt_fresco_focuscrop:
                tv_fresco_explain.setText("同centerCrop, 但居中点不是中点，而是指定的某个点,这里我设置为图片的左上角那点");
                //设置focusCrop的缩放形式  并指定缩放的中心点在左上角
                PointF point = new PointF(0f, 0f);
                //样式设置
                GenericDraweeHierarchy hierarchy2 = builder.setActualImageScaleType(ScalingUtils.ScaleType.FOCUS_CROP)
                        .setActualImageFocusPoint(point)
                        .build();
                //显示图片
                imageDisplay(hierarchy2);
                break;
            //CENTER_INSIDE
            case R.id.bt_fresco_centerinside:
                tv_fresco_explain.setText("使两边都在显示边界内，居中显示。如果图尺寸大于显示边界，则保持长宽比缩小图片");
                GenericDraweeHierarchy hierarchy3 = builder.setActualImageScaleType(ScalingUtils.ScaleType.CENTER_INSIDE).build();
                imageDisplay(hierarchy3);
                break;
            //FIT_CENTER
            case R.id.bt_fresco_fitcenter:
                tv_fresco_explain.setText("保持宽高比，缩小或者放大，使得图片完全显示在显示边界内。居中显示");
                GenericDraweeHierarchy hierarchy4 = builder.setActualImageScaleType(ScalingUtils.ScaleType.FIT_CENTER).build();
                imageDisplay(hierarchy4);
                break;
            //FIT_START
            case R.id.bt_fresco_fitstart:
                tv_fresco_explain.setText("保持宽高比，缩小或者放大，使得图片完全显示在显示边界内，不居中，和显示边界左上对齐");
                GenericDraweeHierarchy hierarchy5 = builder.setActualImageScaleType(ScalingUtils.ScaleType.FIT_START).build();
                imageDisplay(hierarchy5);
                break;
            //FIT_END
            case R.id.bt_fresco_fitend:
                tv_fresco_explain.setText("保持宽高比，缩小或者放大，使得图片完全显示在显示边界内，不居中，和显示边界右下对齐");
                GenericDraweeHierarchy hierarchy6 = builder.setActualImageScaleType(ScalingUtils.ScaleType.FIT_END).build();
                imageDisplay(hierarchy6);
                break;
            //FIT_XY
            case R.id.bt_fresco_fitxy:
                tv_fresco_explain.setText("不保持宽高比，填充满显示边界");
                GenericDraweeHierarchy hierarchy7 = builder.setActualImageScaleType(ScalingUtils.ScaleType.FIT_XY).build();
                imageDisplay(hierarchy7);
                break;
            //null
            case R.id.bt_fresco_none:
                tv_fresco_explain.setText("不设置任何样式");
                GenericDraweeHierarchy hierarchy8 = builder.setActualImageScaleType(null).build();
                imageDisplay(hierarchy8);
                break;
            default:
                break;
        }
    }
```

> 结果，因为图片大小的缘故，没有完全演示，自己可以动手试试

![image](http://ww3.sinaimg.cn/mw690/b0d9a523jw1fb4jnws49ng207i0ec42n.gif)

## 7.3：圆形和圆角图片

> 布局


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:fresco="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.facebook.drawee.view.SimpleDraweeView
        android:id="@+id/sdv_fresco_circleandcorner"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_gravity="center"
        fresco:placeholderImage="@mipmap/fbb" />

    <Button
        android:id="@+id/bt_fresco_circle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="48dp"
        android:text="设置圆形图片" />

    <Button
        android:id="@+id/bt_fresco_corner"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:text="设置圆角图片" />

</LinearLayout>

```
> 代码


```
uri = Uri.parse("http://bizhi.zhuoku.com/bizhi/200706/4/20070625/bingbing/012.jpg");
         builder = new GenericDraweeHierarchyBuilder(getResources());

        bt_fresco_circle.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 参数设置为圆形
                RoundingParams params = RoundingParams.asCircle();
                GenericDraweeHierarchy hierarchy = builder.setRoundingParams(params).build();
                sdv_fresco_circleandcorner.setHierarchy(hierarchy);
                sdv_fresco_circleandcorner.setImageURI(uri);
            }
        });

        bt_fresco_corner.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 配置参数
                RoundingParams params = RoundingParams.fromCornersRadius(50f);//设置圆角大小
                params.setOverlayColor(getResources().getColor(android.R.color.holo_blue_light));//覆盖层
                params.setBorder(getResources().getColor(android.R.color.holo_blue_light), 5);//边框
//                params.setRoundAsCircle(true);//如果是RoundingParams.fromCornersRadius，这个可以强制进行圆形展示
                // 设置圆形参数
                GenericDraweeHierarchy hierarchy = builder.setRoundingParams(params).build();
                sdv_fresco_circleandcorner.setHierarchy(hierarchy);
                // 加载图片
                sdv_fresco_circleandcorner.setImageURI(uri);
            }
        });
```

> 结果

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fb4jt0ra0fg208y0gr0vp.gif)

## 7.4：渐进式显示图片

> 布局


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:fresco="http://schemas.android.com/apk/res-auto"
    android:id="@+id/activity_fresco_jpeg"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    
    <com.facebook.drawee.view.SimpleDraweeView
        android:layout_gravity="center"
        android:id="@+id/sdv_fresco_jpeg"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_marginTop="48dp"
        fresco:placeholderImage="@mipmap/fbb" />

    <Button
        android:id="@+id/sdv_fresco_askImg"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="48dp"
        android:text="请求网络图片" />
</LinearLayout>

```

> 代码


```
builder = new GenericDraweeHierarchyBuilder(getResources());

        sdv_fresco_askImg.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 加载质量配置
                ProgressiveJpegConfig jpegConfig = new ProgressiveJpegConfig() {
                    @Override
                    public int getNextScanNumberToDecode(int scanNumber) {
                        return scanNumber + 2;
                    }

                    @Override
                    public QualityInfo getQualityInfo(int scanNumber) {
                        boolean isGoodEnough = (scanNumber >= 5);

                        return ImmutableQualityInfo.of(scanNumber, isGoodEnough, false);
                    }
                };

                ImagePipelineConfig.newBuilder(FrescoJianJinShiActivity.this).setProgressiveJpegConfig(jpegConfig).build();
                // 获取图片URL
                uri = Uri.parse("http://bizhi.zhuoku.com/bizhi/200706/4/20070625/bingbing/012.jpg");

                // 获取图片请求
                ImageRequest request = ImageRequestBuilder.newBuilderWithSource(uri).setProgressiveRenderingEnabled(true).build();

                DraweeController draweeController = Fresco.newDraweeControllerBuilder()
                        .setImageRequest(request)
                        .setTapToRetryEnabled(true)
                        .setOldController(sdv_fresco_jpeg.getController())//使用oldController可以节省不必要的内存分配
                        .build();

                // 设置加载的控制
                sdv_fresco_jpeg.setController(draweeController);
            }
        });
```


> 结果

![image](http://ww4.sinaimg.cn/mw690/b0d9a523jw1fb4jzy49frg208y0grwg3.gif)

## 7.5：Gif动画图片

> 布局


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:fresco="http://schemas.android.com/apk/res-auto"
    android:id="@+id/activity_fresco_gif"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">


    <com.facebook.drawee.view.SimpleDraweeView
        android:id="@+id/sdv_fresco_gif"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_marginTop="48dp"
        android:layout_gravity="center"
        fresco:placeholderImage="@mipmap/fbb" />

    <Button
        android:id="@+id/bt_fresco_askImg"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="48dp"
        android:text="请求gif图片" />

    <Button
        android:id="@+id/bt_fresco_stopAnim"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:text="动画停止" />

    <Button
        android:id="@+id/bt_fresco_startAnim"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:text="动画开始" />
</LinearLayout>

```


> 代码

1. 请求Gif图片

```
bt_fresco_askImg.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 图片地址
                Uri uri = Uri.parse("http://www.sznews.com/humor/attachement/gif/site3/20140902/4487fcd7fc66156f51db5d.gif");
                DraweeController controller = Fresco.newDraweeControllerBuilder()
                        .setUri(uri)
                        .setAutoPlayAnimations(false)//设置为true将循环播放Gif动画
                        .setOldController(sdv_fresco_gif.getController())
                        .build();

                // 设置控制器
                sdv_fresco_gif.setController(controller);
            }
        });
```

2. 开始动画


```
bt_fresco_startAnim.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Animatable animation = sdv_fresco_gif.getController().getAnimatable();

                if (animation != null && !animation.isRunning()) {
                    animation.start();
                }
            }
        });
```

3. 停止动画


```
 bt_fresco_stopAnim.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Animatable animation = sdv_fresco_gif.getController().getAnimatable();

                if (animation != null && animation.isRunning()) {
                    animation.stop();
                }
            }
        });
```

> 结果（不要忘了在gradle中添加gif的依赖）


```
compile 'com.facebook.fresco:animated-gif:0.14.1'
```

![image](http://ww4.sinaimg.cn/mw690/b0d9a523jw1fb4k9p0w2bg208y0grqiy.gif)

## 7.6：多图请求以及图片复用

>  布局


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:fresco="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_fresco_multi"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">


    <com.facebook.drawee.view.SimpleDraweeView
        android:id="@+id/sdv_fresco_multi"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_gravity="center"
        android:layout_marginTop="48dp"
        fresco:placeholderImage="@mipmap/fbb" />

    <Button
        android:id="@+id/bt_fresco_multiImg"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="48dp"
        android:text="先显示低分辨率的图，然后是高分辨率的图"
        android:onClick="bt_fresco_multiImg_click"
        />

    <Button
        android:id="@+id/bt_fresco_thumbnailImg"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:text="本地缩略图预览"
        android:onClick="bt_fresco_thumbnailImg_click"
        />

    <Button
        android:id="@+id/bt_fresco_multiplexImg"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:text="本地图片复用"
        android:onClick="bt_fresco_multiplexImg_click"
        />

</LinearLayout>

```
> 代码

1. 先显示低分辨率的图，然后是高分辨率的图


```
void bt_fresco_multiImg_click(View view) {
        //同一张图片，不同品质的两个uri
        Uri lowResUri = Uri.parse("http://img1.gamedog.cn/2012/03/11/19-120311133617-50.jpg");
        Uri highResUri = Uri.parse("http://img5.duitang.com/uploads/item/201312/03/20131203153823_Y4y8F.jpeg");
        DraweeController controller = Fresco.newDraweeControllerBuilder()
                .setLowResImageRequest(ImageRequest.fromUri(lowResUri))
                .setImageRequest(ImageRequest.fromUri(highResUri))
                .setOldController(sdv_fresco_multi.getController())
                .build();
        sdv_fresco_multi.setController(controller);
    }
```

2. 本地缩略图预览


```
void bt_fresco_thumbnailImg_click(View view){

        //将本地图片地址转换成Uri
        Uri uri = Uri.fromFile(new File(Environment.getExternalStorageDirectory()+"/swj.jpg"));
        ImageRequest request = ImageRequestBuilder.newBuilderWithSource(uri)
                .setLocalThumbnailPreviewsEnabled(true)
                .build();
        DraweeController controller = Fresco.newDraweeControllerBuilder()
                .setImageRequest(request)
                .setOldController(sdv_fresco_multi.getController())
                .build();
        sdv_fresco_multi.setController(controller);
    }
```

3. 本地图片复用


```
void bt_fresco_multiplexImg_click(View view){

        //本地图片的复用
        //在请求之前，还会去内存中请求一次图片，没有才会先去本地，最后去网络uri

        //本地准备复用图片的uri  如果本地这个图片不存在，会自动去加载下一个uri
        Uri uri1 = Uri.fromFile(new File(Environment.getExternalStorageDirectory()+"/swj.jpg"));
        //图片的网络uri
        Uri uri2 = Uri.parse("http://img5.duitang.com/uploads/item/201312/03/20131203153823_Y4y8F.jpeg");
        ImageRequest request1 = ImageRequest.fromUri(uri1);
        ImageRequest request2 = ImageRequest.fromUri(uri2);
        ImageRequest[] requests = {request1, request2};
        DraweeController controller = Fresco.newDraweeControllerBuilder()
                .setFirstAvailableImageRequests(requests)
                .setOldController(sdv_fresco_multi.getController())
                .build();
        sdv_fresco_multi.setController(controller);
    }
```
> 结果

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fb4kp5ea49g208y0gr780.gif)

## 7.7：图片加载监听

> 布局


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:fresco="http://schemas.android.com/apk/res-auto"
    android:id="@+id/activity_fresco_listener"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">


    <com.facebook.drawee.view.SimpleDraweeView
        android:layout_gravity="center"
        android:id="@+id/sdv_fresco_listener"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_marginTop="48dp"
        fresco:placeholderImage="@mipmap/fbb" />

    <Button
        android:id="@+id/bt_fresco_listener"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="48dp"
        android:text="开始加载图片"
        android:onClick="bt_fresco_listener_click"
        />

    <TextView
        android:id="@+id/tv_fresco_listener"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textColor="@android:color/black"
        android:textSize="14sp"
        android:text="tv_fresco_listener"
        />

    <TextView
        android:id="@+id/tv_fresco_listener2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textColor="@android:color/black"
        android:textSize="14sp"
        android:text="tv_fresco_listener2"
        />
</LinearLayout>

```


> 代码


1. 自定义BaseControllerListener初始化


```
//对所有的图片加载，onFinalImageSet 或者 onFailure 都会被触发。前者在成功时，后者在失败时。
    //如果允许呈现渐进式JPEG，同时图片也是渐进式图片，onIntermediateImageSet会在每个扫描被解码后回调。
    // 具体图片的那个扫描会被解码，参见渐进式JPEG图
    private ControllerListener controllerListener = new BaseControllerListener<ImageInfo>() {
        @Override
        public void onFinalImageSet(String id, ImageInfo imageInfo, Animatable animatable) {
           //前者在成功时
            super.onFinalImageSet(id, imageInfo, animatable);

            if (imageInfo == null) {
                return;
            }

            QualityInfo qualityInfo = imageInfo.getQualityInfo();

            tv_fresco_listener.setText("Final image received! " +
                    "\nSize: " + imageInfo.getWidth()
                    + "x" + imageInfo.getHeight()
                    + "\nQuality level: " + qualityInfo.getQuality()
                    + "\ngood enough: " + qualityInfo.isOfGoodEnoughQuality()
                    + "\nfull quality: " + qualityInfo.isOfFullQuality());
        }

        @Override
        public void onIntermediateImageSet(String id, ImageInfo imageInfo) {
            super.onIntermediateImageSet(id, imageInfo);

            tv_fresco_listener2.setText("IntermediateImageSet image receiced");
        }
        //后者在失败时
        @Override
        public void onFailure(String id, Throwable throwable) {
            super.onFailure(id, throwable);

            tv_fresco_listener.setText("Error loading" + id);
        }
    };
```


2. 开始加载图片按钮方法

```
void bt_fresco_listener_click(View view) {

        ProgressiveJpegConfig jpegConfig = new ProgressiveJpegConfig() {
            @Override
            public int getNextScanNumberToDecode(int scanNumber) {
                return scanNumber + 2;
            }

            @Override
            public QualityInfo getQualityInfo(int scanNumber) {
                boolean isGoodEnough = (scanNumber >= 5);

                return ImmutableQualityInfo.of(scanNumber, isGoodEnough, false);
            }
        };

        ImagePipelineConfig.newBuilder(this).setProgressiveJpegConfig(jpegConfig).build();
        Uri uri = Uri.parse("http://h.hiphotos.baidu.com/zhidao/pic/item/58ee3d6d55fbb2fbac4f2af24f4a20a44723dcee.jpg");
        ImageRequest request = ImageRequestBuilder.newBuilderWithSource(uri).setProgressiveRenderingEnabled(true).build();

        DraweeController controller = Fresco.newDraweeControllerBuilder()
                .setOldController(sdv_fresco_listener.getController())
                .setImageRequest(request)
                //设置监听器监听图片加载状态
                .setControllerListener(controllerListener)
                .build();

        sdv_fresco_listener.setController(controller);
    }
```


> 结果,加载成功的时候可以得到图片的相关信息，如Size，在浏览器中查看，该图片的尺寸就是那么大的。

![image](http://ww3.sinaimg.cn/mw690/b0d9a523jw1fb4l73xynyg208y0grgo6.gif)

## 7.8：图片缩放和旋转

> 布局


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:fresco="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_fresco_resize"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.facebook.drawee.view.SimpleDraweeView
        android:id="@+id/sdv_fresco_resize"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_gravity="center"
        android:layout_marginTop="48dp"
        fresco:placeholderImage="@mipmap/fbb" />

    <Button
        android:id="@+id/bt_fresco_resize"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="48dp"
        android:onClick="bt_fresco_resize_click"
        android:text="修内存中改图片大小" />

    <Button
        android:id="@+id/bt_fresco_rotate"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="bt_fresco_rotate_click"
        android:text="旋转图片" />

</LinearLayout>

```

> 代码


```
//缩放
    void bt_fresco_resize_click(View view) {

        int width = 50;
        int height = 50;
        Uri uri = Uri.parse("http://c.hiphotos.baidu.com/image/pic/item/962bd40735fae6cd21a519680db30f2442a70fa1.jpg");
        ImageRequest request = ImageRequestBuilder.newBuilderWithSource(uri)
                .setResizeOptions(new ResizeOptions(width, height)).build();
        PipelineDraweeController controller = (PipelineDraweeController) Fresco.newDraweeControllerBuilder()
                .setOldController(sdv_fresco_resize.getController())
                .setImageRequest(request)
                .build();
        sdv_fresco_resize.setController(controller);
    }

    //旋转
    void bt_fresco_rotate_click(View view) {
        Uri uri2 = Uri.parse("http://c.hiphotos.baidu.com/image/pic/item/962bd40735fae6cd21a519680db30f2442a70fa1.jpg");
        RotationOptions rotationOptions = RotationOptions.forceRotation(RotationOptions.ROTATE_90);
        ImageRequest request1 = ImageRequestBuilder.newBuilderWithSource(uri2)
                .setRotationOptions(RotationOptions.autoRotate())
                .setRotationOptions(rotationOptions)//旋转的时候要使用RotationOptions类
                .build();//但貌似Fresco在旋转的功能上不是很好
        DraweeController controller1 = Fresco.newDraweeControllerBuilder()
                .setImageRequest(request1)
                .setOldController(sdv_fresco_resize.getController()).build();
        sdv_fresco_resize.setController(controller1);
    }
```


> 结果

![image](http://ww3.sinaimg.cn/mw690/b0d9a523jw1fb50aeo8y6g208y0grwke.gif)


## 7.9：修改图片

> 布局


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:fresco="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_fresco_modify"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.facebook.drawee.view.SimpleDraweeView
        android:id="@+id/sdv_fresco_modify"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_gravity="center"
        android:layout_marginTop="48dp"
        fresco:placeholderImage="@mipmap/fbb" />

    <Button
        android:id="@+id/bt_fresco_modify"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="48dp"
        android:text="为图片添加网格"
        android:onClick="bt_fresco_modify_click"
        />
</LinearLayout>

```

> 代码


```
void bt_fresco_modify_click(View view){

        Uri uri = Uri.parse("http://c.hiphotos.baidu.com/image/pic/item/962bd40735fae6cd21a519680db30f2442a70fa1.jpg");

        Postprocessor redMeshPostprocessor = new BasePostprocessor() {
            @Override
            public String getName() {
                return "redMeshPostprocessor";
            }
            //绘制红色点状网格
            @Override
            public void process(Bitmap bitmap) {

                for (int x = 0; x < bitmap.getWidth(); x += 2) {

                    for (int y = 0; y < bitmap.getHeight(); y += 2) {
                        bitmap.setPixel(x, y, Color.RED);
                    }
                }
            }
        };
        ImageRequest request = ImageRequestBuilder.newBuilderWithSource(uri)
                .setPostprocessor(redMeshPostprocessor)
                .build();
        PipelineDraweeController controller = (PipelineDraweeController)
                Fresco.newDraweeControllerBuilder()
                        .setImageRequest(request)
                        .setOldController(sdv_fresco_modify.getController())
                        .build();
        sdv_fresco_modify.setController(controller);
    }
```

> 结果

![image](http://ww3.sinaimg.cn/mw690/b0d9a523jw1fb509ax122g208y0gr0y7.gif)


## 7.10：

> 布局


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_fresco_auto_size"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">


    <Button
        android:id="@+id/bt_fresco_loadsmall"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:text="加载图片"
        android:onClick="bt_fresco_loadsmall_click"
        />

    <LinearLayout
        android:id="@+id/ll_fresco"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:orientation="vertical">

    </LinearLayout>
</LinearLayout>

```

> 代码


```
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_fresco_dynamic_display_img);
        ll_fresco = (LinearLayout) findViewById(R.id.ll_fresco);
        setTitle("动态展示图片");

        simpleDraweeView = new SimpleDraweeView(this);
        //设置宽高比
        simpleDraweeView.setAspectRatio(1.0f);
    }

    void bt_fresco_loadsmall_click(View view) {

        final Uri uri = Uri.parse("http://img4q.duitang.com/uploads/item/201304/27/20130427043538_wAfHC.jpeg");

        ImageRequest request = ImageRequestBuilder.newBuilderWithSource(uri)
                .build();

        PipelineDraweeController controller = (PipelineDraweeController)
                Fresco.newDraweeControllerBuilder()
                        .setImageRequest(request)
                        .setOldController(simpleDraweeView.getController())
                        .build();
        simpleDraweeView.setController(controller);
        ll_fresco.addView(simpleDraweeView);
    }
```

> 结果

![image](http://ww2.sinaimg.cn/mw690/b0d9a523jw1fb5085lt7bg208y0grn0m.gif)



# 8：注意事项

## 8.1：问题处理

1. 重复的边界

```
这是使用圆角的一个缺陷。 参考圆角和圆圈 来获取更多信息。
```

2. 图片没有加载


```
你可以从 image pipeline 打出的日志来查看原因。 这里提供一些通常会导致问题的原因:
```
 
3. 文件不可用


```
无效的路径、链接会导致这种情况。
判断网络链接是否有效，你可以尝试在浏览器中打开它，看看是否图片会被加载。若图片依然加载不出来，那么这不是Fresco的问题。
判断本地文件是否有效，你可以通过下面这段代码来校验：

FileInputStream fis = new FileInputStream(new File(localUri.getPath()));

如果这里抛出了异常，那么这不是Fresco的问题，可能是你的其他代码导致的。有可能是没有获取到SD卡读取权限、路径不合法、文件不存在等。
```

4. OOM - 无法分配图片空间

```
加载特别特别大的图片时最容易导致这种情况。
如果你加载的图片比承载的View明显大出太多，那你应该考虑将它Resize一下。
```

5. Bitmap太大导致无法绘制

```
Android 无法绘制长或宽大于2048像素的图片。
这是由OpenGL渲染系统限制的，如果它超过了这个界限，Fresco会对它进行Resize。
```

6. 通过Logcat来判断原因

```
在加载图片时会出现各种各样的原因导致加载失败。
在使用Fresco的时候，最直接的方式就是查看 image pipeline 打出的VERBOSE级别日志。
```

7. 启动日志

```
默认情况下Fresco是关闭日志输出的，你可以配置image pipeline让它启动.

Set<RequestListener> requestListeners = new HashSet<>();
requestListeners.add(new RequestLoggingListener());
ImagePipelineConfig config = ImagePipelineConfig.newBuilder(context)
   // other setters
   .setRequestListeners(requestListeners)
   .build();
Fresco.initialize(context, config);
FLog.setMinimumLoggingLevel(FLog.VERBOSE);
```

8. 查看日志

```
你可以通过下面这条shell命令来查看Fresco日志：

adb logcat -v threadtime | grep -iE 'LoggingListener|AbstractDraweeController|BufferedDiskCache'

它的输出为如下格式：
08-12 09:11:14.792 6690 6734 V unknown:RequestLoggingListener: time 11201792: onProducerStart: {requestId: 1, producer: EncodedMemoryCacheProducer}
08-12 09:11:14.792 6690 6734 V unknown:RequestLoggingListener: time 11201792: onProducerFinishWithSuccess: {requestId: 1, producer: EncodedMemoryCacheProducer, elapsedTime: 0 ms, extraMap: {cached_value_found=false}}
08-12 09:11:14.792 6690 6734 V unknown:RequestLoggingListener: time 11201792: onProducerStart: {requestId: 1, producer: DiskCacheProducer}
08-12 09:11:14.792 6690 6735 V unknown:BufferedDiskCache: Did not find image for http://www.example.com/image.jpg in staging area
08-12 09:11:14.793 6690 6735 V unknown:BufferedDiskCache: Disk cache read for http://www.example.com/image.jpg
08-12 09:11:14.793 6690 6735 V unknown:BufferedDiskCache: Disk cache miss for http://www.example.com/image.jpg
08-12 09:11:14.793 6690 6735 V unknown:RequestLoggingListener: time 11201793: onProducerFinishWithSuccess: {requestId: 1, producer: DiskCacheProducer, elapsedTime: 1 ms, extraMap: {cached_value_found=false}}
08-12 09:11:14.793 6690 6735 V unknown:RequestLoggingListener: time 11201793: onProducerStart: {requestId: 1, producer: NetworkFetchProducer}
08-12 09:11:15.161 6690 7358 V unknown:RequestLoggingListener: time 11202161: onProducerFinishWithSuccess: {requestId: 1, producer: NetworkFetchProducer, elapsedTime: 368 ms, extraMap: null}
08-12 09:11:15.162 6690 6742 V unknown:BufferedDiskCache: About to write to disk-cache for key http://www.example.com/image.jpg
08-12 09:11:15.162 6690 6734 V unknown:RequestLoggingListener: time 11202162: onProducerStart: {requestId: 1, producer: DecodeProducer}
08-12 09:11:15.163 6690 6742 V unknown:BufferedDiskCache: Successful disk-cache write for key http://www.example.com/image.jpg
08-12 09:11:15.169 6690 6734 V unknown:RequestLoggingListener: time 11202169: onProducerFinishWithSuccess: {requestId: 1, producer: DecodeProducer, elapsedTime: 7 ms, extraMap: {hasGoodQuality=true, queueTime=0, bitmapSize=600x400, isFinal=true}}
08-12 09:11:15.169 6690 6734 V unknown:RequestLoggingListener: time 11202169: onRequestSuccess: {requestId: 1, elapsedTime: 378 ms}
08-12 09:11:15.184 6690 6690 V unknown:AbstractDraweeController: controller 28ebe0eb 1: set_final_result @ onNewResult: image: CloseableReference 2fd41bb0

在这个示例中，我们发现名为28ebe0eb的 DraweeView 向名为36e95857的 DataSource 进行了图像请求。
首先，图片没有在内存缓存中找到，也没有在磁盘缓存中找到，最后去网络上下载图片。下载成功后，图片被解码，之后请求结束。
最后数据源通知 controller 图片就绪，显示图片(set_final_result）。

```

## 8.2：一些陷阱


1. 不要使用 ScrollViews界

```
如果你想要在一个长的图片列表中滑动，你应该使用 RecyclerView，ListView，或 GridView。这三者都会在你滑动时不断重用子视图。Fresco 的 view 会接收系统事件，使它们能正确管理内存。
ScrollView 不会这样做。因此，Fresco view 不会被告知它们是否在屏幕上显示，并保持图片内存占用直到你的 Fragment 或 Activity 停止。
你的 App 将会面临更大的 OOM 风险。
```

2. 不要向下转换


```
不要试图把Fresco返回的一些对象进行向下转化，这也许会带来一些对象操作上的便利
，但是也许在后续的版本中，你会遇到一些因为向下转换特性丢失导致的难以处理的问题。
```
 
3. 不要使用getTopLevelDrawable


```
DraweeHierarchy.getTopLevelDrawable() 仅仅 应该在DraweeViews中用，除了定义View中，其他应用代码建议连碰都不要碰这个。

在自定义view中，也千万不要将返回值向下转换，也许下个版本，我们会更改这个返回值类型。
```

4. 不要复用 DraweeHierarchies

```
永远不要把 DraweeHierarchy 通过 DraweeView.setHierarchy 设置给不同的View。
DraweeHierarchy 是由一系列 Drawable 组成的。在 Android 中, Drawable 不能被多个 View 共享。
```

5. 不要在多个DraweeHierarchy中使用同一个Drawable

```
原因同上。不过你可以在占位图、重试图、错误图中使用相同的资源ID，Android 实际会创建不同的 Drawable。

如果你使用GenericDraweeHierarchyBuilder，那么需要调用Resources.getDrawable来通过资源获取图片。

不过请不要只调用一次然后将结果传给不同的Hierarchy！
```

6. 不要直接控制 hierarchy

```
不要直接使用 SettableDraweeHierarchy 方法(reset，setImage，…)。

它们应该仅由 controller 使用。

不要使用setControllerOverlay来设置一个覆盖图，这个方法只能给 controller 调用。
如果你需要显示覆盖图，可以参考Drawee的各种效果配置
```

7. 不要直接给 DraweeView 设置图片

```
目前 DraweeView 直接继承于 ImageView，因此它有 setImageBitmap,
setImageDrawable 等方法。
如果利用这些方法直接设置一张图片，内部的 DraweeHierarchy 就会丢失，也就无法取到image
pipeline 的任何图像了。
```

8. 使用 DraweeView 时，请不要使用任何 ImageView 的属性

```
在后续的版本中，DraweeView 会直接从 View 派生。
任何属于 ImageView 但是不属于 View 的方法都会被移除。

```


## 8.3：为什么不支持wrap_content


```
人们经常会问，为什么Fresco中不可以使用wrap_content？

主要的原因是，Drawee永远会在getIntrinsicHeight/getIntrinsicWidth中返回-1。
这么做的原因是 Drawee 不像ImageView一样。它同一时刻可能会显示多个元素。比如在从占位图渐变到目标图
时，两张图会有同时显示的时候。再比如可能有多张目标图片（低清晰度、高清晰度两张）。如果这些图像都是不同的尺寸，那么很难定义”intrinsic”尺寸。

如果我们要先用占位图的尺寸，等加载完成后再使用真实图的尺寸，那么图片很可能显
示错误。它可能会被根据占位图的尺寸来缩放、裁剪。
唯一防止这种事情的方式就只有在图片加载完成后强制触发一次layout。这样的话不仅
会影响性能，而且会让应用的界面突变，很影响用户体验！
如果用户正在读一篇文章，然后在图片加载完成后整篇文章突然向下移动，这是非常不好的。

所以你必须指定尺寸或者用match_parent来布局。

你如果从服务端请求图片，服务端可以做到返回图片尺寸。然后你拿到之后通过setLayoutParams 来给View设置宽高。

当然如果你必须要使用wrap_content，那么你可以参考StackOverflow上的一个回答。
但是我们以后会移除这个功能，Ugly things should look ugly。
```


## 8.4：共享元素动画


```
使用 ChangeBounds，而不是ChangeImageTransform

Android 5.0 (Lollipop) 引入了 共享元素动画，允许在多个Activity切换时共享相同的View！

你可以在XML中定义这个变换。有个ChangeImageTransform变换可以在共享元素切换时对ImageView进行变换，可惜Fresco暂时不支持它，因为Drawee维护着自己的转换Matrix。

幸运的是你可以有另一种做法：使用ChangeBounds。你可以改变layout的边界，这样Fresco会根据它进行自适应，也能够达到你想要的功能。
```

# 9：总结

Fresco的使用在一定程度上要比Picasso、GLide要复杂，但是在强大的Fresco面前，复杂并不是理由，只要灵活的使用Fresco，一定会给我们的开发带来便利。

有关Fresco的任何问题可以查看Fresco的中文API网址来寻求解决方案。

https://www.fresco-cn.org/docs/index.html


> 欢迎访问[201216323.tech](http://www.201216323.tech)来查看我的CSDN博客。

> 欢迎关注我的个人技术公众号,快速查看我的最新文章。

![我的公众号图片](http://img.blog.csdn.net/20161220174646569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2NnXzIwMTIxNjMyMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "bruce常")