# DataBinding，了解一下（一）





## 为什么喜欢用DataBinding



假设有一个页面包含三个textview，显示个人信息，下面是正常情况的写法：

>```xml
><?xml version="1.0" encoding="utf-8"?>
><LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
>    xmlns:tools="http://schemas.android.com/tools"
>    android:layout_width="match_parent"
>    android:layout_height="match_parent"
>    android:orientation="vertical">
>
>    <TextView
>        android:id="@+id/tv_name"
>        android:layout_width="wrap_content"
>        android:layout_height="wrap_content"
>        android:textColor="#CCCCCC"
>        tools:text="name" />
>
>    <TextView
>        android:id="@+id/tv_sex"
>        android:layout_width="wrap_content"
>        android:layout_height="wrap_content"
>        tools:text="sex" />
>
>    <TextView
>        android:id="@+id/tv_age"
>        android:layout_width="wrap_content"
>        android:layout_height="wrap_content"
>        tools:text="age" />
>
>    <Button
>        android:id="@+id/button"
>        android:layout_width="wrap_content"
>        android:layout_height="wrap_content"
>        android:text="加一岁" />
>
></LinearLayout>
>```

> ```java
> public class BindingTest extends AppCompatActivity {
>
>     private TextView tvName;
>     private TextView tvSex;
>     private TextView tvAge;
>     private Button button;
>     private User user;
>
>     @Override
>     protected void onCreate(@Nullable Bundle savedInstanceState) {
>         setContentView(R.layout.binding_test);
>         super.onCreate(savedInstanceState);
>         tvName = findViewById(R.id.tv_name);
>         tvSex = findViewById(R.id.tv_sex);
>         tvAge = findViewById(R.id.tv_age);
>         button = findViewById(R.id.button);
>         button.setOnClickListener(new View.OnClickListener() {
>             @Override
>             public void onClick(View v) {
>                 user.age++;
>                 tvAge.setText(user.age + "岁");
>                 if (user.age < 18) {
>                     tvAge.setTextColor(Color.parseColor("#CE0000"));
>                 } else {
>                     tvAge.setTextColor(Color.parseColor("#00FFFF"));
>                 }
>             }
>         });
>       
>         user = getUser();
>
>         if (!TextUtils.isEmpty(user.name)) {
>             tvName.setText(user.name);
>         }
>
>         tvAge.setText(user.age + "岁");
>         if (user.age < 18) {
>             tvAge.setTextColor(Color.parseColor("#CE0000"));
>         } else {
>             tvAge.setTextColor(Color.parseColor("#00FFFF"));
>         }
>
>         if (user.sex) {
>             tvSex.setText("男");
>             tvSex.setTextColor(Color.parseColor("#FFF000"));
>         } else if (user.sex) {
>             tvSex.setText("女");
>             tvSex.setTextColor(Color.parseColor("#000FFF"));
>         }
>     }
>
>     private User getUser() {
>         return new User();
>     }
> }
> ```

典型正常的流程，findview，获取数据，根据数据逻辑setView，添加点击事件监听，当按钮点击时先改变数据，在重新setView。

下面是DataBinding的流程：

>```xml
><?xml version="1.0" encoding="utf-8"?>
><layout xmlns:android="http://schemas.android.com/apk/res/android"
>    xmlns:tools="http://schemas.android.com/tools">
>
>    <data>
>
>        <import type="User" />
>
>        <variable
>            name="user"
>            type="User" />
>    </data>
>
>    <LinearLayout
>        android:layout_width="match_parent"
>        android:layout_height="match_parent"
>        android:orientation="vertical">
>
>        <TextView
>            android:id="@+id/tv_name"
>            android:layout_width="wrap_content"
>            android:layout_height="wrap_content"
>            android:text="@{user.name}"
>            android:textColor="#CCCCCC" />
>
>        <TextView
>            android:id="@+id/tv_sex"
>            android:layout_width="wrap_content"
>            android:layout_height="wrap_content"
>            android:text="@{user.sex?@string/male:@string/female}"
>            android:textColor="@{user.sex?@color/_FFF000:@color/_000FFF}"
>            tools:text="sex" />
>
>        <TextView
>            android:id="@+id/tv_age"
>            android:layout_width="wrap_content"
>            android:layout_height="wrap_content"
>            android:text="@{user.age+@string/yearold}"
>            android:textColor="@{user.age<18?@color/_CE0000:@color/_00FFFF}"
>            tools:text="age" />
>
>        <Button
>            android:id="@+id/button"
>            android:layout_width="wrap_content"
>            android:layout_height="wrap_content"
>            android:text="加一岁" />
>
>    </LinearLayout>
></layout>
>```

>```java
>public class BindingTest extends AppCompatActivity {
>
>    private BindingTestBinding binding;
>    private Button button;
>    private User user;
>
>    @Override
>    protected void onCreate(@Nullable Bundle savedInstanceState) {
>        binding = DataBindingUtil.setContentView(this, R.layout.binding_test);
>        super.onCreate(savedInstanceState);
>        user = getUser();
>        binding.setUser(user);
>        binding.button.setOnClickListener(new View.OnClickListener() {
>            @Override
>            public void onClick(View v) {
>                user.setAge(user.getAge() + 1);
>            }
>        });
>    }
>
>    private User getUser() {
>        return new User();
>    }
>}
>```

不再需要findView，所有包含id的view都在binding里可以直接使用。

不在需要setView，只要改变数据，被绑定上的view自动更新。

Data Binding 之前，我们不可避免地要编写大量的毫无营养的代码，如 findViewById()、setText()，setVisibility()，setEnabled() 或 setOnClickListener() 等，通过 Data Binding , 我们可以通过声明式布局以精简的代码来绑定应用程序逻辑和布局，这样就不用编写大量的毫无营养的代码了。



数据驱动好处：

举例:成绩单分享的添加准考证页面

![avatar](https://github.com/yd613/picRepo/blob/master/WX20180529-110740@2x.png?raw=true)

一个很简单的需求：添加或者修改个人信息，之后点击按钮提交表单

常规的做法是这样：

1. 得到学员信息的数据实体，填充到各种TextView和EditText上面显示信息。
2. 学员修改信息，修改的是控件里存储的信息，数据实体并没有被修改。
3. 点击按钮提交，要各种getText()来得到所有的信息，并提交。

可以看到基本流程比较繁琐，如果需要重新网络拉数据，又要操作一遍更新数据实体，set控件N次，get控件N次。

数据驱动的DataBinding做法：

1. 得到学员信息的数据实体（和常规做法一样，只不过我们构建的是ObserverField等，后面会讲），然后View层把数据实体set到xml文件里和布局绑定。
2. 没有了，基本结束战斗。。。因为绑定的原因，操作的都是数据实体。

这是如果此时网络重新拉数据，只需要改变数据，控件内容就自动改变了，而像EditText由用户输入的更可以双向绑定，不论修改数据或是用户输入导致的变化，数据实体和控件都是同步的。
这样就少了一堆中间的set&get方法过程，规避了很多判空和多线程（）之类的操作，这就是数据驱动的好处。

#### MVVM的简单介绍以及优点

- 数据驱动

> 其实刚才的数据驱动就是MVVM一个很重要的特点，ViewModel层，也就是MVVM中的VM，就是处理数据逻辑的地方，VM层只关心数据，不需要和UI直接打交道。

- 低耦合

> 可以看到数据是独立于UI的，这样产品如果要改UI，那么完全不用动别的代码，比如把TextView改成EditText，只需要动xml文件就可以，而且可以更好的分工， 比如一个人写ViewModel层所有的网络和数据逻辑处理都在这里，一个人写View层，Activity和XML主要画UI。

- 可复用性

> 如果两个页面使用数据源相同可以使用同一个ViewModel 

#### Databinding的缺点

- xml文件代码提示缺失，调试时候日志的支持性不好，xml需要转义
- 逻辑复杂的页面，会导致一些混乱，例如ViewModel之间的通讯，想要为页面创建不同的数据源，但一部分逻辑是在写xml中的。



## 使用DataBinding



### 准备

确保 Android 的 Gradle 插件版本不低于 **1.5.0-alpha1**：

> classpath 'com.android.tools.build:gradle:1.5.0'

然后修改对应模块（Module）的 build.gradle：

> ```
> android {
>     ....
>     dataBinding {
>         enabled = true    
>     }    
> }
> ```





### 布局文件

使用 Data Binding 之后，xml 的布局文件就不再用于单纯地展示 UI 元素，还需要定义 UI 元素用到的变量。所以，它的根节点不再是一个 `ViewGroup`，而是变成了 `layout`，并且新增了一个节点 `data`。

起始根标签是**layout**，接下来一个 **data** 元素以及一个 **view** 的根元素。这个 **view** 元素就是你没有使用 DataBinding 的 layout 文件的根元素。

>```xml
><layout xmlns:android="http://schemas.android.com/apk/res/android">
>    <data>
>    </data>
>    <!--原先的根节点（Root Element）-->
>    <LinearLayout>
>    ....
>    </LinearLayout>
></layout>
>```



在**data**内描述了一个名为user的变量属性，使其可以在这个layout中使用：

>```Xml
><data>
>    <import name="Textutils"/>
>    <variable name="user" type="User" />
></data>
>```



在 layout 的属性表达式写作`@{}`，下面是一个TextView的text设置为user的name属性:

>```xml
><TextView
>    android:id="@+id/tv_name"
>    android:layout_width="wrap_content"
>    android:layout_height="wrap_content"
>    android:text="@{user.name}"
>    android:textColor="#CCCCCC" />
>```





### 被绑定的数据

任何 POJO 对象都能用在 Data Binding 中，但是更改 POJO 并不会同步更新 UI。Data Binding 的强大之处就在于它可以让你的数据拥有更新通知的能力。普通类型的数据被绑定到view上，可以理解为setView，即使数据发生改变，但view并不会跟随数据改变。这当然不是绑定的意义。

#### Observable 对象

下面是一段普通的POJO类：

>```java
>public class ObservableContact {
>    private String mName;
>    private String mPhone;
>
>    public ObservableContact(String name, String phone) {
>        mName = name;
>        mPhone = phone;
>    }
>
>    public String getName() {
>        return mName;
>    }
>
>    public void setName(String name) {
>        mName = name;
>    }
>
>    public String getPhone() {
>        return mPhone;
>    }
>
>    public void setPhone(String phone) {
>        mPhone = phone;
>    }
>}
>```

当一个类实现了 Observable 接口时，Data Binding 会设置一个 listener 在绑定的对象上，以便监听对象字段的变动。

Observable 接口有一个添加/移除 listener 的机制，但通知取决于开发者。为了简化开发，Android 原生提供了一个基类 `BaseObservable` 来实现 listener 注册机制。这个类也实现了字段变动的通知，只需要在 getter 上使用 Bindable 注解，并在 setter 中通知更新即可。

上面的POJO类实现了Observable 接口后，如下所示：

>```java
>public class ObservableContact extends BaseObservable {
>    private String mName;
>    private String mPhone;
>
>    public ObservableContact(String name, String phone) {
>        mName = name;
>        mPhone = phone;
>    }
>
>    @Bindable
>    public String getName() {
>        return mName;
>    }
>
>    public void setName(String name) {
>        mName = name;
>        notifyPropertyChanged(BR.name);
>    }
>
>    @Bindable
>    public String getPhone() {
>        return mPhone;
>    }
>
>    public void setPhone(String phone) {
>        mPhone = phone;
>        notifyPropertyChanged(BR.phone);
>    }
>}
>```

`BR` 是编译阶段生成的一个类，功能与 R.java 类似，用 @Bindable 标记过 getter 方法会在 BR 中生成一个 entry。
当数据发生变化时需要调用 `notifyPropertyChanged(BR.firstName)` 通知系统 `BR.firstName` 这个 entry 的数据已经发生变化以更新UI。

#### ObservableFields

创建 Observable 类还是需要花费一点时间的，如果想要省时，或者数据类的字段很少的话，可以使用 `ObservableField`  以及它的派生 `ObservableBoolean` 、 `ObservableByte` 、 `ObservableChar` 、 `ObservableShort` 、 `ObservableInt` 、 `ObservableLong` 、 `ObservableFloat` 、 `ObservableDouble` 、 `ObservableParcelable` 。

>```java
>public class ObservableFieldContact {
>    public ObservableField<String> mName = new ObservableField<>();
>    public ObservableField<String> mPhone = new ObservableField<>();
>
>    public ObservableFieldContact() {
>        mName.set("ConnorLin");
>			mPhone.set("12345678901");
>
>			String name = mName.get();
>    }
>}
>```

#### Observable Collections 容器类

Observable集合允许键控访问这些data对象。`ObservableArrayMap`用于键是引用类型,如`String`。

>```java
>ObservableArrayMap<String, Object> user = new ObservableArrayMap<>();
>user.put("firstName", "Google");
>user.put("lastName", "Inc.");
>user.put("age", 17);
>```

在layout文件中，通过String键可以访问map：
>```xml
><data>
>    <import type="android.databinding.ObservableMap"/>
>    <variable name="user" type="ObservableMap<String, Object>"/>
></data>
>…
><TextView
>   android:text='@{user["lastName"]}'
>   android:layout_width="wrap_content"
>   android:layout_height="wrap_content"/>
><TextView
>   android:text='@{String.valueOf(1 + (Integer)user["age"])}'
>   android:layout_width="wrap_content"
>   android:layout_height="wrap_content"/>
>```



### 生成绑定

生成的 binding 类将布局中的 View 与变量绑定在一起。就像先前提到过的，类名和包名可以自定义 。生成的 binding 类会继承 ViewDataBinding 。



#### Creating

绑定布局有好几种方式。最常见的是使用 binding 类中的静态方法。inflate 函数会 inflate View 并将 View 绑定到 binding 类上。此外有更加简单的函数，只需要一个 LayoutInflater 或一个 ViewGroup。

> ```java
> MyLayoutBinding binding = MyLayoutBinding.inflate(layoutInflater);
> MyLayoutBinding binding = MyLayoutBinding.inflate(layoutInflater, viewGroup, false);
> ```

如果布局使用不同的机制来 inflate，则可以独立做绑定操作：

> ```java
> MyLayoutBinding binding = MyLayoutBinding.bind(viewRoot);
> ```

有时绑定关系是不能提前确定的。这种情况下，可以使用 DataBindingUtil ：

```java
ViewDataBinding binding = DataBindingUtil.inflate(LayoutInflater, layoutId, parent, 		attachToParent);
ViewDataBinding binding = DataBindingUtil.bindTo(viewRoot, layoutId);
```

在默认情况下，会基于布局文件生成一个继承于 `ViewDataBinding` 的 Binding 类，将它转换成帕斯卡命名并在名字后面接上`Binding`。例如，布局文件叫 `main_activity.xml`，所以会生成一个 `MainActivityBinding` 类。这个类包含了布局文件中所有的绑定关系，会根据绑定表达式给布局文件赋值。在 inflate 的时候创建 binding 的方法如下：

> ```java
> private BindingTestBinding binding;
> private User user;
>
> @Override
> protected void onCreate(Bundle savedInstanceState) {
>    super.onCreate(savedInstanceState);
>    
>    //  ActivityBaseBinding 类是自动生成的
>    binding = DataBindingUtil.setContentView(this, R.layout.binding_test);
>    user = getUser();
>    // 所有的 set 方法也是根据布局中 variable 名称生成的
>    binding.setUser(user);
> }
> ```



#### Views With IDs

布局中每一个带有 ID 的 View，都会生成一个 public final 字段。binding 过程会做一个简单的赋值，在 binding 类中保存对应 ID 的 View。这种机制相比调用 findViewById 效率更高。

```xml
<layout xmlns:android="http://schemas.android.com/apk/res/android">
   <data>
       <variable name="user" type="com.connorlin.databinding.model.User"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.firstName}"
           android:id="@+id/firstName"/>
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.lastName}"
           android:id="@+id/lastName"/>
   </LinearLayout>
</layout>
```

将会在 binding 类内生成：

```java
public final TextView firstName;
public final TextView lastName;
```



#### Variables

每一个变量会有相应的存取函数：

```xml
<data>
    <import type="android.graphics.drawable.Drawable"/>
    <variable name="user"  type="com.connorlin.databinding.model.User"/>
    <variable name="image" type="Drawable"/>
    <variable name="note"  type="String"/>
</data>
```

并在 binding 类中生成对应的 getters 和 setters：

```java
public com.connorlin.databinding.model.User getUser();
public void setUser(com.connorlin.databinding.model.User user);
public Drawable getImage();
public void setImage(Drawable image);
public String getNote();
public void setNote(String note);
```

