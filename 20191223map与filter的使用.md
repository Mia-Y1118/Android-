#### 1、map()

- map是指“映射”。[].map(); 基本用法跟forEach方法类似

```
packageList.forEach { 
    it.firProjectId = 9
}

packageList.map { 
    it.firProjectId = 9
}
```

- 注意

```java
["1", "2", "3"].map(parseInt);  //返回的值为[1,NaN,NaN]

那么如果要得到[1,2,3]该怎么写。
["1","2","3"].map((x)=>{
    return parseInt(x);
});

["1","2","3"].map(x=>parseInt(x));
```

#### 2、filter()过滤：数组filter后，返回过滤后的新数组

```
val mbalist =  packageList.filter {
    mbaList.contains(it.firProjectId)
}
```

#### 3、flatMap()：flatMap包含两个操作：会将每一个输入对象输入映射为一个新集合，然后把这些新集合连成一个大集合

