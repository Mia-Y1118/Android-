一、在一个Activity的页面，布局文件中写一个tabLayout与一个ViewPager

二、将tabLayout获取到的List去初始化这个Viewpager:

- 设置ViewPager的Adapter,与当前的Item.

- 需要自己写一个ViewPagerAdapter

  - 继承自FragmentPagerAdapter或者FragmentStatePagerAdapter.
  - 在getItem中去返回相应的Fragment 和getCount中去返回Fragment的数量

- 给tabLayout添加选择监听：

  - 当选中的时候需要设置ViewPager的currentItem的为当前选中tab的索引

  - 设置默认情况下第一个tabLayout的view.

    

三、新建Fragment

- 在onCreateView中进行页面的初始化：加载一个View
- 在onViewCreated中进行数据的获取：Intent传递的数据，然后进行View的初始化，请求数据。
- 注意在接口返回回来的时候需要判断Activity的状态。

四、FragmentPagerAdapter 与FragmentStatePagerAdapter的区别

- FragmentPagerAdapter继承自PagerAdapter,更专注于每一页均为Fragment的情况，该类的每一个生成的Fragment都保存在内存之中。适用于那些相对静态的页面，数量比较少。如果需要处理有很多页面，并且数据动态性较大，占用内存较多的情况，应该使用FragmentStatePagerAdapter.

  - getItem():目的是为生成新的Fragment对象，如果需要向Fragment对象传递相对静态的数据时，一般通过setArguements()来进行设置。如果需要将一些动态的数据传递给该Fragment,那么该部分代码不适合放到getItem()中，因为当数据集发生变化的时候，往往对应的Fragment已经生成。因为PagerAdapter.notifyDataSetChanged()后的getItem()不会被调用。

  - instantiateItem():函数中判断一下要生成的Fragment是否已经生成过了，如果已经生成过了，就使用旧的，旧的将被Fragment.attach();如果没有，就调用getItem()生成一个新的，新的对象将被FragmentTransation.add()。并且如果要在Fragment生成之后进行一些值的动态的设置，需要将设置过程放在instantiateItem()之中。

  - destroyItem():该函数被调用后，会对Fragment进行FragmentTransaction.detach()。这里不是remove(),只是detach(),因此Fragment还在FragmentManager管理中，Fragment所占用的资源不会被释放。

    

- FragmentStatePagerAdapter:继承自PagerAdapter,该PagerAdapter的实现将只保留当前页面，当页面离开实现后，就会被消除，释放其资源。而在页面需要显示的时候，就生成新的页面，好处就是在拥有大量的页面的时候不会占用大量内存。

  - getItem():生成新的Fragment对象，并设置一些相对静态的参数
  - instantiateItem()
  - destroyItem():将Fragment移除，即调用FragmentTransaction.remove(),并释放其资源。                                                