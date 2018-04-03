## ViewPager的切换动画 ##
- 在ViewPager中有一个setPageTransformer的方法，在其中可以设置ViewPager的过渡切换。
- 实现的动画需要实现ViewPager.PageTransformer来实现切换的效果。
- 目前实现是生硬的将中间的图片变大，其他两侧的图片变小。

		public class MyAnimation implements ViewPager.PageTransformer {
		
		    private static final float MIN_SCALE = 1F;
		    private static final float MAX_SCALE = 1.5F;
		    public void transformPage(View view, float position) {
		        if(position <= -1){//左边
		            view.setScaleY(MIN_SCALE);
		        }else if(position < 1 ){
		            view.setScaleY(MAX_SCALE);
		        }else {
		            view.setScaleY(MIN_SCALE);
		        }
		    }
		
		}
