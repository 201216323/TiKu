对Glide库不了解的朋友,可以先看看[这篇文章](http://blog.csdn.net/mchenys/article/details/51599039) Glide图片加载库的使用

本库主要有3部分组成,分别是:Glide的配置类,Glide的加载类,Glide的加载监听接口.

配置类

```
package com.example.mchenys.httputilsdemo.image.glide.cofig;

import android.graphics.Bitmap;
import android.view.View;

import com.bumptech.glide.Priority;
import com.bumptech.glide.load.engine.DiskCacheStrategy;
import com.bumptech.glide.load.resource.drawable.GlideDrawable;
import com.bumptech.glide.request.animation.ViewPropertyAnimation;
import com.bumptech.glide.request.target.AppWidgetTarget;
import com.bumptech.glide.request.target.NotificationTarget;
import com.bumptech.glide.request.target.SimpleTarget;
import com.bumptech.glide.request.target.ViewTarget;

/**
 * Created by mChenys on 2016/4/29.
 */
public class ImageLoadConfig {
    public static final int CENTER_CROP = 0;
    public static final int FIT_CENTER = 1;
    private Integer placeHolderResId; //默认占位资源
    private Integer errorResId; //错误时显示的资源
    private boolean crossFade; //是否淡入淡出动画
    private int crossDuration; //淡入淡出动画持续的时间
    private OverrideSize size; //图片最终显示在ImageView上的宽高度像素
    private int CropType = CENTER_CROP; //裁剪类型,默认为中部裁剪
    private boolean asGif; //true,强制显示的是gif动画,如果url不是gif的资源,那么会回调error()
    private boolean asBitmap;//true,强制显示为常规图片,如果是gif资源,则加载第一帧作为图片
    private boolean skipMemoryCache;//true,跳过内存缓存,默认false
    private DiskCache diskCacheStrategy; //硬盘缓存,默认为all类型
    private LoadPriority prioriy;
    private float thumbnail;//设置缩略图的缩放比例0.0f-1.0f,如果缩略图比全尺寸图先加载完，就显示缩略图，否则就不显示
    private String thumbnailUrl;//设置缩略图的url,如果缩略图比全尺寸图先加载完，就显示缩略图，否则就不显示
    private SimpleTarget<Bitmap> simpleTarget; //指定simpleTarget对象,可以在Target回调方法中处理bitmap,同时该构造方法中还可以指定size
    private ViewTarget<? extends View, GlideDrawable> viewTarget;//指定viewTarget对象,可以是自定义View,该构造方法传入的view和通配符的view是同一个
    private NotificationTarget notificationTarget; //设置通知栏加载大图片的target;
    private AppWidgetTarget appWidgetTarget;//设置加载小部件图片的target
    private Integer animResId;//图片加载完后的动画效果,在异步加载资源完成时会执行该动画。
    private ViewPropertyAnimation.Animator animator; //在异步加载资源完成时会执行该动画。可以接受一个Animator对象
    private boolean cropCircle;//圆形裁剪
    private boolean roundedCorners;//圆角处理
    private boolean grayscale;//灰度处理
    private boolean blur;//高斯模糊处理
    private boolean rotate;//旋转图片
    private int rotateDegree;//默认旋转°
    private String tag; //唯一标识
    /**
     * 硬盘缓存策略
     */
    public enum DiskCache {
        NONE(DiskCacheStrategy.NONE),//跳过硬盘缓存
        SOURCE(DiskCacheStrategy.SOURCE),//仅仅保存原始分辨率的图片
        RESULT(DiskCacheStrategy.RESULT),//仅仅缓存最终分辨率的图像(降低分辨率后的或者转换后的)
        ALL(DiskCacheStrategy.ALL);//缓存所有版本的图片,默认行为
        private DiskCacheStrategy strategy;

        DiskCache(DiskCacheStrategy strategy) {
            this.strategy = strategy;
        }

        public DiskCacheStrategy getStrategy() {
            return strategy;
        }
    }

    /**
     * 加载优先级策略
     */
    public enum LoadPriority {
        LOW(Priority.LOW),
        NORMAL(Priority.NORMAL),
        HIGH(Priority.HIGH),
        IMMEDIATE(Priority.IMMEDIATE),;
        Priority priority;

        LoadPriority(Priority priority) {
            this.priority = priority;
        }

        public Priority getPriority() {
            return priority;
        }
    }

    private ImageLoadConfig(ImageLoadConfig.Builder builder) {
        this.placeHolderResId = builder.placeHolderResId;
        this.errorResId = builder.errorResId;
        this.crossFade = builder.crossFade;
        this.crossDuration = builder.crossDuration;
        this.size = builder.size;
        this.CropType = builder.CropType;
        this.asGif = builder.asGif;
        this.asBitmap = builder.asBitmap;
        this.skipMemoryCache = builder.skipMemoryCache;
        this.diskCacheStrategy = builder.diskCacheStrategy;
        this.thumbnail = builder.thumbnail;
        this.thumbnailUrl = builder.thumbnailUrl;
        this.simpleTarget = builder.simpleTarget;
        this.viewTarget = builder.viewTarget;
        this.notificationTarget = builder.notificationTarget;
        this.appWidgetTarget = builder.appWidgetTarget;
        this.animResId = builder.animResId;
        this.animator = builder.animator;
        this.prioriy = builder.prioriy;
        this.blur = builder.blur;
        this.cropCircle = builder.cropCircle;
        this.roundedCorners = builder.roundedCorners;
        this.grayscale = builder.grayscale;
        this.rotate =builder.rotate;
        this.rotateDegree = builder.rotateDegree;
        this.tag = tag;
    }
    /**
    *Builder类
    */
    public static class Builder {
        private Integer placeHolderResId;
        private Integer errorResId;
        private boolean crossFade;
        private int crossDuration;
        private OverrideSize size;
        private int CropType = CENTER_CROP;
        private boolean asGif;
        private boolean asBitmap;
        private boolean skipMemoryCache;
        private DiskCache diskCacheStrategy = DiskCache.ALL;
        private LoadPriority prioriy = LoadPriority.HIGH;
        private float thumbnail;
        private String thumbnailUrl;
        private SimpleTarget<Bitmap> simpleTarget;
        private ViewTarget<? extends View, GlideDrawable> viewTarget;
        private NotificationTarget notificationTarget;
        private AppWidgetTarget appWidgetTarget;
        private Integer animResId;
        private ViewPropertyAnimation.Animator animator;
        private boolean cropCircle;
        private boolean roundedCorners;
        private boolean grayscale;
        private boolean blur;
        private boolean rotate;
        private int rotateDegree =90;
        private String tag;

        public Builder setPlaceHolderResId(Integer placeHolderResId) {
            this.placeHolderResId = placeHolderResId;
            return this;
        }

        public Builder setErrorResId(Integer errorResId) {
            this.errorResId = errorResId;
            return this;
        }

        public Builder setCrossFade(boolean crossFade) {
            this.crossFade = crossFade;
            return this;
        }

        public Builder setCrossDuration(int crossDuration) {
            this.crossDuration = crossDuration;
            return this;
        }

        public Builder setSize(OverrideSize size) {
            this.size = size;
            return this;
        }

        public Builder setCropType(int cropType) {
            CropType = cropType;
            return this;
        }

        public Builder setAsGif(boolean asGif) {
            this.asGif = asGif;
            return this;
        }

        public Builder setAsBitmap(boolean asBitmap) {
            this.asBitmap = asBitmap;
            return this;
        }

        public Builder setSkipMemoryCache(boolean skipMemoryCache) {
            this.skipMemoryCache = skipMemoryCache;
            return this;
        }

        public Builder setDiskCacheStrategy(DiskCache diskCacheStrategy) {
            this.diskCacheStrategy = diskCacheStrategy;
            return this;
        }

        public Builder setPrioriy(LoadPriority prioriy) {
            this.prioriy = prioriy;
            return this;
        }

        public Builder setThumbnail(float thumbnail) {
            this.thumbnail = thumbnail;
            return this;
        }

        public Builder setThumbnailUrl(String thumbnailUrl) {
            this.thumbnailUrl = thumbnailUrl;
            return this;
        }

        public Builder setSimpleTarget(SimpleTarget<Bitmap> simpleTarget) {
            this.simpleTarget = simpleTarget;
            return this;
        }

        public Builder setViewTarget(ViewTarget<? extends View, GlideDrawable> viewTarget) {
            this.viewTarget = viewTarget;
            return this;
        }

        public Builder setNotificationTarget(NotificationTarget notificationTarget) {
            this.notificationTarget = notificationTarget;
            return this;
        }

        public Builder setAppWidgetTarget(AppWidgetTarget appWidgetTarget) {
            this.appWidgetTarget = appWidgetTarget;
            return this;
        }

        public Builder setAnimResId(Integer animResId) {
            this.animResId = animResId;
            return this;
        }

        public Builder setAnimator(ViewPropertyAnimation.Animator animator) {
            this.animator = animator;
            return this;
        }

        public Builder setCropCircle(boolean cropCircle) {
            this.cropCircle = cropCircle;
            return this;
        }

        public Builder setRoundedCorners(boolean roundedCorners) {
            this.roundedCorners = roundedCorners;
            return this;
        }

        public Builder setGrayscale(boolean grayscale) {
            this.grayscale = grayscale;
            return this;
        }

        public Builder setBlur(boolean blur) {
            this.blur = blur;
            return this;
        }

        public Builder setRotate(boolean rotate) {
            this.rotate = rotate;
            return this;
        }

        public Builder setRotateDegree(int rotateDegree) {
            this.rotateDegree = rotateDegree;
            return this;
        }

        public Builder setTag(String tag) {
            this.tag = tag;
            return this;
        }

        public ImageLoadConfig build() {
            return new ImageLoadConfig(this);
        }
    }

    public static Builder parseBuilder(ImageLoadConfig config) {
        Builder builder = new Builder();
        builder.placeHolderResId = config.placeHolderResId;
        builder.errorResId = config.errorResId;
        builder.crossFade = config.crossFade;
        builder.crossDuration = config.crossDuration;
        builder.size = config.size;
        builder.CropType = config.CropType;
        builder.asGif = config.asGif;
        builder.asBitmap = config.asBitmap;
        builder.skipMemoryCache = config.skipMemoryCache;
        builder.diskCacheStrategy = config.diskCacheStrategy;
        builder.thumbnail = config.thumbnail;
        builder.thumbnailUrl = config.thumbnailUrl;
        builder.simpleTarget = config.simpleTarget;
        builder.viewTarget = config.viewTarget;
        builder.notificationTarget = config.notificationTarget;
        builder.appWidgetTarget = config.appWidgetTarget;
        builder.animResId = config.animResId;
        builder.animator = config.animator;
        builder.prioriy = config.prioriy;
        builder.cropCircle = config.cropCircle;
        builder.roundedCorners = config.roundedCorners;
        builder.grayscale = config.grayscale;
        builder.blur = config.blur;
        builder.rotate = config.rotate;
        builder.rotateDegree =config.rotateDegree;
        builder.tag = config.tag;
        return builder;
    }

    public Integer getPlaceHolderResId() {
        return placeHolderResId;
    }

    public Integer getErrorResId() {
        return errorResId;
    }

    public boolean isCrossFade() {
        return crossFade;
    }

    public int getCrossDuration() {
        return crossDuration;
    }

    public OverrideSize getSize() {
        return size;
    }

    public int getCropType() {
        return CropType;
    }

    public boolean isAsGif() {
        return asGif;
    }

    public boolean isAsBitmap() {
        return asBitmap;
    }

    public boolean isSkipMemoryCache() {
        return skipMemoryCache;
    }

    public DiskCache getDiskCacheStrategy() {
        return diskCacheStrategy;
    }

    public LoadPriority getPrioriy() {
        return prioriy;
    }

    public float getThumbnail() {
        return thumbnail;
    }

    public String getThumbnailUrl() {
        return thumbnailUrl;
    }

    public SimpleTarget<Bitmap> getSimpleTarget() {
        return simpleTarget;
    }

    public ViewTarget<? extends View, GlideDrawable> getViewTarget() {
        return viewTarget;
    }

    public NotificationTarget getNotificationTarget() {
        return notificationTarget;
    }

    public AppWidgetTarget getAppWidgetTarget() {
        return appWidgetTarget;
    }

    public Integer getAnimResId() {
        return animResId;
    }

    public ViewPropertyAnimation.Animator getAnimator() {
        return animator;
    }

    public boolean isCropCircle() {
        return cropCircle;
    }

    public boolean isRoundedCorners() {
        return roundedCorners;
    }

    public boolean isGrayscale() {
        return grayscale;
    }

    public boolean isBlur() {
        return blur;
    }

    public boolean isRotate() {
        return rotate;
    }

    public int getRotateDegree() {
        return rotateDegree;
    }

    public String getTag() {
        return tag;
    }

    /**
     * 图片最终显示在ImageView上的宽高像素
     * Created by mChenys on 2016/4/29.
     */
    public static class OverrideSize {
        private final int width;
        private final int height;

        public OverrideSize(int width, int height) {
            this.width = width;
            this.height = height;
        }

        public int getWidth() {
            return width;
        }

        public int getHeight() {
            return height;
        }
    }

}
```
加载监听接口

有2个

```
public interface LoaderListener {

    void onSuccess();

    void onError();
}
public interface BitmapLoadingListener {

    void onSuccess(Bitmap b);

    void onError();
}
```
加载类

```
/**
 * Created by mChenys on 2016/4/29.
 */
public class ImageLoader {
    //默认配置
    public static ImageLoadConfig defConfig = new ImageLoadConfig.Builder().
            setCropType(ImageLoadConfig.CENTER_CROP).
            setAsBitmap(true).
            setPlaceHolderResId(R.drawable.bg_loading).
            setErrorResId(R.drawable.bg_error).
            setDiskCacheStrategy(ImageLoadConfig.DiskCache.SOURCE).
            setPrioriy(ImageLoadConfig.LoadPriority.HIGH).build();

    /**
     * 加载String类型的资源
     * SD卡资源："file://"+ Environment.getExternalStorageDirectory().getPath()+"/test.jpg"<p/>
     * assets资源："file:///android_asset/f003.gif"<p/>
     * raw资源："Android.resource://com.frank.glide/raw/raw_1"或"android.resource://com.frank.glide/raw/"+R.raw.raw_1<p/>
     * drawable资源："android.resource://com.frank.glide/drawable/news"或load"android.resource://com.frank.glide/drawable/"+R.drawable.news<p/>
     * ContentProvider资源："content://media/external/images/media/139469"<p/>
     * http资源："http://img.my.csdn.net/uploads/201508/05/1438760757_3588.jpg"<p/>
     * https资源："https://img.alicdn.com/tps/TB1uyhoMpXXXXcLXVXXXXXXXXXX-476-538.jpg_240x5000q50.jpg_.webp"<p/>
     *
     * @param view
     * @param imageUrl
     * @param config
     * @param listener
     */
    public static void loadStringRes(ImageView view, String imageUrl, ImageLoadConfig config, LoaderListener listener) {
        load(view.getContext(), view, imageUrl, config, listener);
    }

    public static void loadFile(ImageView view, File file, ImageLoadConfig config, LoaderListener listener) {
        load(view.getContext(), view, file, config, listener);
    }

    public static void loadResId(ImageView view, Integer resourceId, ImageLoadConfig config, LoaderListener listener) {
        load(view.getContext(), view, resourceId, config, listener);
    }

    public static void loadUri(ImageView view, Uri uri, ImageLoadConfig config, LoaderListener listener) {
        load(view.getContext(), view, uri, config, listener);
    }

    public static void loadGif(ImageView view, String gifUrl, ImageLoadConfig config, LoaderListener listener) {
        load(view.getContext(), view, gifUrl, ImageLoadConfig.parseBuilder(config).setAsGif(true).build(), listener);
    }

    public static void loadTarget(Context context, Object objUrl, ImageLoadConfig config, final LoaderListener listener) {
        load(context, null, objUrl, config, listener);
    }

    private static void load(Context context, ImageView view, Object objUrl, ImageLoadConfig config, final LoaderListener listener) {
        if (null == objUrl) {
            throw new IllegalArgumentException("objUrl is null");
        } else if (null == config) {
            config = defConfig;
        }
        try {
            GenericRequestBuilder builder = null;
            if (config.isAsGif()) {//gif类型
                GifRequestBuilder request = Glide.with(context).load(objUrl).asGif();
                if (config.getCropType() == ImageLoadConfig.CENTER_CROP) {
                    request.centerCrop();
                } else {
                    request.fitCenter();
                }
                builder = request;
            } else if (config.isAsBitmap()) {  //bitmap 类型
                BitmapRequestBuilder request = Glide.with(context).load(objUrl).asBitmap();
                if (config.getCropType() == ImageLoadConfig.CENTER_CROP) {
                    request.centerCrop();
                } else {
                    request.fitCenter();
                }
                //transform bitmap
                if (config.isRoundedCorners()) {
                    request.transform(new RoundedCornersTransformation(context, 50, 50));
                } else if (config.isCropCircle()) {
                    request.transform(new CropCircleTransformation(context));
                } else if (config.isGrayscale()) {
                    request.transform(new GrayscaleTransformation(context));
                } else if (config.isBlur()) {
                    request.transform(new BlurTransformation(context, 8, 8));
                } else if (config.isRotate()) {
                    request.transform(new RotateTransformation(context, config.getRotateDegree()));
                }
                builder = request;
            } else if (config.isCrossFade()) { // 渐入渐出动画
                DrawableRequestBuilder request = Glide.with(context).load(objUrl).crossFade();
                if (config.getCropType() == ImageLoadConfig.CENTER_CROP) {
                    request.centerCrop();
                } else {
                    request.fitCenter();
                }
                builder = request;
            }
            //缓存设置
            builder.diskCacheStrategy(config.getDiskCacheStrategy().getStrategy()).
                    skipMemoryCache(config.isSkipMemoryCache()).
                    priority(config.getPrioriy().getPriority());
            builder.dontAnimate();
            if (null != config.getTag()) {
                builder.signature(new StringSignature(config.getTag()));
            } else {
                builder.signature(new StringSignature(objUrl.toString()));
            }
            if (null != config.getAnimator()) {
                builder.animate(config.getAnimator());
            } else if (null != config.getAnimResId()) {
                builder.animate(config.getAnimResId());
            }
            if (config.getThumbnail() > 0.0f) {
                builder.thumbnail(config.getThumbnail());
            }
            if (null != config.getErrorResId()) {
                builder.error(config.getErrorResId());
            }
            if (null != config.getPlaceHolderResId()) {
                builder.placeholder(config.getPlaceHolderResId());
            }
            if (null != config.getSize()) {
                builder.override(config.getSize().getWidth(), config.getSize().getHeight());
            }
            if (null != listener) {
                setListener(builder, listener);
            }
            if (null != config.getThumbnailUrl()) {
                BitmapRequestBuilder thumbnailRequest = Glide.with(context).load(config.getThumbnailUrl()).asBitmap();
                builder.thumbnail(thumbnailRequest).into(view);
            } else {
                setTargetView(builder, config, view);
            }
        } catch (Exception e) {
            view.setImageResource(config.getErrorResId());
        }
    }

    private static void setListener(GenericRequestBuilder request, final LoaderListener listener) {
        request.listener(new RequestListener() {
            @Override
            public boolean onException(Exception e, Object model, Target target, boolean isFirstResource) {
                if (!e.getMessage().equals("divide by zero")) {
                    listener.onError();
                }
                return false;
            }

            @Override
            public boolean onResourceReady(Object resource, Object model, Target target, boolean isFromMemoryCache, boolean isFirstResource) {
                listener.onSuccess();
                return false;
            }
        });
    }

    private static void setTargetView(GenericRequestBuilder request, ImageLoadConfig config, ImageView view) {
        //set targetView
        if (null != config.getSimpleTarget()) {
            request.into(config.getSimpleTarget());
        } else if (null != config.getViewTarget()) {
            request.into(config.getViewTarget());
        } else if (null != config.getNotificationTarget()) {
            request.into(config.getNotificationTarget());
        } else if (null != config.getAppWidgetTarget()) {
            request.into(config.getAppWidgetTarget());
        } else {
            request.into(view);
        }
    }

    /**
     * 加载bitmap
     *
     * @param context
     * @param url
     * @param listener
     */
    public static void loadBitmap(Context context, Object url, final BitmapLoadingListener listener) {
        if (url == null) {
            if (listener != null) {
                listener.onError();
            }
        } else {
            Glide.with(context).
                    load(url).
                    asBitmap().
                    diskCacheStrategy(DiskCacheStrategy.NONE).
                    dontAnimate().
                    into(new SimpleTarget<Bitmap>() {
                        @Override
                        public void onResourceReady(Bitmap resource, GlideAnimation<? super Bitmap> glideAnimation) {
                            if (listener != null) {
                                listener.onSuccess(resource);
                            }
                        }
                    });
        }
    }

    /**
     * 高优先级加载
     *
     * @param url
     * @param imageView
     * @param listener
     */
    public static void loadImageWithHighPriority(Object url, ImageView imageView, final LoaderListener listener) {
        if (url == null) {
            if (listener != null) {
                listener.onError();
            }
        } else {
            Glide.with(imageView.getContext()).
                    load(url).
                    asBitmap().
                    priority(Priority.HIGH).
                    dontAnimate().
                    listener(new RequestListener<Object, Bitmap>() {
                        @Override
                        public boolean onException(Exception e, Object model, Target<Bitmap> target, boolean isFirstResource) {
                            if (null != listener) {
                                listener.onError();
                            }
                            return false;
                        }

                        @Override
                        public boolean onResourceReady(Bitmap resource, Object model, Target<Bitmap> target, boolean isFromMemoryCache, boolean isFirstResource) {
                            if (null != listener) {
                                listener.onSuccess();
                            }
                            return false;
                        }
                    }).into(imageView);
        }
    }

    /**
     * 取消所有正在下载或等待下载的任务。
     */
    public static void cancelAllTasks(Context context) {
        Glide.with(context).pauseRequests();
    }

    /**
     * 恢复所有任务
     */
    public static void resumeAllTasks(Context context) {
        Glide.with(context).resumeRequests();
    }

    /**
     * 清除磁盘缓存
     *
     * @param context
     */
    public static void clearDiskCache(final Context context) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                Glide.get(context).clearDiskCache();
            }
        }).start();
    }
     /**
     * 清除所有缓存
     * @param context
     */
    public static void cleanAll(Context context) {
        clearDiskCache(context);
        Glide.get(context).clearMemory();
    }
    /**
     * 获取缓存大小
     *
     * @param context
     * @return
     */
    public static synchronized long getDiskCacheSize(Context context) {
        long size = 0L;
        File cacheDir = PathUtils.getDiskCacheDir(context, CacheConfig.IMG_DIR);

        if (cacheDir != null && cacheDir.exists()) {
            File[] files = cacheDir.listFiles();
            if (files != null) {
                File[] arr$ = files;
                int len$ = files.length;

                for (int i$ = 0; i$ < len$; ++i$) {
                    File imageCache = arr$[i$];
                    if (imageCache.isFile()) {
                        size += imageCache.length();
                    }
                }
            }
        }

        return size;
    }

    public static void clearTarget(Context context, String uri) {
        if (SimpleGlideModule.cache != null && uri != null) {
            SimpleGlideModule.cache.delete(new StringSignature(uri));
            Glide.get(context).clearMemory();
        }
    }

    public static void clearTarget(View view) {
        Glide.clear(view);
    }

    public static File getTarget(Context context, String uri) {
        return SimpleGlideModule.cache != null && uri != null ? SimpleGlideModule.cache.get(new StringSignature(uri)) : null;
    }
}
```
测试类

```
/**
 * Created by mChenys on 2016/4/30.
 */
public class GlideActivity extends AppCompatActivity implements View.OnClickListener {
    private ImageView mTargetView;
    private String url = "http://www.qq745.com/uploads/allimg/141106/1-141106153Q5.png";
    private String gif = "http://image24.360doc.com/DownloadImg/2011/03/0513/9707070_3.gif";
    private String thumbnailUrl = "http://img.my.csdn.net/uploads/201407/26/1406383299_1976.jpg";
    private File jpgFile = new File(Environment.getExternalStorageDirectory().getAbsolutePath() + "/droid4xshare/test.jpg");

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_glide);
        mTargetView = (ImageView) findViewById(R.id.iv_target);
        findViewById(R.id.btn_load_bimap).setOnClickListener(this);
        findViewById(R.id.btn_del_bimap).setOnClickListener(this);
        findViewById(R.id.btn_load_gif).setOnClickListener(this);
        findViewById(R.id.btn_load_file).setOnClickListener(this);
        findViewById(R.id.btn_load_resource).setOnClickListener(this);
        findViewById(R.id.btn_load_animid).setOnClickListener(this);
        findViewById(R.id.btn_load_anim).setOnClickListener(this);
        findViewById(R.id.btn_load_thumbnailUrl).setOnClickListener(this);
        findViewById(R.id.btn_load_thumbnailSize).setOnClickListener(this);

        Glide.get(GlideActivity.this).clearMemory();
        new Thread(new Runnable() {
            @Override
            public void run() {
                Glide.get(GlideActivity.this).clearDiskCache();
            }
        }).start();
    }

    @Override
    public void onClick(View v) {
        ImageLoadConfig.OverrideSize size = new ImageLoadConfig.OverrideSize(100, 100);
        switch (v.getId()) {
            case R.id.btn_del_bimap://删除已加载过url的图片
                ImageLoader.clearTarget(this, url);
                break;
            case R.id.btn_load_bimap: //加载网络图片
                ImageLoader.loadStringRes(mTargetView, url, ImageLoader.defConfig, mListener);
                break;
            case R.id.btn_load_gif://加载网络的gif
                ImageLoadConfig config2 = ImageLoadConfig.parseBuilder(ImageLoader.defConfig).setAsGif(true).
                        setSkipMemoryCache(true).
                        setDiskCacheStrategy(ImageLoadConfig.DiskCache.NONE).
                        build();
                ImageLoader.loadGif(mTargetView, gif, config2, mListener);
                break;
            case R.id.btn_load_file://加载本地图片
                ImageLoadConfig config3 = ImageLoadConfig.parseBuilder(ImageLoader.defConfig).
                        setSkipMemoryCache(true).
                        setDiskCacheStrategy(ImageLoadConfig.DiskCache.NONE).setSize(size).
                        build();
                ImageLoader.loadFile(mTargetView, jpgFile, config3, mListener);
                break;
            case R.id.btn_load_resource://加载本地资源
                ImageLoadConfig config4 = ImageLoadConfig.parseBuilder(ImageLoader.defConfig).
                        setSkipMemoryCache(true).
                        setDiskCacheStrategy(ImageLoadConfig.DiskCache.NONE)
                        .build();
                ImageLoader.loadResId(mTargetView, R.drawable.dog, config4, mListener);
                break;
            case R.id.btn_load_animid://加载资源动画
                ImageLoadConfig config5 = ImageLoadConfig.parseBuilder(ImageLoader.defConfig).
                        setAnimResId(R.anim.left_in).
                        setSkipMemoryCache(true).
                        setDiskCacheStrategy(ImageLoadConfig.DiskCache.NONE).
                        setAsGif(true).build();
                ImageLoader.loadResId(mTargetView, R.drawable.smail, config5, mListener);
                break;
            case R.id.btn_load_anim://加载属性动画
                ViewPropertyAnimation.Animator animationObject = new ViewPropertyAnimation.Animator() {
                    @Override
                    public void animate(View view) {
                        ObjectAnimator moveIn = ObjectAnimator.ofFloat(view, "translationX", -500f, 0f);
                        ObjectAnimator rotate = ObjectAnimator.ofFloat(view, "rotation", 0f, 360f);
                        ObjectAnimator fadeInOut = ObjectAnimator.ofFloat(view, "alpha", 1f, 0f, 1f);
                        ObjectAnimator moveTop = ObjectAnimator.ofFloat(view, "translationY", 0f, -2000, 0f);
                        AnimatorSet animSet = new AnimatorSet();
                        animSet.play(rotate).with(fadeInOut).after(moveIn).before(moveTop);
                        animSet.setDuration(5000);
                        animSet.start();
                    }
                };
                ImageLoadConfig config6 = ImageLoadConfig.parseBuilder(ImageLoader.defConfig).
                        setAnimator(animationObject).
                        setSkipMemoryCache(true).
                        setDiskCacheStrategy(ImageLoadConfig.DiskCache.NONE).
                        setAsGif(true).
                        build();
                ImageLoader.loadResId(mTargetView, R.drawable.smail, config6, mListener);
                break;


            case R.id.btn_load_thumbnailUrl://先加载缩略图片,再显示原图
                ImageLoadConfig config7 = ImageLoadConfig.parseBuilder(ImageLoader.defConfig).
                        setSkipMemoryCache(true).
                        setDiskCacheStrategy(ImageLoadConfig.DiskCache.NONE).
                        setThumbnailUrl(thumbnailUrl)
                        .build();
                ImageLoader.loadStringRes(mTargetView, url, config7, mListener);
                break;
            case R.id.btn_load_thumbnailSize:
                ImageLoadConfig config8 = ImageLoadConfig.parseBuilder(ImageLoader.defConfig).
                        setSkipMemoryCache(true).
                        setDiskCacheStrategy(ImageLoadConfig.DiskCache.NONE).
                        setThumbnail(0.7f)
                        .build();
                ImageLoader.loadStringRes(mTargetView, url, config8, mListener);
                break;

        }
    }

    private LoaderListener mListener = new LoaderListener() {

        @Override
        public void onSuccess() {
            System.out.println("onSuccess()");
        }

        @Override
        public void onError() {
            System.out.println("onError()");
        }
    };

}
```


