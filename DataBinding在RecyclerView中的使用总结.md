#### DataBinding在RecyclerView中的使用总结

##### 1、在item的布局文件中可以导入显示的实体类，注意实体类需要继承自BaseObservable,在get方法上方添加@Bindable,在set方法中添加：notifyPropertyChanged(BR.packageId);类似的代码。

##### 2、在填写Item布局文件的时候，如果需要根据vholder中的某些字段来进行可见的判断，则需要导入View的包

##### 3、写Adapter,首先使用自己的ViewHolder,需要将ViewHolder声明为Public,否则布局文件无法使用。并且在onCreateViewHolder时，如果要set布局文件的ViewHolder,则这个ViewHolder需要与返回的ViewHolder相同，例如：

```java
@Override
    public RecyclerView.ViewHolder _onCreateViewHolder(ViewGroup parent, int viewType) {
        binding = DataBindingUtil.inflate(LayoutInflater.from(context), 		R.layout.item_term_subject, parent, false);
        MyViewHolder holder = new MyViewHolder(context,binding);
        binding.setSubjectHolder(holder);
        return holder;
    }
```

#####  4、在ViewHolder中可以写绑定Item的方法，然后在onBindViewHolder中调用，注意书写格式如下：

```java
public void setDataBinding(AfterAllTermEntity.SubjectEntity entity){

            if(entity == null){
                return;
            }
            binding.setVariable(BR.subjectItem,entity);
            binding.executePendingBindings();// 用于刷新绑定的Item,防止复用
        }
```

##### 5、在ViewHolder的构造方法中传入的应该是生成的Binding类，super(binding.getRoot());

```java
    public MyViewHolder(Context context, ItemTermSubjectBinding binding) {
            super(binding.getRoot());
            this.context = context;
            this.binding = binding;
        }

```

##### 6、如果要设置Item或者Item中的某个组件的点击事件，如果需要传递参数，则需要在布局文件中使用Lamada表达式。



##### 7、在RecyclerView中使用DataBinding的注意事项：

​	a、一般Adapter的ViewHolder仅仅做绑定数据的作用，所以尽量减少将ViewHolder进入布局文件中

​	b、对RecyclerView的第一个item等需要做特殊处理的时候，可以在Entity中声明多余的变量，从而用于改变View的部分属性。

​	c、对于item的点击事件，指定到相应的Activity去实现。