## 一、Http协议： ##
Android中主要提供了两种方式来进行Http的操作：HttpURLConnection和HttpClient。这两种方式都支持HTTPS协议、以流的形式进行上传和下载、配置超时时间、IPv6、以及连接池等功能。

## 1、HttpURLConnection: ##
是一种多用途，轻量级的HTTP客户端。HttpURLConnection是Java的标准类，继承自URLConnection,可以用于向指定网站发送GET请求，POST请求。它在URLConnection的基础上提供了如下便捷的方法：

(但是在Android2.2之前，当一个可读的InputStream调用了close()方法，就有可能导致连接池失效,通常的解决办法就是直接禁掉连接池的功能)

int getResponseCode()：获取服务器的响应代码
String getResponseMessage()：获取服务器的响应消息String getResponseMethod()：获取发送请求的方法
void setRequestMethod(String method)：设置发送请求的方法

2、在Android4.0的版本中，又添加了一些响应的缓存机制，当缓存被安装（调用HttpResponseCache的install()方法)后

## 3、HttpClient: ##

 可以更好的处理Session、Cookie等细节问题。是一个增强型的HttpURLConnection,在Android5之后已经被废弃了。

## 4、那么在Android开发中，访问网络我们是选择HttpURLConnection还是HttpClient好呢？ ## 

在Android 2.2版本之前，HttpClient拥有较少的bug，因此使用它是最好的选择。
而在Android 2.3版本及以后，HttpURLConnection则是最佳的选择。它的API简单，体积较小，因而非常适用于Android项目。压缩和缓存机制可以有效地减少网络访问的流量，在提升速度和省电方面也起到了较大的作用。对于新的应用程序应该更加偏向于使用HttpURLConnection，因为在以后的工作当中Android也会将更多的时间放在优化HttpURLConnection上面。所以目前来说，HttpURLConnection应该是更受亲睐的选择。

## 5、Https ##
- https是经由http通信，但利用TLS来加密数据包的。HTTPS开发的主要目的是提供网站服务器的身份认证，保护交换数据的隐私与完整性。
- TLS是传输层的加密协议，前身是SSL协议。
- 由于Http是采用明文进行传输的，可以通过网络抓包的形式获取到Http所传输的数据，因此采用Http协议的数据是不安全的，这才催生了HTTPS的诞生。HTTPS相对于HTTP提供了更安全的数据传输保障，主要体现在以下3个方面：

	①内容加密。客户端到服务器的内容都是以加密形式传输，中间者无法直接查看明文内容。 
	②身份认证。通过校验保证客户端访问的是自己的服务器。
	③数据完整性。防止内容被第三方冒充或者篡改。

- 实现原理：为了提高安全性和效率HTTPS结合了对称和非对称两种加密方式。即客户端使用对称加密生成密钥（key）对传输数据进行加密，然后使用非对称加密的公钥再对key进行加密。因此网络上传输的数据是被key加密的密文和用公钥加密后的密文key，因此即使被黑客截取，由于没有私钥，无法获取到明文key，便无法获取到明文数据。所以HTTPS的加密方式是安全的。
 
	对称加密就是加密和解密使用同一个密钥，虽然破解难度加大，但是对称加密需要在网络上传输密钥和密文。
	非对称加密就是加密和解密使用不同的密钥，但是非对称加密消耗的CPU资源非常大，效率很低，严重影响HTTPS的性能和速度。

- HTTPS的握手过程：

	- 客户端发送 client_hello，包含一个随机数 random1。 

	- 服务端回复 server_hello，包含一个随机数 random2，携带了证书公钥 P。 

	- 客户端接收到 random2 之后就能够生成 premaster_secrect （对称加密的密钥）以及 master_secrect（用premaster_secret加密后的数据）。 

	- 客户端使用证书公钥 P 将 premaster_secrect 加密后发送给服务器 (用公钥P对premaster_secret加密)。 

	- 服务端使用私钥解密得到 premaster_secrect。又由于服务端之前就收到了随机数 1，所以服务端根据相同的生成算法，在相同的输入参数下，求出了相同的 master secrect。

- 数字证书：由受信用的数字证书颁发机构，证书中包含一个密钥（公钥和私钥）和所者识别信息。
