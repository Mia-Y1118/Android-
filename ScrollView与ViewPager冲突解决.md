#### 一、问题

- 当ScrollView中嵌套ViewPager的时候，ViewPager如果设置match_parent,或者wrap_content都展示不出来。

- 滑动会有冲突

  

#### 二、解决方法

- 选择fragment中高度最大的那个作为整个viewPager的高度。

```java
class ViewPagerForScrollView @JvmOverloads constructor(context: Context, attrs: AttributeSet) : ViewPager(context,attrs) {
    
    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        var heightMeasureSpec1 = heightMeasureSpec
        var height = 0
        for (i in 0 until childCount) {
            val child = getChildAt(i)
            child.measure(widthMeasureSpec, MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED))
            val h = child.measuredHeight
            if (h > height)
                height = h
        }
        heightMeasureSpec1 = MeasureSpec.makeMeasureSpec(height, MeasureSpec.EXACTLY)
        super.onMeasure(widthMeasureSpec, heightMeasureSpec)
    }
}
```

- 但是会出现一个问题，如果fragment中的view的高度很小的话，其他的fragment的高度很大，会导致某些fragment中出现大面积空白。

  