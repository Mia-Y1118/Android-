## 布局优化 ##
- 尽量减少布局文件中的层级，布局的层级减少了，就意味着Android绘制时的工作量少了，程序的性能自然就提高了。

- 通过<include>主要用于布局重用,可以指定android:layout_等属性，支持id,width,height，但是不能设置背景。

- <merge>：一般和include配合使用，当当前的布局是一个LinearLayout的时候，如果被包含的布局也采用了竖直方向的LinearLayout,那么显然被包含的布局文件的Linearlayout是多余的，通过Merge标签，可以去掉多余的那一层LinearLayout.降低减少布局的层级

- ViewStub的方式:按照需要加载布局文件，其继承自View，所以其高宽均为0，因此其本身不参与任何的布局和绘制过程。

  ```java
  //使用时可以通过设置Visibility来按需加载
  ```

  

- onDraw中尽量不要做一些耗时的操作，也不能执行成千上万的循环的操作;并且onDraw中不要创建新的布局对象，因为onDraw可能会被频繁调用。由Google提出的标准，View的绘制帧率保持在60fps是最佳的，这就要求没帧的绘制时间不超过16ms(16ms=1000/60),虽然程序很难保证16ms模式，但是尽量降低onDraw方法的复杂度总是切实有效的。

## 内存泄漏 ##
- 静态变量容易导致内存泄漏
- 单例模式容易导致内存泄漏：注册之后需要解注册（有可能需要传参，被静态的Instance持有，可以使用application的Context）
- 属性动画中的无线循环动画，容易导致内存泄漏：在OnDestroy中去停止动画，animator.cancel();

## 响应速度优化和ANR日志分析 ##
- Activity如果在5秒之内无法响应屏幕触摸事件或者键盘输入事件就会出现ANR。
- BroadCastReceiver:如果10s之内还没执行完操作，就会出现ANR.

```java
//ANR分析方式：
- adb pull /data/anr/traces.txt  //可以倒出traces文件的部分内容
```

### List和Bitmap的优化

---



