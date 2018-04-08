## AlertDialog与FragmentDialog的区别 ##
- 使用AlertDialog的时候，如果屏幕发生变化，导致Activity重建，然后之前显示的对话框就不见了。如果要想在屏幕旋转的时候显示对话框，就需要做一定的处理，在Activity要销毁的时候设立一个标识，看这时的对话框是否显示，如果是，那么activity在下次重建的时候直接显示对话框。
- 如果使用DialogFragment,当屏幕旋转的时候，fragmentManager会自动管理DialogFragment的生命周期，其主要优势是：旋转屏幕和按下back键的时候可以更好的管理其生命周期。
- Dialog:有很多种形式(进度条，等待，多选，单选，列表等），均是通过builder的形式进行的，也可以自定义产生dialog.