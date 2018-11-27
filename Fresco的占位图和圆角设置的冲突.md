## 占位图和圆角设置冲突 ##
- 现象：如果在布局文件中设置了占位图，并且设置了占位图的scaleType为centerinside,并设置圆角，即一下两个同时使用的时候则会出现拉伸。

   fresco:placeholderImage="@drawable/free_course_bg"
   	fresco:roundedCornerRadius="@dimen/dimen06"
- 解决方法：在代码中动态添加圆角，通过监听的方式去在加载成功或者加载失败的时候添加圆角。

    

            ControllerListener controllerListener = new ControllerListener() {
        
                @Override
        
                public void onSubmit(String id, Object callerContext) {
        }
        
            @Override
            public void onFinalImageSet(String id, @Nullable Object imageInfo, @Nullable Animatable animatable) {
                RoundingParams roundingParams = RoundingParams.fromCornersRadius( Utils.dip2px(context, 6));
                ivFreeCourse.getHierarchy().setRoundingParams(roundingParams);
        		 // 图片加载成功时候会执行onFinalImageSet方法
            }
        
            @Override
            public void onIntermediateImageSet(String id, @Nullable Object imageInfo) {
        		// 设置图片为渐进式的时候，加载失败，会执行onIntermediateImageFailed方法
            }
        
            @Override
            public void onIntermediateImageFailed(String id, Throwable throwable) {
        
            }
        
            @Override
            public void onFailure(String id, Throwable throwable) {
        		// 图片加载失败时候，会执行onFailure方法
            }
        
            @Override
            public void onRelease(String id) {
        
            }
        };
        
         DraweeController controller = Fresco.newDraweeControllerBuilder()
                    .setUri(uri)
                    .setControllerListener(controllerListener)
                    .build();
            view.setController(controller);

## greenDao的逐渐自增必须大写 ##