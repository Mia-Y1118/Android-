# 点击事件的分发过程 #

## 点击事件的分发或称主要由3个重要的方法来实现 ##
	dispatchTouchEvent:如果事件能传递给当前的View,那么此方法一定会被调用，返回的结果受当前View的onTouch和下级View的dispatchTouchEvent的影响，表示是否消耗掉当前事件。
	
	onInterceptTouchEvent:用来表示是否拦截当前的事件，如果这个事件已经被拦截了，那么当前的方法不会再次被调用。
	
	onTouchEvent:用来处理点击事件，返回结果表示是否消耗掉当前的事件，如果不消耗，那么当前View无法再次接收到事件。

## 事件传输规则 ##

	对于一个ViewGroup来说，点击事件产生后，首先会传递给它，这时它的dispatchTouchEvent就会被调用，如果这个的ViewGroup的onIterceptTouchEvent方法返回True,就表示这个ViewGroup要拦截当前的事件,接着这个事件就会交给这个ViewGroup来处理,即当前的OnTouchEvent就会被调用。如果当前ViewGroup的onIterceptTouchEvent返回false,表示当前的ViewGroup不拦截当前的事件，这时当前的事件就会往下给ViewGroup的子元素。如果子元素的onTouchEvent返回false,则当前View的父View的onTouchEvent会被调用。






