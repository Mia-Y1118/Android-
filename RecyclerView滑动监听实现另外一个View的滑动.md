一、实现View的滑动监听：

```java
 listView.addOnScroll((view, firstVisibleItem, visibleItemCount, totalItemCount, scrollHeight) -> {

            if (scrollHeight > screenHeight / 2) {
                entryContribute(scrollHeight - screenHeight / 2);
            } else {
                iv_contribute.setVisibility(View.GONE);
            }

        });


    /**
     * 投稿按钮的弹出动画
     */
    private void entryContribute(float translationY) {
        iv_contribute.setVisibility(View.VISIBLE);
        if (translationY > Utils.dip2px(context, 72f)) {
            translationY = Utils.dip2px(context, 72f);
        }
        RelativeLayout.LayoutParams lp = (RelativeLayout.LayoutParams) iv_contribute.getLayoutParams();
        lp.width = (int)Utils.dip2px(context,57f);
        lp.height = (int)Utils.dip2px(context,57f);
        lp.bottomMargin = (int) (translationY - Utils.dip2px(context,57f));
        iv_contribute.setLayoutParams(lp);
    }
```

