* 为何我一直找不到底层http源码。妈的，智障。
* 首先声明一个线程安全的单例okhttp。
* 通过builder模式构建一个request，request中包含HttpUrl，method，Headers，RequestBody，tag,CacheControl。通过这6个对象组合成request。HttpUrl，Header，也是利用builder模式实现。CacheControl利用静态构造函数和接口构造CacheControl。
 ```Java
OkHttpClient client = new OkHttpClient();
Request request = new Request.Builder()
        .url(ENDPOINT)
        .build();
Response response = client.newCall(request).execute();
 ```
* 重点在realCall
*
