## Bitmap的高效加载 ##
- BitmapFactory提供了4类方法，decodeFile(文件系统）,decodeResource（资源）,decodeStream（输入流）,decodeByteArray（字节数组）
- BitmapFactory.Options通过inSampleSize(采样率）可以按照一定的采样率来加载缩小后的图片
- inSampleSize的大小必须是2的指数，当小于1的时候，按照1处理。
- 通过采样率可以有效的加载图片，那么到底该如何获取采样率呢？
		①将BitmapFactory.Options的inJustDecodeBounds参数设为true并加载图片
		②从BitmapFactory.Options中取出图片的原始宽高信息，他们对应于outWidth和outHeight.
		③根据采样率规则并结合目标的View的所需要的大小计算采样率inSampleSize
		④将BitmapFactory.Options的inJustDecodeBound参数设置为false,然后重新加载图片。

## Android中的缓存策略 ##
- LRUCache:最近最少使用缓存策略。内部采用LinkedHashMap以强引用的方式来存储外界存储对象，提供get,set方法来完成缓存获取和添加操作，当缓存满的时候，LRUCache会移除较早使用的缓存对象，然后再添加新的缓存对象。
- DiskLruCache:存储设备缓存（磁盘缓存）

## ImageLoader ##
- 首先尝试从内存中读取图片，接着尝试从磁盘缓存中读取图片，最后才从网络拉取图片。