# 【12】HttpURLConnection、HttpClient，OkHttp，Volley
- HttpURLConnection是java.net包提供的基本类，没有什么封装，具有良好扩展性。但是其close方法会导致连接池失效，一般会禁用连接池。
- HttpClient相对于HttpURLConnection具有良好的封装，对外提供大量api，扩展性不太好。并且由于其维护过于垃圾，版本之间兼容性太差，目前基本已被抛弃。
- Okhttp具有比httpclient更好的扩展性和兼容性，且比httpurlconnection更容易使用，目前使用范围十分广泛。
    - 同时支持同步和异步请求；
    - 支持http/2协议，支持端口的复用。
    - 当http/2bu不可用时，可用连接池技术复用连接。
    - 使用GZIP，压缩传输大小。
    - 支持缓存处理。
    - 缺点：okhttp请求网络回调在线程里，不能直接更新UI。

- Volley是google推出的通信框架，比较适合处理数据量小，通信频繁的网络操作。优点是内部封装了异步线程，可直接在主线程请求网络，并处理返回结果，更新UI。但是对大数据量的请求比较糟糕（下载东西比较慢）。一般网络请求底层采用okhttp，异步回调使用Volley框架。