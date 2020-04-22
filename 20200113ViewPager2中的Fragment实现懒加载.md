#### 一、ViewPager2需要使用FragmentStateAdapter

- ##### 不会调用Fragment的setUserVisibleHint(在Android X中已经被废弃)，所以不能依靠setUserVisibleHint 来判断Fragment是否可见。

- ##### FragmentStateAdapter 会自动销毁不再用的Fragment（打log发现销毁倒数第三个),如果需要 首次加载后不再进行接口请求，则需要设置ViewPager的offscreenPageLimit

```kotlin

/**
 * Created by Yangxy on 2020-01-13
 * description --
 */
abstract class LazyFragment : Fragment() {

    private var isFirstLoad = true


    override fun onResume() {
        super.onResume()
        if (isFirstLoad) {
            isFirstLoad = false
            lazyLoad()
        }
    }

    abstract fun lazyLoad()
}

//设置offscreenPageLimit 为列表总数
vp_node.offscreenPageLimit = nodeList.size

```

