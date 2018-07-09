##一、题库PK中所遇到的Fresco缓存问题概述：
- 加载题库PK的擂主，后台返回的图片的地址不变，由于Fresco存在缓存，当擂主的头像改变了，app再次加载擂主头像的时候，并未加载最新的擂主头像，而是app首次加载该链接返回的头像。（清除缓存数据，卸载也没有用，除非用代码清楚磁盘的缓存）

- 浏览器:对同一链接的加载也存在缓存问题。

##二、Fresco的缓存策略

### 1、Fresco的三级缓存（两级内存缓存 + 一级磁盘缓存）很少发生oom

	1、Bitmap缓存：

		①存储Bitmap对象，显示的时候更流畅，在5.0以下的系统，Fresco使用匿名共享内存区域存储Bitmap缓存，这样Bitmap对象的创建，释放将不会触发GC，不会占用Javaheap.
		②在5.0以上系统，内存管理有了很大程度的改进，Bitmap的缓存直接位于Java的heap上
		③当应用运行在后台中时，该内存是会被清空的。
	
	2、未解码图片的内存缓存：缓存的是原始压缩格式的图片。从该缓存取到的图片在使用之前需要进行解码，当app运行在后台的时候，这个缓存同样会被清理。

	3、文件缓存：和未解码的内存缓存相似，文件缓存存储的是未解码的原始压缩格式的图片，在使用之前同样需要经过解码等处理。但是APP在后台时，内容是不会被清空的。即使关机也不会。用户可以随时用系统的设置菜单中进行清空缓存操作。（磁盘缓存）

### 2、Fresco的加载流程：
- Fresco的加载实际上是由Image Pipeline负责完成，可以从本地文件，网络加载PNG，GIF，WebP,JPEG等格式图片，其大致加载流程如下：

![](https://i.imgur.com/LDjDMr4.jpg)

		①检查内存缓存，如果有内存缓存，则返回加载
		②检查是否在未解码内存缓存中，如果有，解码，变换，返回，然后缓存到内存缓存中
		③检查是否在文件缓存中，如果有，变换，返回。缓存到未解码缓存和内存缓存中
		④从网络或者本地加载，加载完成后解码，变换，返回。存到各个缓存中

##三、解决方案
- 后端返回不一样的加载图片的地址：
- 前端手动清理磁盘缓存，再重新加载：

		①清理一条url缓存：
		ImagePipeline imagePipeline = Fresco.getImagePipeline();
		Uri uri;
		imagePipeline.evictFromMemoryCache(uri);
		imagePipeline.evictFromDiskCache(uri);

		②清除缓存
		ImagePipeline imagePipeline = Fresco.getImagePipeline();
		imagePipeline.clearMemoryCaches();
		imagePipeline.clearDiskCaches();

