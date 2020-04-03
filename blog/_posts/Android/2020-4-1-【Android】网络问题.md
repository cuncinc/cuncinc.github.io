### Http请求不能在主线程使用

### 界面不能在子线程中更新

### 安卓9不能使用http

今天用OKHttp访问自己云服务器上的资源，结果总是出错，还以为是我代码写错了。去查询了错误信息`java.net.UnknownServiceException: CLEARTEXT communication to *** not permitted by network security policy`，才发现，安卓9居然不能用http而只能用https请求。

根据别人的博客，有两种解决方法

1. 把 targetSdkVersion 降到27以下

2. 在`res/xml`中定义一个xml文件，写入以下内容：

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <network-security-config>
    <base-config cleartextTrafficPermitted="true" />
   </network-security-config>
   ```

   并在`AndroidManifest.xml`的`application`标签下增加

   ```xml
   android:networkSecurityConfig="@xml/上面那个文件的基本名"
   ```




> 参考：[Android P(9.0) 中OKHttp3网络请求出现java.net.UnknownServiceException: CLEARTEXT communication ** not permitted by network security policy 异常的原因及解决方法]([https://github.com/wenhelinlu/spark/wiki/Android-P(9.0)-%E4%B8%ADOKHttp3%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82%E5%87%BA%E7%8E%B0java.net.UnknownServiceException:-CLEARTEXT-communication-**-not-permitted-by-network-security-policy-%E5%BC%82%E5%B8%B8%E7%9A%84%E5%8E%9F%E5%9B%A0%E5%8F%8A%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95](https://github.com/wenhelinlu/spark/wiki/Android-P(9.0)-中OKHttp3网络请求出现java.net.UnknownServiceException:-CLEARTEXT-communication-**-not-permitted-by-network-security-policy-异常的原因及解决方法))