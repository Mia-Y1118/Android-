##一、ImageView的scaleType:
- matrix:表示原图从ImageView的左上角绘制，如果原图大于ImageView,那么多余的部分则裁剪掉，如果原图小于ImageView，那么对原图不做任何处理。

- fitXY：将图片按比例放至View的宽度或者高度(取宽和高的最小值），然后居上或者居左显示。
- fitStart：将图片按照原来的的缩放比例缩放之后居左显示
- fitCenter：将图片按比例缩放之后居中显示
- fitEnd：将图片按照比例缩放后居右或者居下显示
- center：将原图按照原来的大小剧中，如果图片超过ImageView的大小，剪裁掉多余的部分
- centerCrop：目标是将ImageView填充满，即按照比例缩放原图，将多余的部分宽或者高度裁剪掉
- centerInside：目标是将原图完整的显示出来，是按照比例缩放原图，使得ImageView可以将原图完整显示。