### ShareElement:可以实现Activity以及Fragment的转场动画。

- 要实现自定义的ShareElement动画，一切的重点都在于Activity对外暴露的回调ShareElementCallback.

  - 可以通过两个函数进行回调的设置：
    - activity.setExitSharedElementCallback(callback)
    - activity.setEnterSharedElementCallback(callback)

- ShareElementCallback有以下7个回调，最麻烦的是，这几个回调在进入和退出时的调用顺序是不一样的。

  -  最先调用，用于动画开始前替换ShareElements, 比如在Activity B翻过若干页大图之后，返回Activity A     *的时候需要缩小回到对应的小图，就需要在这里进行替换public void onMapSharedElements(List<String> names, Map<String, View> sharedElements) {} 

  - 表示ShareElement已经全部就位，可以开始动画了 

    public void onSharedElementsArrived(List<String> sharedElementNames, List<View> sharedElements, OnSharedElementsReadyListener listener) {} 

  - 在之前的步骤里(onMapSharedElements)被从ShareElements列表里除掉的View会在此回调,*不处理的话默认进行alpha动画消失 

     public void onRejectSharedElements(List<View> rejectedSharedElements) {} 

  - 在这里会把ShareElement里值得记录的信息存到为Parcelable格式，以发送到Activity B     *默认处理规则是ImageView会特殊记录Bitmap、ScaleType、Matrix，其它View只记录大小和位置 

     public Parcelable onCaptureSharedElementSnapshot(View sharedElement, Matrix viewToGlobalMatrix, RectF screenBounds) {} 

  - 在这里会把Activity A传过来的Parcelable数据，重新生成一个View，这个View的大小和位置会与Activity A里的     *ShareElement一致， 

    public View onCreateSnapshotView(Context context, Parcelable snapshot) {} 

  - public void onSharedElementStart(List<String> sharedElementNames, List<View> sharedElements, List<View> sharedElementSnapshots) {}  

  - public void onSharedElementEnd(List<String> sharedElementNames, List<View> sharedElements, List<View> sharedElementSnapshots) {} 