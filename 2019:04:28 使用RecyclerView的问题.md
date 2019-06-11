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

  

### 三、RecyclerView嵌套时点击事件

- 现象：

  ```java
  当两个RecyclerView嵌套使用时，如果需要设置外层RecyclerView的item的点击事件，由于内层recyclerView在外层item之上，所以点击内层的recyclerView无法响应点击事件。只有点击内层recyclerView外，外层recyclerView的item内区域才能响应点击事件
  ```

- 解决办法：将内层recyclerView的点击事件转移到外层

  ```java
   //设置内层RecyclerView的touch到外层
  itemView.rv_list.setOnTouchListener(object : View.OnTouchListener {
                  override fun onTouch(v: View?, event: MotionEvent?): Boolean {
                      return itemView.onTouchEvent(event)
                  }
              })
  ```

  

