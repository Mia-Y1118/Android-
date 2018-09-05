### 一、在使用ScrollView与RecyclerView遇到的冲突问题（当ScrollView中嵌套RecyclerView的时候会出现滑动冲突）：

- 禁止RecyclerView的滑动：

  ```java
  LinearLayoutManager layoutManager = new LinearLayoutManager(context) {
      @Override
      public boolean canScrollVertically() {
          // 直接禁止垂直滑动
          return false;
      }
  }
  ```

  

- 通过View事件分发机制进行事件拦截，重写ScrollView,将滑动事件拦截掉 https://blog.csdn.net/coralline_xss/article/details/72887136：

- android:descendantFocusability="blocksDescendants":该属性是当一个View获取焦点的时候，定义ViewGroup和其子控件直接的关系，常用来解决父空间的焦点或者点击事件被子控件获取。

  - beforeDescendants: ViewGroup会优先其子控件获取焦点
  - afterDescendants: ViewGroup只有当其子控件不需要获取焦点时才获取焦点
  - blocksDescendants: ViewGroup会覆盖子类控件而直接获得焦点

  

### 二、RcyclerView嵌套RecyclerView时，会出现被嵌套的RecyclerView的第一个Item获取到焦点，当超过一屏幕的时候会向下滑动一定的距离。

- 解决办法：在RecyclerView的最直接的父布局中添加：

  ```java
        android:focusable="true"
        android:focusableInTouchMode="true"
  ```

- 在nps的卡顿主要是由于ScrollView中嵌套了RecyclerView,RecyclerView中又嵌套了一层RecyclerView,为解决掉滑动的卡顿，需要将最内层的RecyclerView的滑动禁止掉，采用以下方式：

  ```java
  itemView.rv_tags.layoutManager = object : GridLayoutManager(context,2){
                  //禁止二级标签的滑动
                  override fun canScrollVertically(): Boolean {
                      return false
                  }
              }
  ```

  

### 三、键盘弹出的问题

1、键盘的弹出位置从底部弹出，有可能会出现弹出的键盘挡住了EditText

- 解决办法： 在键盘弹出的时候，将View 向下滑动一定的距离。可以通过以下方法获取键盘弹出的高度

```java
 private fun getKeyBoardHeight(){
      SoftKeyBoardListener.setListener(act,object : SoftKeyBoardListener.OnSoftKeyBoardChangeListener{
          override fun keyBoardHide(height: Int) {

          }

          override fun keyBoardShow(height: Int) {
              Log.d("yxy", "键盘的高度为$height")
                  //必须放到两外一个线程去执行，否则可能会造成卡顿
              Handler().postDelayed({
                  sv_nps.isSmoothScrollingEnabled = true
                  sv_nps.smoothScrollBy(0,1920)
              },10)
          }
      })

  }
```

2、对于ScrollView的滑动(Android 的View试图是没有边界的，Canvas是没有边界的。布局坐标：父视图给子视图分配布局的大小，有边界)

- scrollTo（int x ,int y):将当前的视图内容偏移致（x,y)坐标出，即将View滑动到（x,y)的坐标处。
- scrollBy（int x,int y):将当前的View再滑动x,y这么长的偏移量。

