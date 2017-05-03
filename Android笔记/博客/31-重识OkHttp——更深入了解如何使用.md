# 重识OkHttp——更深入了解如何使用

> 本文的分析基于[OkHttp3.4](https://github.com/square/okhttp/tree/okhttp_34)，不展示完整的代码示例，具体可以查看[这个官方例子](https://github.com/square/okhttp/wiki/Recipes)或者项目中的samples。

OkHttp作为square公司出品的一个网络请求框架，应该算是目前Android端最火爆的网络框架了。我公司目前的项目中采用的都是Rxjava结合Retrofit进行网络请求的处理，对于底层真正实现网络请求的OkHttp关注的不是很多。最近探究了一下OkHttp的源码，对OkHttp的使用有了一些新的认识，在此做一下总结。

## 1 OkHttp的优点

OkHttp作为当前Android端最火热的网络请求框架，必然有很多的优点。

1. 支持HTTP/2 协议，允许连接到同一个主机地址的所有请求共享Socket。这必然会提高请求效率。
1. 在HTTP/2协议不可用的情况下，通过连接池减少请求的延迟。
1. GZip透明压缩减少传输的数据包大小。
1. 响应缓存，避免同一个重复的网络请求。

## 2 网络处理3要素

对于客户端来讲，我们关注的就是把正确的请求发送到服务端并拿到结果来进行处理。在OkHttp中，我认为可以分为3个部分：

Request类封装客户端发送的请求，包括请求的url，请求方法method（主要是GET和POST方法）、请求头header以及请求体requestBody；
Response类封装了服务器响应的数据，包括code、message、body、header等。

OkHttpClient负责发送请求Request并通过同步或者异步的方式返回服务器的响应Response，就好比是一个浏览器。

OkHttp中通过**建造者模式**来构建OkHttpClient、Request和Response。对于客户端来讲，我们不需要过多关注Response是如何构建的，因为这个是OkHttp对响应结果进行了封装处理。我们只关注**请求Request**和**客户端OkHttpClient**如何构建即可。

### 2.1 请求Request
Request采用建造者模式来配置url，请求方法method、header、tag和cacheControl。

1. 设置url。可以是String类型、URL类型和HttpUrl类型。最终都是用到HttpUrl类型。
1. 设置method，包含get、post方法等。默认的是get方法。post方法要传RequestBody，类似的还有delete、put、patch。
1. 设置header，方法有addHeader(String name, String value)、 removeHeader(String name)、header(String name, String value)、headers(Headers headers)。**headers(Headers headers)调用之后其它的header都会被移除，只添加这一个header。**   **而header(String name, String value)方法调用之后，其它与这个name同名的header都会被移除，只保留这一个header。**
1. 设置tag，设置tag可以用来取消这一请求。如果未指定tag或者tag为null，那么这个**request本身**就会当做是一个tag用来被取消请求。
1. 设置cacheControl，这个是设置到请求头中。用来替换其它name是"Cache-Control"的header。如果**cacheControl是空的话就会移除请求头中name是"Cache-Control"的header**。

![image](http://upload-images.jianshu.io/upload_images/2083810-256d5a86abb5e29b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

OkHttp采用POST方法向服务器发送一个请求体，在OkHttp中这个请求体是RequestBody。这个请求体可以是：

1. String类型
1. Stream流类型
1. File文件类型
1. Form表单形式的key-value类型
1. 类似Html文件上传表单的复杂请求体类型（多块请求）。

RequestBody有几个静态方法用于创建不同类型的请求体：


```
//创建String类型的请求体
public static RequestBody create(MediaType contentType, String content)

//创建文件类型的请求体
 public static RequestBody create(final MediaType contentType, final File file)
```

最终都是相当于重写了RequestBody的两个抽象方法来写入流，如果传递流类型的参数，只要重写这两个抽象方法即可。


```
//对应的是name为Content-Type的header
  public abstract MediaType contentType();

  //这个BufferedSink位于Okio包下，提供高效的写入。
  public abstract void writeTo(BufferedSink sink) throws IOException;

  //在写入的时候可以传递内容的大小，如果不知道就返回-1即可。
  public long contentLength() throws IOException {  return -1;}
```
例如，我们提交一个String：


```
String postBody = ""
        + "Releases\n"
        + "--------\n"
        + "\n"
        + " * _1.0_ May 6, 2013\n"
        + " * _1.1_ June 15, 2013\n"
        + " * _1.2_ August 11, 2013\n";

Request request = new Request.Builder()
        .url("https://api.github.com/markdown/raw")
        .post(RequestBody.create(MEDIA_TYPE_MARKDOWN, postBody))
        .build();
```

提交File：


```
File file = new File("README.md");

Request request = new Request.Builder()
        .url("https://api.github.com/markdown/raw")
        .post(RequestBody.create(MEDIA_TYPE_MARKDOWN, file))
        .build();
```
提交流：


```
RequestBody requestBody = new RequestBody() {
       @Override
        public MediaType contentType() {
             return MEDIA_TYPE_MARKDOWN;
        }

        @Override
         public void writeTo(BufferedSink sink) throws IOException {
              sink.writeUtf8("Numbers\n");
              sink.writeUtf8("-------\n");
              for (int i = 2; i <= 997; i++) {
                     sink.writeUtf8(String.format(" * %s = %s\n", i, factor(i)));
               }
           }

          private String factor(int n) {
                for (int i = 2; i < n; i++) {
                     int x = n / i;
                     if (x * i == n) return factor(x) + " × " + i;
                 }
                return Integer.toString(n);
                }
};

Request request = new Request.Builder()
        .url("https://api.github.com/markdown/raw")
        .post(requestBody)
        .build();
```

对于提交表单和分块请求，OkHttp提供了两个RequestBody的子类，**FormBody**和**MultipartBody**

#### 2.1.1 表单FormBody

FormBody也是采用建造者模式， 这个很简单，添加key-value形式的键值对即可。
添加键值对有两个方法：


```
//采用OkHttp默认的编码
public Builder add(String name, String value) 

//采用用户要求的编码
public Builder addEncoded(String name, String value)
```
例如：


```
  RequestBody formBody = new FormBody.Builder()
                        .add("search", "Jurassic Park")
                        .build();
  Request request = new Request.Builder()
                        .url("https://en.wikipedia.org/w/index.php")
                        .post(formBody)
                        .build();
```

#### 2.1.2 分块MultipartBody

MultipartBody也是采用建造者模式，MultipartBody.Builder可以构建**兼容Html文件上传表单**的复杂请求体。每一部分的多块请求体都是它自身的请求体，并且可以定义它自己的请求头。如果存在的话，这些请求头用来描述这部分的请求体。例如**Content-Disposition**、**Content-Length** 和 **Content-Type**如果可用就会被自动添加到头。

MIME类型有：

```
public static final MediaType MIXED = MediaType.parse("multipart/mixed");

public static final MediaType ALTERNATIVE = MediaType.parse("multipart/alternative");

public static final MediaType DIGEST = MediaType.parse("multipart/digest");

public static final MediaType PARALLEL = MediaType.parse("multipart/parallel");
```
有几个主要的方法：


```
//设置MIME类型，如MIXED（默认的）
    public Builder setType(MediaType type) {}

  //添加请求体
    public Builder addPart(RequestBody body) {
      return addPart(Part.create(body));
    }

  //添加包含header的请求体
    public Builder addPart(Headers headers, RequestBody body) {
      return addPart(Part.create(headers, body));
    }

    //请求体添加表单
    public Builder addFormDataPart(String name, String value) {
      return addPart(Part.createFormData(name, value));
    }

    //请求体中包含文件
    public Builder addFormDataPart(String name, String filename, RequestBody body) {
      return addPart(Part.createFormData(name, filename, body));
    }

    //添加自己定义的part
    public Builder addPart(Part part) {
      if (part == null) throw new NullPointerException("part == null");
      parts.add(part);
      return this;
    }
```

例如提交一个图片文件：


```
RequestBody requestBody = new MultipartBody.Builder()
         .setType(MultipartBody.FORM)
        .addFormDataPart("title", "Square Logo")
        .addFormDataPart("image", "logo-square.png",
         RequestBody.create(MEDIA_TYPE_PNG, new File("website/static/logo-square.png")))
        .build();

 Request request = new Request.Builder()
         .header("Authorization", "Client-ID " + IMGUR_CLIENT_ID)
         .url("https://api.imgur.com/3/image")
         .post(requestBody)
         .build();
```

### 2.2 客户端OkHttpClient

OkHttpClient采用建造者模式，通过Builder可以配置连接超时时间、读写时间，是否缓存、是否重连，还可以设置各种拦截器interceptor等。
建议在一个App中，OkHttpClient保持一个实例。一个OkHttpClient支持一定数量的并发，请求同一个主机最大并发是5，所有的并发最大是64。这个与OkHttp中的调度器Dispatcher有关，可以设置并发数。本文不对Dispatcher进行讨论。


```
OkHttpClient okHttpClient=new OkHttpClient.Builder().build();

//如果不需要我们额外配置，可以使用默认的配置
OkHttpClient okHttpClient1 = new OkHttpClient();
```
一个例子：

```
int cacheSize = 10 * 1024 * 1024; // 10 MiB
File cacheDirectory = new File(getCacheDir(), "OkHttpCache");
Cache cache = new Cache(cacheDirectory, cacheSize);

OkHttpClient client = new OkHttpClient.Builder()
        .connectTimeout(60, TimeUnit.SECONDS)//连接超时时间
        .readTimeout(60, TimeUnit.SECONDS)//读的时间
        .writeTimeout(60, TimeUnit.SECONDS)//写的时间
        .cache(cache)//配置缓存
        .build();
```
OkHttpClient支持单独配置，例如原来设置不同的请求时间，可以通过OkHttpClient的newBuilder()方法来重新构造一个OkHttpClient。例如：

```
OkHttpClient client = new OkHttpClient();

//读的时间设置为500ms
OkHttpClient copy = client.newBuilder()
                          .readTimeout(500, TimeUnit.MILLISECONDS)
                          .build();

//读的时间设置为3000ms
OkHttpClient copy = client.newBuilder()
                          .readTimeout(3000, TimeUnit.MILLISECONDS)
                          .build();
```
## 3 同步请求和异步请求

上面已经讲了如何创建Request和OkHttpClient，剩下的就是发送请求并得到服务器的响应了。OkHttp发送请求可分为同步和异步。OkHttpClient首先通过Request构建一个Call，通过这个Call去执行同步或者异步请求。


```
#OkHttpClient
public Call newCall(Request request)
```
同步方式，调用Call的execute()方法，返回Response，会阻塞当前线程：

```
response = client.newCall(request).execute();
```
异步方式，调用Call的enqueue(CallBack callBack)方法，会在另一个线程中返回结果。

```
client.newCall(request).enqueue(new Callback() {
            @Override
            public void onFailure(Call call, IOException e) {
              //处理错误的回调
            }

            @Override
            public void onResponse(Call call, Response response) throws IOException {
              //处理正确的回调
            }
        });
```
## 4 其他

### 4.1 配置响应缓存

为了缓存响应，需要一个可读写并且设置大小Size的缓存目录。缓存目录需要私有，其它不信任的应用不能访问这个文件。
如果同时有多个缓存访问同一个缓存目录会报错。所以最好只在App中初始化一次OkHttpClient，给这个实例配置缓存，在整个App生命周期内都用这一个缓存。否则几个缓存会相互影响，导致缓存出错，引起程序崩溃。
响应缓存采用Http头来配置，你可以添加这样的请求头**Cache-Control: max-stale=3600**。 **max-age**指的是客户端可以接收生存期不大于指定时间（以秒为单位）的响应。
为了防止响应使用缓存，可以用**CacheControl.FORCE_NETWORK**。为了防止使用网络，采用** CacheControl.FORCE_CACHE**。

> 注意:如果使用FORCE_CACHE强制缓存，而响应又需要网络，OkHttp会报504 Unsatisfiable Request 响应错误。

### 4.2 取消请求

调用Call.cancel()方法可以立即取消一个网络请求。如果当前线程正在写request或者读response会报IO异常。如果不再需要网络请求，采用这种方法是比较方便的。例如在App中返回了上一页。无论是同步还是异步的请求都可以被取消。

### 4.3 Response读取响应结果

可以通过Response的code来判断请求是否成功，如果服务器返回的有数据，可以通过Response的body得到一个ResponseBody读取。
如果采用ResponseBody的string()方法会一次性把数据读取到内存中，如果数据超过1MB可能会报内存溢出，所以对于超过1MB的数据，建议采用流的方式去读取，如ResponseBody的byteStream()方法。

**需要说明的是：**

> 1. 如果ResponseBody的内容不读取的话，不会触发IO流的读取操作
> 1. 内容读取之后，这个body需要关闭。

## 5 总结

OkHttp中的很多类都用到了建造者模式，可以根据需要灵活配置。采用建造者模式的有：

> 1. OkHttpClient.Builder
> 1. Request.Builder
> 1. FormBody.Builder
> 1. MultipartBody.Builder
> 1. Response.Builder


如果单独使用OkHttp进行网络请求，通常需要开发者自己再封装一下，如果不想重复造轮子，Github上面的有一些优秀开源库可以拿来使用（本文只列出star较多的几个）：

> 1. [hongyangAndroid/okhttputils](https://github.com/hongyangAndroid/okhttputils)
> 1. [jeasonlzy/okhttp-OkGo](https://github.com/jeasonlzy/okhttp-OkGo)
> 1. [yanzhenjie/NoHttp](https://github.com/yanzhenjie/NoHttp)

## 参考
[OkHttp官方Wiki文档](https://github.com/square/okhttp/wiki)