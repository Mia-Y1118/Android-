#### 一、SurfaceView

- SurfaceView是在一个子线程对自己进行绘制，优点：避免造成UI线程阻塞.

- Surface包含一个专门用于绘制的Surface,Surface中包含一个Canvas

- 如何获取Canvas?

  ```java
  getHolder -> SurfaceHolder
  holder -> Canvas + 管理SurfaceView的生命周期
  surfaceCreated -> surfaceChanged -> surfaceDestoryed
  
  ```

  

