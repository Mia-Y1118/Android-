#### 一、CoordinatorLayout

- 是material design中的组件，用来协调其子View并以触摸影响布局的形式产生动画效果的一个FragmentLayout.

#### 二、AppBarLayout

- 是一个实现了很多材料设计特性的垂直的LinearLayout.必须在子View(如CollapsingToolbarLayout)上设置app:layout_scrollFlags属性或者代码中setScrollFlags()设置这个属性。

  ```java
  app:layout_scrollFlags属性值举例：
  
  scroll:想要滚出屏幕的View都需要设置，如果没有设置这个flag为scroll,将被固定在屏幕顶部。
  enterAlways:让任意向下的滑动都会导致该View变为可见，即快速返回模式。
  enterAlwaysCollapsed:在设置了最小高度和enterAlways情况下，appLayout将在最小高度时开始显示,并且从这个时候开始慢慢展示，当滚动到顶部的时候展开完成。
  exitUntilCollapsed：在设置了最小高度的情况下，当布局滚动到这个最小高度时折叠。
  snap:当滚动事件结束，如果视图部分可见，那么将被滑动到收缩或者展开。fg:如果试图只有底部25%,将折叠。如果底部可见75%，它将完全展开。
  ```

- 使用时需要让APPBarLayout的父View是CoordinatorLayout.否则很多属性不能使用。

- AppBarLayout一般与一个具有滑动属性的同级View配合使用，并且这个同级View需要设置behavior属性为@string/appbar_scrolling_view_behavior，否则无法实现与AppBarLayout的联动。

#### 三、CollapsingToolbarLayout

- 提供一个可以折叠的Toolbar,继承自FragmeLayout。设置layout_scrollFlags后，能控制其子View的滑动事件。

- 子布局的三种折叠模式：

  ```java
  app:layout_collapseMode属性值举例：
  
  off:布局将正常显示，无折叠模式
  pin:CollapsingToolbarLaylout折叠后，此布局将固定在顶部
  parallax:CollapsingToolbarLayout折叠时，此布局也会有视差折叠效果。当CollapsingToolbarLayout的子布局设置了parallax模式时，我们还可以通过app:layout_collapseParallaxMultiplier设置视差滚动因子，值为：0~1
  ```

#### 四、注意事项：

- 当TabLayout放在AppBarLaylout中时，会显示一个默认的阴影。需要用app:elevation = "0dp"。而不是android:elevation = "0dp"
- 使用CooedinatorLayout的时候注意设置其高度为match_parent,如果设置为wrap_content,会出现toolbar有多高，下面就会留出多高的距离。

#### 五、项目中使用的CoordinatorLayout

```java
//布局文件，由于bar存在阴影，所以暂时把ViewPager写到上面了
<androidx.coordinatorlayout.widget.CoordinatorLayout
        android:id="@+id/coordinator"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_below="@+id/toolbar">

        <androidx.viewpager.widget.ViewPager
            android:id="@+id/vp_study_pager"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_gravity="fill_vertical"
            android:layout_marginTop="-12dp"
            android:background="@color/white"
            app:layout_behavior="@string/appbar_scrolling_view_behavior" />

        <com.google.android.material.appbar.AppBarLayout
            android:id="@+id/app_bar"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@color/transparent"
            android:orientation="vertical"
            app:elevation="0dp">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:background="@color/white"
                android:orientation="vertical"
                android:paddingStart="@dimen/dimen15"
                android:paddingEnd="@dimen/dimen15"
                app:layout_scrollFlags="scroll|exitUntilCollapsed">

                <TextView
                    android:id="@+id/tv_subject_name"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="@dimen/dimen15"
                    android:textColor="@color/color_value_515151"
                    android:textSize="@dimen/dimen_size_19"
                    android:textStyle="bold"
                    tools:text="思想道德修养与法律基础" />

                <RelativeLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="@dimen/dimen15"
                    android:background="@drawable/study_report_bg"
                    android:paddingStart="@dimen/dimen20"
                    android:paddingTop="@dimen/dimen15"
                    android:paddingEnd="@dimen/dimen20"
                    android:paddingBottom="@dimen/dimen15">

                    <TextView
                        android:id="@+id/tv_prediect"
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="@string/knowledge_node_score"
                        android:textColor="@color/white"
                        android:textSize="@dimen/dimen_size_17" />

                    <TextView
                        android:id="@+id/tv_report_score"
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_below="@id/tv_prediect"
                        android:layout_marginTop="@dimen/dimen2"
                        android:textColor="@color/white"
                        android:textSize="50sp"
                        tools:text="80" />

                    <TextView
                        android:id="@+id/tv_tip"
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_below="@id/tv_report_score"
                        android:textColor="@color/white"
                        android:textSize="@dimen/dimen_size_13"
                        tools:text="您已经超越了52%的学员" />

                    <TextView
                        android:id="@+id/btn_go_quick_score"
                        android:layout_width="100dp"
                        android:layout_height="@dimen/dimen37"
                        android:layout_alignBottom="@+id/tv_tip"
                        android:layout_alignParentEnd="true"
                        android:background="@drawable/btn_improve_score"
                        android:gravity="center"
                        android:text="@string/fast_improve_prediect"
                        android:textColor="@color/color_value_ff7767" />
                </RelativeLayout>
            </LinearLayout>

            <com.google.android.material.tabs.TabLayout
                android:id="@+id/tab_layout"
                android:layout_width="match_parent"
                android:layout_height="66dp"
                android:background="@drawable/study_tab_list_bg"
                android:paddingBottom="@dimen/dimen12"
                app:tabBackground="@color/transparent"
                app:tabIndicatorHeight="0dp"
                app:tabMode="fixed"
                app:tabRippleColor="@color/transparent" />
        </com.google.android.material.appbar.AppBarLayout>

    </androidx.coordinatorlayout.widget.CoordinatorLayout>
    
    //对于appBar的滑动监听
      app_bar.addOnOffsetChangedListener(AppBarLayout.OnOffsetChangedListener { _, p1 ->
            if (p1 < -Utils.dip2px(this, 34f)) {
                setToolBarTitle(subjectName)
            } else {
                setToolBarTitle("学习报告")
            }
        })
```

