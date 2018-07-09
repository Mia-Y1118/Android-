## 一、泛型的概念

#### 1、泛型是什么？

	泛型：即"参数化类型",就是将类型由原来的具体类型参数化，使代码可以应用于多种类型。类似于方法中的变量参数，在调用类的时候需要传入具体的类型参数。可以用在类，接口，方法。

#### 2、泛型的用处？

##### ①泛型是用来提高类型安全，消除强制类型转换的烦恼。

```java
 a、未用泛型：
public static void notUseGeneric(){

        List arrayList = new ArrayList();
        arrayList.add("1000");
        arrayList.add(100);
        for (int i = 0; i < arrayList.size(); i++ ){
            String item = (String) arrayList.get(i);
            Log.d("yangxy", "notUseFan: " + item);
        }
    }
```

![](https://i.imgur.com/3m981fQ.png)

	应该在编译阶段就发现程序抛出这样的异常，泛型应运而生。

​	b、使用泛型：

![](https://i.imgur.com/IQVDYZP.png)



##### ②返回多个不同类型的对象（使用元组）

```java
public class TwoTuple<A,B>{
    public final A first;
    public final B second;
    public TwoTuple(A a,B b){
        first = a;
        second = b;
    }
    public String toString(){
        return "("+first+","+second+")";
    }
}
```



## 二、Kotlin泛型与Java泛型的相似之处（类，接口，方法）

#### 1、声明一个泛型类：常见用T、E、K、V等形式的参数常用于表示泛型，在实例化泛型类时，必须指定T的具体类型

- Java写法：

  ```java
  public class Generic<T>{
          private T key;
          public Generic(T key) { 
          this.key = key;
      }
  }
  
  使用：
  Generic<Integer> genericInteger = new Generic<Integer>(123456);
  ```

- Kotlin写法：

   ```kotlin
   class Generic<T>(private val key: T){
       
   }
   
   使用：
    val a : Generics<Integer> = Generics<Integer>(1)
   ```
#### 2、声明一个接口：

- Java写法：

  ``` java
  public interface Generator<T> {
    		public T doGenerate();
  	}
  
  使用：
  ①当未传入具体参数时：实现接口的类也必须加上泛型，否则编译器会报错"Unknown class"
  	class FruitGenerator<T> implements Generator<T>{
      @Override
      public T doGenrate() {
          return null;
      	}
  	}
  
  ②当T替换成具体的参数化类型的时候，实现类中的泛型可以省略。
  public class FruitGenerator implements Generator<String> {
  	@Override
    	public String doGenerate() {
        	return null;
        	}
  	}
  ```

- Kotlin写法：

   ```kotlin
   interface Generator<T> {
       fun doGenerate(): T
   }
   
   使用：
   ①当未传入具体参数时：实现接口的类也必须加上泛型，否则编译器会报错"Unknown class"
   class FruitGenerator<T> : Generator<T> {
       override fun doGenerate(): T? {
        		return null
    		}
   	}
   
   ②当T替换成具体的参数化类型的时候，实现类中的泛型可以省略。
   
   	class FruitGenerator : Generator<String> {
    		override fun doGenerate(): String? {
        		return null
    		}
   	}
   ```

#### 3、泛型方法调用方法的时候指明泛型的具体类型（泛型在返回值的前面）

- Java的写法：在public与返回值之间的<T>必不可少，表明这是一个泛型方法，并且声明了一个泛型T

    ```java
    public <T> T showFruit(Generic<T> generator){
    	        T test = generator.doGenerate();
    	        return test;
    	    }
    ```

- Kotlin的写法：

   ```kotlin
   fun <T>  showFruit(generator: Generator<T>): T {
       		 return generator.doGenerate()
    	}
   ```

- 当泛型类中包含泛型方法的时候，当采用的相同的泛型标识符的时候，方法中的泛型标识符是一个全新的类型：

   ```kotlin
   	class GenerateTest<T> {
   		fun show_1(t: T) { // T的类型与类中泛型的类型一致
      			println(t.toString())
            }
           		
            fun <T> show_2(t: T) { // 泛型方法中的T的类型可以与类的T所代表的类型相同，也可以不相同
      			 println(t.toString())
   		}
   	}
   
   使用：
   	fun doGenrateTest(){
          var a = GenerateTest<String>()
          a.show_1("1234")
          a.show_1(0)//编译会出错，原因是show_1的类型为String
          a.show_2(0)
       }
   ```

## 三、Java泛型与Kotlin泛型的不同之处

#### 1、Java泛型拥有通配符类型参数（? extends E)

​	表示此方法或者类等接收E或者E的一些子类对象的集合，而不只是E自身。（就意味着能从其中读取E,但是并不能写入，因为不知道什么对象符合那个未知E的子类型），带 extends 限定（上界）的通配符类型使得类型是协变的。（理解为：由于Java中的泛型是不型变的，所以List<String>并不是List<Object>的子类型。但是可以通过通配符的形式使其成为协变的，就可以传入某种类型的父类或者某种类型的子类）

```java
①不可型变
void demo(Source<String> strs) {
  Source<Object> objects = strs; // ！！！在 Java 中不允许
}


②采用通配符的方式实现协变

public void showValue(Generic<? extends Number> obj){
	  .......
	}
	
使用：

	Generic<String> generic1 = new Generic<String>("11111");
	Generic<Integer> generic2 = new Generic<Integer>(2222);
	Generic<Float> generic3 = new Generic<Float>(2.4f);
	Generic<Double> generic4 = new Generic<Double>(2.56);

	showValue(generic1);//会报错，由于String不是Number的子类
	showValue(generic2);
	showValue(generic3);
	showValue(generic4);
```

#### 2、Kotlin 中没有通配符类型，其拥有 **声明处型变**  与  **类型投影**。

- ##### 声明处型变:声明处的类型变异使用协变注解修饰符：in、out。

```java
    ①、out修饰符,在类型参数声明时使用（生产者：从其中读取对象）（out T 对应 ？extends T)
    interface Source<out T> {
	    fun nextT(): T
	}
	fun demo(strs: Source<String>) {
	    val objects: Source<Any> = strs // T 是一个 out参数
	}

	方法中运用：

	fun showValue(obj: Generator<out Number>) {
				.......
	   		}
	var a: Generator<String> = Generator<String>()
    showValue("fruit")//会报错，由于String不是Number的子类

    var b:Generator<Integer> = Generator<Integer>()
    showValue(fruit1)
        
    
    ②、in 修饰符,在类型参数声明时使用，类型参数逆变，可以作为入参的类型但是无法作为返回值的类型（消费者：向其中写入对象）(in T 对应 ？super T)
    // 定义一个支持逆变的类
	class Runoob<in A>(a: A) {
	    fun foo(a: A) {
	    }
	}
	
	fun main(args: Array<String>) {
	    var strDCo = Runoob<String>("a")
	    var anyDCo = Runoob<Any>("b")
	    strDCo = anyDCo
	}
```

- ##### 类型投影：使用处型变。

  ```java
  val ints: Array<Int> = arrayOf(1, 2, 3)
  	val any = Array<Any>(3) { "" } 
  	copy(ints, any) // 错误：期望 (Array<Any>, Array<Any>)
  
  	fun copy(from: Array<Any>, to: Array<Any>) {
      for (i in from.indices)
          to[i] = from[i]
  	}
  ```

- ##### 星投影：在对类型参数一无所知，但可以通过定义泛型类型的这种投影来安全的使用泛型

  ```java
  ① 对于 Foo <out T : TUpper>，如果已知TUpper的类型，其中 T 是一个具有上界 TUpper 的协变类型参数，Foo <> 等价于 Foo <out TUpper>。 这意味着当 T 未知时，可以安全地从 Foo <> 读取 TUpper 的值
  
  ② 对于 Foo <in T>，其中 T 是一个逆变类型参数，Foo <> 等价于 Foo <in Nothing>。 这意味着当 T 未知时，没有什么可以以安全的方式写入 Foo <>。
  
  ② 对于 Foo <T : TUpper>，其中 T 是一个具有上界 TUpper 的不型变类型参数，Foo<*> 对于读取值时等价于 Foo<out TUpper>， 而对于写值时等价于 Foo<in Nothing>。
  
  如果一个泛型类型中存在多个类型参数, 那么每个类型参数都可以单独的投射. 
  
  比如, 如果类型定义为interface Function<in T, out U> , 那么可以出现以下几种星号投射:
  Function<*, String> , 代表 Function<in Nothing, String> ;
  Function<Int, *> , 代表 Function<Int, out Any?> ;
  Function<, > , 代表 Function<in Nothing, out Any?> .
  
  注意: 星号投射与 原生类型(raw type)非常类似, 但可以安全使用
  ```

  

## 四、类型擦除：

##### 	Kotlin为泛型声明用法执行的类型安全检测仅在编译器进行，进行泛型类型的实例不保留关于某类型实参的任何消息，其类型信息成为被擦除。Foo<Bar>与Foo<Baz?>的实例都会被擦除为Foo<*>