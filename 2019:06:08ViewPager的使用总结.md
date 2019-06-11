#### 一、ViewPager的滑动监听

1、OnPageChangeListener这个接口需要实现三个方法：onPageScrollStateChanged，onPageScrolled ，onPageSelected，ViewPager未滑动之前是不会调用的

```java
①、onPageScrollStateChanged(int arg0) 此方法是在状态改变的时候调用。
    - 其中arg0这个参数有三种状态（0，1，2）
    - arg0 ==1的时表示正在滑动，arg0==2的时表示滑动完毕了，arg0==0的时表示什么都没做
    - 当页面开始滑动的时候，三种状态的变化顺序为1-->2-->0
  
②、onPageScrolled(int arg0,float arg1,int arg2) 当页面在滑动的时候会调用此方法，在滑动被停止之前，此方法回一直被调用。
    - 其中三个参数的含义分别为：
    - arg0 :当前页面，及你点击滑动的页面
    - arg1:当前页面偏移的百分比
    - arg2:当前页面偏移的像素位置 
    
③、onPageSelected(int arg0) 此方法是页面跳转完后被调用，arg0是你当前选中的页面的Position（位置编号）

viewpager.addOnPageChangeListener(new OnPageChangeListener() {
			
			@Override
			public void onPageSelected(int arg0) {
				// arg0是当前选中的页面的Position
				Log.e(TAG, "onPageSelected------>"+arg0);
			}
			
			@Override
			public void onPageScrolled(int arg0, float arg1, int arg2) {
				// arg0 :当前页面，及你点击滑动的页面；arg1:当前页面偏移的百分比；arg2:当前页面偏移的像素位置
				Log.e(TAG, "onPageScrolled------>arg0："+arg0+"\nonPageScrolled------>arg1:"+arg1+"\nonPageScrolled------>arg2:"+arg2);
			}
			
			@Override
			public void onPageScrollStateChanged(int arg0) {
				//arg0 ==1的时表示正在滑动，arg0==2的时表示滑动完毕了，arg0==0的时表示什么都没做。
				if(arg0 == 0){
					Log.e(TAG, "onPageScrollStateChanged------>0");
				}else if(arg0 == 1){
					Log.e(TAG, "onPageScrollStateChanged------>1");
				}else if(arg0 == 2){
					Log.e(TAG, "onPageScrollStateChanged------>2");
				}
				
			}
		});
```



```java
vp_exam_work.addOnPageChangeListener(object : ViewPager.SimpleOnPageChangeListener() {
            override fun onPageSelected(position: Int) {
                //已经在当前页面
                if (currentEntity?.sequence == position + 1) return
            }
        })
```



#### 二、ViewPager刷新无效

- 现象：在做题库多选多时，发现到第一道题目去提交时，提交后，使用ViewPager的notifyDataSetChanged刷新界面无效。

- 原因：如果item的位置如果没有发生变化，则返回**POSITION_UNCHANGED**。如果返回了**POSITION_NONE**，表示该位置的item已经不存在了。默认的实现是假设item的位置永远不会发生变化，所以返回的是**POSITION_UNCHANGED**。

- 解决方法：尝试修改适配器的getItemPosition方法，当调用notifyDataSetChanged时，让getItemPosition方法人为的返回POSITION_NONE，从而达到强制刷新viewPager重绘所有Item的目的。

  ```java
  override fun getItemPosition(`object`: Any): Int {
          return POSITION_NONE
      }
  
  //存在问题：
  调用notifyDataSetChanged方法会通过adapter的getItemPosition方法查询一遍所有的child view。当child View的位置均为POSITION_NONE时，所有的child View都不逊在，viewPager会调用destroyItem方法销毁，并重新生成，加大系统开销。
  ```

  

  

