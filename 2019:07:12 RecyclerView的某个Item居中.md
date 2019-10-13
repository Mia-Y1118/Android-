一、做刻度尺时用了RecyclerView,但是有时候需要某个Item居中

```java
fun setCurrentRow(current: Int) {
        val position = current / 10
        //init数据源,我的报告的数据源未0-100
        val dataList = IntArray(11) { i -> i }
 
        val rvManager = LinearLayoutManager(context, HORIZONTAL, false)
        val translationX = Utils.dip2px(context, 60f).toInt()
          
        rv_rule.layoutManager = rvManager
        rv_rule.adapter = RulerAdapter(dataList.asList(), position)
        rv_rule.smoothScrollToPosition(position)

        val linearSnapHelper = LinearSnapHelper()
        linearSnapHelper.attachToRecyclerView(rv_rule)
          
        rv_rule.addOnScrollListener(object : RecyclerView.OnScrollListener() {
            override fun onScrolled(recyclerView: RecyclerView, dx: Int, dy: Int) {
                super.onScrolled(recyclerView, dx, dy)
                val firstVisiblePosition = rvManager.findFirstVisibleItemPosition()
                val lastVisiblePosition = rvManager.findLastVisibleItemPosition()
                val centerPosition = (firstVisiblePosition + lastVisiblePosition) / 2
                Log.d("yxy", "centerPosition = $centerPosition")
                if (position > centerPosition) {
                    //处于右边的则需要尽量居中
                    rv_rule.post {
                        rv_rule.scrollBy(translationX * (position - centerPosition), 0)
                    }
                }
            }
        })
    }
```

