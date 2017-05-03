前言

对于还不知道Refrofit是什么的同学，我只能说你们太out了，赶紧去Google下，本文是针对Retrofit2.0的使用介绍，当然大家也可以直接去官网上去看使用介绍

添加依赖

要使用Retrofit首先得添加gradle依赖

> compile 'com.squareup.retrofit2:retrofit:2.0.2'

> compile 'com.squareup.retrofit2:converter-gson:2.0.0'

声明服务接口

依赖添加完成之后，就可以开始撸代码了，首先我们要新建一个Interface，这个接口就是你要请求的接口服务类，就像这个样子


```
public interface RetrofitService {
    @GET("getModelService")
    Call<User> getModelService();
    ...
}
```
上面这个接口中暂时只声明了一个方法，而在Retrofit中想这样声明的一个方法就代表了一个具体的Http请求接口。Retrofit中方法上添加@GET注解就表示当前方法代表一个Get请求，我们声明的这个请求方法不带任何参数，且返回结果是User对象。
@GET注解括号中的值getModelService就代表http请求地址的具体指向，这个值与下面baseUrl中配置的URL一起组成一个完整的请求地址
Retrofit就是这么任性，它可以帮我们把http的请求结果自动转换成Javabean对象，这将为我们减少不少编码工作。

构造Retrofit对象

接口服务声明完成之后，我们接着需要新建一个Retrofit对象


```
Retrofit retrofit = new Retrofit.Builder()
         .baseUrl("http://localhost:8080/AndroidService/")
         .addConverterFactory(GsonConverterFactory.create())
         .build();
RetrofitService service = retrofit.create(RetrofitService.class);

```

通过上面的代码我们就创建了一个Retrofit对象，同时通过这个Retrofit对象将我们前面声明的服务接口进行了实例化，有了这个服务接口实例化对象，我们就可以调用接口里面声明的方法进行http请求了。

另外需要说明是我们在设置baseUrl的时候一般在都是以/结尾，而不是在接口方法中的url以/开头
前面说的Retrofit会将请求结果自动转换成Javabean对象，其实这个是需要配置结果转换器的，就像上面我配置的addConverterFactory(GsonConverterFactory.create())，配置了这个转换器工厂之后，就会将请求得到的JSON数据自动转换成Javabean对象。同样如果请求得到的数据是xml类型的话，转换成Javabean对象则需要配置成其他类型的转换器
Retrofit其实已经为我们提供了多种转换器供我们选择：

1. Gson: com.squareup.retrofit2:converter-gson
1. Jackson: com.squareup.retrofit2:converter-jackson
1. Moshi: com.squareup.retrofit2:converter-moshi
1. Protobuf: com.squareup.retrofit2:converter-protobuf
1. Wire: com.squareup.retrofit2:converter-wire
1. Simple XML: com.squareup.retrofit2:converter-simplexml
1. Scalars (primitives, boxed, and String): com.squareup.retrofit2:converter-scalars

**同步请求**

前面准备工作都完成了，ok，可以开始发起请求了


```
Call<User> call = service.getModelServiceSync();
Response<User> response = call.execute();
if(response.isSuccessful()) {
    User user = response.body();
}
```
这就是Retrofit的基本使用流程：添加依赖–>声明接口–>创建Retrofit和接口对象–>选择执行同步异步请求

**异步请求**

同步请求执行call.execute()即可，异步请求则是这样：

```
call.enqueue(new Callback<User>() {
    @Override
    public void onResponse(Call<User> call, Response<User> response) {
        if(response.isSuccessful()) {
            User user = response.body();
        }
    }
    @Override
    public void onFailure(Call<User> call, Throwable t) {
    }
});
```
**带参Get请求**

如果要实现带参数的Get请求，则需要将我们上面的方法声明改成如下形式


```
@GET("getModelService")
Call<User> getModelService(@Query("param1") boolean param1, @Query("param2") boolean param2);
```
Retrofit中我们只需要将要传输到服务端的参数添加@Query注解即可，而@Query注解括号中的值param1和param2则是对应的参数名

Retrofit也为我们提供了另外一种形式，使用@QueryMap注解来声明一个集合作为请求参数


```
@GET("getModelService")
Call<User> getModelService(@QueryMap Map<String, String> parmas);

```

**动态URL**

如果我们有两个这样的请求地址


```
http://localhost:8080/AndroidService/user/zhangsan
http://localhost:8080/AndroidService/user/lisi
```
这两个请求地址很明显，除了最后的一段不一样其他的都一样。因此如果按照上面的做法的话，我们要请求这两个服务就需要声明两个接口方法，如果这两个请求地址没有其他参数的话声明两个接口方法不是感觉很累赘，所以Retrofit为我们提供了动态构造URL的方式来声明类似请求地址的接口方法。


```
@GET("user/{path}")
Call<User> getModelService(@Path(path) String path, @Query("param1") boolean param1, @Query("param2") boolean param2);
```
上面的方法中，@GET注解中{path}就代表URL可以动态变化的那一部分，@Path注解则表示该方法参数的值就是{path}中的动态值

**Post请求**

```
@FormUrlEncoded
@POST("postService")
Call<String> postModelService(@Field("msg") String msg, @Field("string") boolean isString);
```
Retrofit中的Post请求则需要将前面的@GET注解换成@POST注解，而Post请求的文本参数则用注解@Field来声明，同时还必须给方法添加注解@FormUrlEncoded来告知Retrofit参数为表单参数，如果只为参数增加@Field注解，而不给方法添加@FormUrlEncoded注解运行时会抛异常。

如果Post请求有很多参数同样也可以使用集合的方式进行声明

```
@FormUrlEncoded
@POST("{path}")
Call<User> postModelService(@Path("path") String path, @FieldMap Map<String, String> map);
```
Retrofit中提供了注解@FieldMap用来声明一个Map类型的集合作为Post请求的表单参数

Post请求的服务接口声明完成之后，讲得到的Call对象调用execute或者enqueue即可发起同步或异步的Post请求