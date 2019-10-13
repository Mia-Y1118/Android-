一、Fresco加载动图

```java
val controller = Fresco.newDraweeControllerBuilder()
        .setAutoPlayAnimations(true)
        .setUri(UriUtil.getUriForResourceId(R.drawable.gif_short_video_play_finish_dialog))
        .setControllerListener(object : BaseControllerListener<ImageInfo>() {
            override fun onFinalImageSet(
                    id: String?,
                    @Nullable imageInfo: ImageInfo?,
                    @Nullable animatable: Animatable?) {
                if (animatable is AnimatedDrawable2) {
                    val animatedDrawable = animatable as AnimatedDrawable2?
                    animatedDrawable!!.animationBackend = LoopCountModifyingBackend(animatedDrawable.animationBackend)
                }
            }
        })
        .build()
gifImage.controller = controller


class LoopCountModifyingBackend(animationBackend: AnimationBackend?)
        : AnimationBackendDelegate<AnimationBackend>(animationBackend) {

        override fun getLoopCount(): Int {
            // GIF只需要播放一次
            return 1
        }
    }
```

