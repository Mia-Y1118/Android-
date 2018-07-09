####Databinding布局文件的 语法
```html
<layout>
	<data>
	</data>
	
	<原来的布局>
	</原来的布局>
	
</layout>
```
Databinding的布局文件被分成两部分，一部分是data标签代表声明和引用，一部分是原来的布局，比原来多出了Databinding的表达式。我们也分两步说明。

#####一，data标签

variable标签表示声明变量， 所有被声明的变量会生成对用的set方法，ViewDataBinding.setHandler();


像Java类一样，我们可以使用data标签中使用import标签导入我们在xml文件绑定数据时需要使用到的类，所有的类都需要写全类名，除了java.lang包下的类。

```html
<data>
    <import type="android.view.View"/>
    
    <variable 
    		  name="vmodel"
    		  type="com.xyz.app.vmodel.HelloVModel"/>
    		  
    <variable 
    		  name="user"
    		  type="com.xyz.app.entity.User"/>		  
    <variable
            name="number"
            type="String"/>
</data>
```

#####二，页面布局
#####1，基本语法：
xml布局文件里属性使用@{}来替换，所有的Databinding相关内容都写在@{}中。
>普通绑定：


```Java
<EditText
   android:text="@{user.userName}"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:visibility="@{vmodel.isExist? View.VISIBLE : View.GONE}"/>
```
>双向绑定：

```Java
<EditText
   android:text="@={user.userName}"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:visibility="@{vmodel.isExist? View.VISIBLE : View.GONE}"/>
```
这里User是一个实体类extends BaseObservable，我们使用‘.’其实代表的是getter方法，所以不论使用ObserveField还是BaseObservable都用fieldName就可以，不需要‘.getUserName’。

>可以使用的表达式

```java
表达式语言与 Java 表达式有很多相似之处。下面是相同之处：
数学计算 + - / * %
字符串连接 +
逻辑 && ||
二进制 & | ^
一元 + - ! ~
位移 >> >>> <<
比较 == > < >= <=
instanceof
组 ()
字面量 - 字符，字符串，数字， null
类型转换
函数调用
字段存取
数组存取 []
三元运算符 ?：
```
>不能使用的表达式

```java
this
super
new
显式泛型调用 <T>
```
#####2，事件处理语法
>第一种，引用调用实在编译期处理，如果有错误会在编译期报错，这时候定义的点击监听不能省略参数View

```html
<Button
	android:onClick="@{vmodel.isDoubleCheck ? vmodel::onConfirmDoubleCheck : vmodel :: onConfirm}"
	android:text="@{vmodel.isDoubleCheck ? @string/tscript_double_check : @string/tscript_confirm }"
	/>
```

```java
public void onConfirm(View view){}
public void onConfirmDoubleCheck(View view){}
```


>第二种，监听绑定，监听绑定在事件发生时调用，可以使用任意表达式

```java
	public void onNoParam() {
        //do something
    }

    public void onNormalClick(View view) {
        //do something

    }

    public void onMultiParamClick(View view, Context context) {
        //do something
    }
```

这种情况下如果无参数可以省略参数，如果是多参数必须把参数全部写上且对应，可以看到使用的是lambda表达式。

```html
		 <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="@{() -> act.onNoParam()}"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="@{(yigeView) -> act.onNormalClick(yigeView)}"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="@{(hahaView) -> act.onMultiParamClick(hahaView,context)}"/>
```
binding 类会生成一个命名为 context 的特殊变量(其实就是 rootView 的 getContext() ) 的返回值)，这个变量可用于表达式中。 如果有名为 context 的变量存在，那么生成的这个 context 特殊变量将被覆盖。

#####3，集合数组
数组，list，sparse list，和 map，可以用 [] 操作符来存取


```html
<data>
    <import type="com.sunzxyong.databinding.User"/>
    <import type="java.util.List"/>
    <variable name="userList" type="List&lt;User>"/>
    <import type="java.util.Map"/>
    <variable name="map" type="Map&lt;String, String>"/>
    <variable name="key" type="String"/>
</data>
<EditText
   android:text="@{userList[0].userName}"
   android:hint="@{map[key]}"
   android:background=""
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
/>
```
#####4, 资源引用

一样的使用@即可


```html
android:text="@{@string/fullname(user.fullName)"
```
#####5,Null运算符
Null合并运算符 ?? 会在非 null 的时候选择左边的操作，反之选择右边。


```html
android:text="@{user.lastName ?? @string/deafultName}"
```
#####6,转义字符
xml中一些字符需要使用转义

原字符       | 转义字符        
--------------------|------------------|
空格     | \&nbsp;   | 
<       | \&lt;     |
&       | \&amp;    | 
"       | \&quot;   | 
‘       | \&apos;   | 
×       | \&times;  |
÷       | \&divide; |
