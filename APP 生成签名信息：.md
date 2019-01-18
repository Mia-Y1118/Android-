APP 生成签名信息：

```javascript
1、新建一个特定包名的app
2、生成一个带签名的线上包，需要新生成.jks文件。注意设置密码，一定要记得，jks不能丢掉。
3、在项目中的project structure中添加签名信息
```

查看app签名：

```javascript
----------------------------------------运用解压查看CERT.RSA的方式----------------------------------
1、将apk运用winRAR的解压，拿出CERT.RSA
2、cd 到jdk中的bin文件中，运用 keytool -printcert -file C:\Users\YangXiaoyu\Desktop\CERT.RSA，可以查看app签名文件。

----------------------------------------运用keytool查看jks的方式------------------------------------
1、cd 到jdk中的bin文件中 ,执行keytool -list -v -keystore oooo.jks
```



