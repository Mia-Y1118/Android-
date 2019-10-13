#### 一、ArrayList的构造函数

- 默认的构造器，以默认的大小容量来初始化内部的数组(默认是10)

  ```java
  public ArrayList() {
          this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
  }
  ```

- 指定大小来初始化内部数组

```java
public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
  
```

- 用一个Collection对象构造，并将集合的元素添加到ArrayList。

```java
public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
```

#### 二、ArrayList通过一个elementData对象数组存数据，所以ArrayList的容量就是elementData的长度。

- 当构建一个空的ArrayList的时候，这时elementData为空，所以数组容量是0.大小也是0。

- 当向ArrayList中添加一个数据源之后，elementData的容量就为10，大小是1。

- 当向ArrayList中添加第11个元素之后，elementData的容量就是15，大小是11。

  

#### 三、为什么扩容时是扩大了1.5倍

- add(E e)时，首先通过ensureCapacityInternal() —> ensureExplicitCapacity（）方法判断是否需要扩容。
- addAll()也是类似的操作s

```java
/**
     * Appends the specified element to the end of this list.
     *
     * @param e element to be appended to this list
     * @return <tt>true</tt> (as specified by {@link Collection#add})
     */
    public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
```

- 当所需的最小容量大于当前的elemetData数组长度时，需要进行扩容操作，调用grow方法。

```java
 private void grow(int minCapacity) {
        int oldCapacity = elementData.length;
        //原来的容量 + 原来容量又移一位，大约是变成了原来的1.5倍。
   			//采用位移的方法时处于效率的考虑
        int newCapacity = oldCapacity + (oldCapacity >> 1);
   
   			//有可能移位后的还比所需要的小，当newCapacity小于所需最小容量，那么将所需最小容量赋值给newCapacity
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // 当新容量够了的时候，则使用新的容量将数据进行copyof复制，完成扩容
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

