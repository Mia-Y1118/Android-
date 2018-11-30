## 2018/1/2

### 一、ExpandableListView的使用方法：

1、ExandableListView是可以扩展的下拉列表，其扩展性在于点击父Item可以拉下或者收起列表，使用于一些场景的使用。

2、具体使用：
	
```java
//①在布局文件中田间ExpandableListView控件。
<ExpandableListView
    android:id="@+id/expandalelistview"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">
</ExpandableListView>

//②添加父列表项Parent_item，子列表项的布局Child_item.
//③写MyExpandableListViewAdapter，继承自BaseExpandableListAdapter.
//④重写Adapter中的方法：

//获取父项的数目
@Override
public int getGroupCount() {
    return 20;
}

//获取某个父项的子项数目
@Override
public int getChildrenCount(int groupPosition) {
    return 2;
}

//获取某个父项
@Override
public Object getGroup(int groupPosition) {
    return null;
}

 //获取某个父项的某个子项
@Override
public Object getChild(int groupPosition, int childPosition) {
    return null;
}

  //获得某个父项的id
@Override
public long getGroupId(int groupPosition) {
    return groupPosition;
}

//获得某个父项的某个子项的id
@Override
public long getChildId(int groupPosition, int childPosition) {
    return childPosition;
}

 //获得父项显示的view(通过Inflate来加载一个Item布局）
@Override
public View getGroupView(int groupPosition, boolean isExpanded, View convertView, ViewGroup parent) {
   if(convertView == null){
       LayoutInflater inflater = (LayoutInflater)
               mContext.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
       convertView = inflater.inflate(R.layout.parent_item,null);
   }
   convertView.setTag(R.layout.parent_item,groupPosition);
   convertView.setTag(R.layout.child_item,-1);
    TextView text = (TextView) convertView.findViewById(R.id.title);
    text.setText("Parent Title");
    return convertView;
}


//获得子项显示的View:(也是通过Inflate来加载一个Item布局)
 @Override
public View getChildView(int groupPosition, int childPosition, boolean isLastChild, View convertView, ViewGroup parent) {
   if(convertView == null){
       LayoutInflater inflater = (LayoutInflater) mContext.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
       convertView = inflater.inflate(R.layout.child_item,null);
   }
   convertView.setTag(R.layout.parent_item,groupPosition);
   convertView.setTag(R.layout.child_item,childPosition);
   TextView text = (TextView) convertView.findViewById(R.id.child_item);
   text.setText("Childtitle");
   text.setOnClickListener(new View.OnClickListener(){

       @Override
       public void onClick(View v) {
           Toast.makeText(mContext,"You have click the child item",Toast.LENGTH_LONG).show();
       }
   });
    return convertView;
}

//子项是否可选，如果需要设置子项的点击事件，需要返回true
@Override
public boolean isChildSelectable(int groupPosition, int childPosition) {
    return true;
}
```

3、对于ExpandableListView的部分属性总结：
	①divider:这个属性用来设置父类之间的分割线样式。
	②childDicider:这个属性用来设置同一个父项下，子类之间的分割线。
	android:childDivider="@color/colorPrimary"
	android:dividerHeight="3dp"

### 二、注意在嵌套ScorllView的时候，会出现显示不全的问题，需要自定义ExpandableListView,并重写onMeasure方法）

```kotlin

/**
 * 作者  yangxy
 * 时间  2018/11/2915:49
 * 描述  为解决ScrollView与ExpandableListView合用时出现显示不全的问题
 */
class MyExpandableListView@JvmOverloads constructor(
        context: Context, attrs: AttributeSet? = null, defStyleAttr: Int = 0)
    : ExpandableListView (context, attrs, defStyleAttr){

    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        val  expandSpec = MeasureSpec.makeMeasureSpec(Integer.MAX_VALUE shr(2),MeasureSpec.AT_MOST)
        super.onMeasure(widthMeasureSpec, expandSpec)
    }
}
```






​	

​	