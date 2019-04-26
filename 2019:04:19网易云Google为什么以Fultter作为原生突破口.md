#### 一、为什么Flutter能成为Android方向标

- 跨平台性:在不同平台下绘制效果是一致的
- 性能优异性：H5通过DOM渲染，RN映射组件，Flutter直接基于native进行绘制
- 热重载性：新的dart文件与旧的dart文件，形成差分文件，adb推送，形成Flutter.jar,通过skia加载，形成热重载

#### 二、Flutter系统架构

- 由Dart语言负责Framwork层

- Dart语法执行器

- Skia图像处理引擎：二维图像处理引擎，包括绘图，渲染，显示图片，依赖于OPenGL

  ```java
  
  ```

  

#### 三、Bitmap的加载过程是在Native堆，真正加载Bitmap的不是Bitmap.java,而是SKBitmap。

```java
createBitmap --> nativeCreateBitmap -->映射到Native层面，最后通过SKBitmap函数库进行加载
```

四、Flutter 的Widght

- StatelessWidget :没有灵魂,仅是展示作用
- StatefulWidget : 有灵魂，控件需要用数据来刷新则用StatefulWidget