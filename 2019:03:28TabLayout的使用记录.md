#### 一、TabLayout的属性

```java
//需要在build.gradle中导入包后才能使用
app:tabBackground="@color/transparent"  //设置Tabs的背景
app:tabIndicatorHeight="0dp"//下面指示器的高度，Material Design 规范建议是2dp
app:tabMode="fixed" //设置Tabs的显示模式，fixed用于较少时，scrollable可以滚动
app:tabRippleColor="@color/transparent"// 去除点击的阴影效果
app:tabMaxWidth = "80dp" //设置Tab的最大宽度
app:tabMinWidth = "30dp" //设置Tab的最小宽度
app:tabGravity="center"  //为tabs设置Gravity,center表示居中显示，fill表示充满Tablayout 
app :tabPadding = "10dp" //设置Tabs的padding
app:tabSelectedTextColor 设置Tab选中后，文字显示的颜色
app:tabTextColor 设置Tab未选中，文字显示的颜色


//示例，不需要与ViewPager关联时
<android.support.design.widget.TabLayout
        android:id="@+id/tab_layout2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:tabMode="fixed"
        app:tabGravity="center"
        app:tabIndicatorColor="@android:color/holo_red_light"
        app:tabIndicatorHeight="2dp"
        app:tabSelectedTextColor="@android:color/holo_red_light">
         
            <android.support.design.widget.TabItem
        	android:layout_width="wrap_content"
        	android:layout_height="wrap_content"
        	android:text="个性推荐"
        	/>
                
       	   <android.support.design.widget.TabItem
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="歌单"
            />
                
        	<android.support.design.widget.TabItem
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="主播电台"
            />

    </android.support.design.widget.TabLayout>
```

#### 二、TabLayout + ViewPager + Fragment

```java
//布局文件
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context=".ui.studyReport.NewStudyReportActivity">

    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />

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
</RelativeLayout>

//先需要初始化ViewPager后，才能与TabLayout关联
val fragments = ArrayList<StudyReportFragment>()
fragments.add(unMasteryFragment)
fragments.add(weakFragment)
fragments.add(masteryFragment)
                    
 private fun initViewPager(fragments: ArrayList<StudyReportFragment>) {
        vp_study_pager.offscreenPageLimit = 3
        adapter = NewStudyPagerAdapter(supportFragmentManager, fragments)
        vp_study_pager.adapter = adapter
}

//TabLayout与ViewPager建立关联
	private fun initTabLayout() {
        tab_layout.setupWithViewPager(vp_study_pager) //建立关联，很重要
        for (i in 0..2) {
            val tab = tab_layout.getTabAt(i) ?: return
            val view = LayoutInflater.from(this).inflate(R.layout.item_study_report, null)
            tab.customView = view
            val tabView = tab.customView ?: return
            val tv = tabView.findViewById<TextView>(R.id.tv_title_tab)
        }

    	//tab_layout添加选中监听
        tab_layout.addOnTabSelectedListener(object : TabLayout.OnTabSelectedListener {
            override fun onTabSelected(tab: TabLayout.Tab) {
               //选中监听
            }

            override fun onTabUnselected(tab: TabLayout.Tab) {
               //未选中监听
            }

            override fun onTabReselected(tab: TabLayout.Tab) {

            }
        })
    }
```

#### 三、注意事项

- 在tabLayout与ViewPager建立关联的之后，ViewPager中的fragment改变或者fragments的数量与tabLayout的数量不对应，可能会造成tabLayout不显示。

- 如果涉及接口，一般需要在接口请求回来再去初始化ViewPager、Fragment和tabLayout.

  