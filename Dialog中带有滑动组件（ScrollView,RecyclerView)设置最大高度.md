##### 一、设置RecyclerView的最大高度,在setAdapter之后根据List的数据进行设置

```java
  private void setMaxHeight(List<CourseHomeWorkIdEntity> list) {
        if (list == null) return;
        if (list.size() > 7) {
            //设置最大高度
            ViewGroup.LayoutParams lp = viewCoursewareListview.getLayoutParams();
            lp.height = (int) Utils.dip2px(act,511f);
            viewCoursewareListview.setLayoutParams(lp);
        }
    }
```

