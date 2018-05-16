##一、Kotlin的基础语法：
- 包声明：代码文件的开头一般为包的声明，如果未指定包名，则默认为default。

		package com.runoob.main
		
		import java.util.*
		
		fun test() {}
		class Runoob {}

- 函数定义：函数定义使用关键字fun,参数的格式为：参数：类型
  
		fun sum(a:Int,b:Int) : Int{ // Int 参数，返回值 Int
			
		}

		1、表达式作为函数体，返回类型自动推断：
		fun sum(a: Int, b: Int) = a + b
		
		public fun sum(a: Int, b: Int): Int = a + b   // public 方法则必须明确写出返回类型

		2、无返回值的函数(类似Java中的void)：
		fun printSum(a: Int, b: Int): Unit { 
		    print(a + b)
		}
		
		
		// 如果是返回 Unit类型，则可以省略(对于public方法也是这样)：
		public fun printSum(a: Int, b: Int) { 
		    print(a + b)
		}

		3、可变长参数函数：
		fun vars(vararg v:Int){
			for(vt in v){
				print(vt)
			}
		}

		// 测试
		fun main(args: Array<String>) {
		    vars(1,2,3,4,5)  // 输出12345
		}

		4、定义常量与变量：
		定义变量：var <标识符> : <类型> = <初始值>
		定义常量：val <标识符> : <类型> = <初始值>

		5、字符串模板
		$表示一个变量名或者变量值
		$varName表示变量值
		${varName.fun()}表示变量的方法返回值

		6、NULL检查机制
		Kotlin的空安全设计对于声明可为空的参数，再使用的时候进行空判断处理。
		①字段后！！像Java一样抛出空异常
		②字段后加？可不做处理返回为null或者配合？：做空判断处理

		//类型后面加?表示可为空
		var age: String? = "23" 
		//抛出空指针异常
		val ages = age!!.toInt()
		//不做处理返回 null
		val ages1 = age?.toInt()
		//age为空返回-1
		val ages2 = age?.toInt() ?: -1

		7、类型检测及自动类型转换：使用is运算符来检测一个表达式是否某类型的一个实例
		fun getStringLength(obj: Any): Int? {
		  if (obj !is String)
		    return null
		  // 在这个分支中, `obj` 的类型会被自动转换为 `String`
		  return obj.length
		}

		8、区间
		区间表达式由具有操作符形式 .. 的 rangeTo 函数辅以 in 和 !in 形成。

		for (i in 1..4) print(i) // 输出“1234”

		for (i in 4..1) print(i) // 什么都不输出
		
		if (i in 1..10) { // 等同于 1 <= i && i <= 10
		    println(i)
		}
		
		// 使用 step 指定步长
		for (i in 1..4 step 2) print(i) // 输出“13”
		
		for (i in 4 downTo 1 step 2) print(i) // 输出“42”
		
		
		// 使用 until 函数排除结束元素
		for (i in 1 until 10) {   // i in [1, 10) 排除了 10
		     println(i)
		}

		for (i in 4 downTo 1 step 2) print(i) // 输出“42”

##二、Kotlin的基本数据结构
- Kotlin的基本数值包括Byte,Short,Int,Long,Float,Double等，不同于Java的是字符不属于数值类型，是一个独立的数据类型。

		 Double 64位  8字节
		 Long   64位  8字节
		 Float  32位  4字节
		 Int    32位  4字节
		 Short  16位  2字节
		 Byte   8位   1字节
- 字面常量：下面是所有类型的字面常量：

		十进制：123
		长整型以大写的 L 结尾：123L
		16 进制以 0x 开头：0x0F
		2 进制以 0b 开头：0b00001011
		注意：8进制不支持

- 比较两个数字：由于Kotlin中没有基础数据类型，只有封装的数字类型，没定义一个变量，Kotlin就默认封装了一个对象，这样可以保证不出现空指针。数字类型也一样，所有在比较两个数字的时候，就有比较数据大小和比较两个对象是否相同的区别了。**在 Kotlin 中，三个等号 === 表示比较对象地址，两个 == 表示比较两个值大小。**

		fun main(args: Array<String>) {
	    val a: Int = 10000
	    println(a === a) // true，值相等，对象地址相等
	
	    //经过了装箱，创建了两个不同的对象
	    val boxedA: Int? = a
	    val anotherBoxedA: Int? = a
	
	    //虽然经过了装箱，但是值是相等的，都是10000
	    println(boxedA === anotherBoxedA) //  false，值相等，对象地址不一样
	    println(boxedA == anotherBoxedA) // true，值相等
		}

- 类型转换：由于不同的表示方式，较小类型并不是较大类型的子类型，较小的类型不能隐式转换成较大的类型。也就是如果不进行显示转换的情况下，Byte就不能转换成Int变量。

		val b: Byte = 1 // OK, 字面值是静态检测的
		val i: Int = b.toInt() // OK

		每种数据类型都有下面的这些方法进行类型转换：
		toByte(): Byte
		toShort(): Short
		toInt(): Int
		toLong(): Long
		toFloat(): Float
		toDouble(): Double
		toChar(): Char

- 位操作符
		shl(bits) – 左移位 (Java’s <<)
		shr(bits) – 右移位 (Java’s >>)
		ushr(bits) – 无符号右移位 (Java’s >>>)
		and(bits) – 与
		or(bits) – 或
		xor(bits) – 异或
		inv() – 反向
- 字符：和Java中不一样，kotlin中的Char不能直接和数字操作，Char必须式单引号'包含起来的，比如：'0','a'

		将字符显示转换为Int数字：
		fun decimalDigitValue(c: Char): Int {
	    if (c !in '0'..'9')
	        throw IllegalArgumentException("Out of range")
	    return c.toInt() - '0'.toInt() // 显式转换为数字
		}
	
- 布尔 用的是Boolean类型表示，有两个值：true,false

		|| – 短路逻辑或
		&& – 短路逻辑与
		! - 逻辑非

- 数值：用类Array实现，并且有一个size属性及get,set方法。Kotlin使用了[]重载了get和set方法，所以可以通过下表很方便的获取或者设置数组对应位置的值。

		数组的创建两种方式：一种是使用函数arrayOf()；另外一种是使用工厂函数。

		fun main(args: Array<String>) {
	    //[1,2,3]
	    val a = arrayOf(1, 2, 3)
	    //[0,2,4]
	    val b = Array(3, { i -> (i * 2) })
	
	    //读取数组内容
	    println(a[0])    // 输出结果：1
	    println(b[1])    // 输出结果：2
		}
- 字符串：String和Java一样是不可变的，[]可以很方便的获取字符串中的某个字符，也可以通过for循环来遍历。

		for (c in str) {
	    println(c)
		}

		String 可以通过 trimMargin() 方法来删除多余的空白。
		fun main(args: Array<String>) {
	    val text = """
	    |多行字符串
	    |菜鸟教程
	    |多行字符串
	    |Runoob
	    """.trimMargin()
	    println(text)    // 前置空格删除了
		}
- 字符串模板：字符串模板可以包含模板表达式，即一小段代码。

		fun main(args: Array<String>) {
		    val i = 10
		    val s = "i = $i" // 求值结果为 "i = 10"
		    println(s)
		}

		fun main(args: Array<String>) {
		    val s = "runoob"
		    val str = "$s.length is ${s.length}" // 求值结果为 "runoob.length is 6"
		    println(str)
		}

##三、Kotlin条件控制语句
- if的特殊用法：
 
	 	①不需要像Java那种有一个三元操作符，因为我们可以使用它来简单实现：
		val c = if (condition) a else b
		// 作为表达式
		val max = if (a > b) a else b

		②使用in运算符来检测某个数字是否在指定的区域内：
			fun main(args: Array<String>) {
			    val x = 5
			    val y = 9
			    if (x in 1..8) {
			        println("x 在区间内")
			    }
			}

-When表达式
		When将他的参数和所有分支条件进行顺序比较，直到某个分支满足条件。
		When类似其他语言的switch操作符：
		when (x) {
		    1 -> print("x == 1")
		    2 -> print("x == 2")
		    else -> { // 注意这个块
		        print("x 不是 1 ，也不是 2")
		    }
		}

		在 when 中，else 同 switch 的 default。如果其他分支都不满足条件将会求值 else 分支。
		如果很多分支需要用相同的方式处理，则可以把多个分支条件放在一起，用逗号分隔：

		when (x) {
		    0, 1 -> print("x == 0 or x == 1")
		    else -> print("otherwise")
		}

##四、Kotlin循环控制语句
- for循环：可以对任何提供迭代器（iterator）的对象进行遍历，语法如下:

		for (item in collection) print(item)

		如果你想要通过索引遍历一个数组或者一个 list，你可以这么做：
		for (i in array.indices) {
		    print(array[i])
		}

		注意这种"在区间上遍历"会编译成优化的实现而不会创建额外对象。
		或者你可以用库函数 withIndex：
		for ((index, value) in array.withIndex()) {
		    println("the element at $index is $value")
		}

- while循环，do...While：

		while( 布尔表达式 ) {
			  //循环内容
			}

- 返回和跳转
		return,break,continue

		fun main(args: Array<String>) {
		    for (i in 1..10) {
		        if (i==3) continue  // i 为 3 时跳过当前循环，继续下一次循环
		        println(i)
		        if (i>5) break   // i 为 6 时 跳出循环
		    }
		}
		输出结果：   1
					2
					4
					5
					6

		可以使用标签来跳出当前循环：
		loop@ for (i in 1..100) {
		    for (j in 1..100) {
		        if (……) break@loop
		    }
		}

##五、Kotlin类和对象
- 类定义：Kotlin类可以包含构造函数，初始化代码块，函数，属性，内部类，对象声明。

		class Runoob() {
		    fun foo() { print("Foo") } // 成员函数
		}

- 构造器：

	1、Koltin 中的类可以有一个 主构造器，以及一个或多个次构造器，主构造器是类头部的一部分，位于类名称之后:class Person constructor(firstName: String) {}，如果主构造器没有任何注解，也没有任何可见度修饰符，那么constructor关键字可以省略。

	2、Kotlin 中类不能有字段。提供了 Backing Fields(后端变量) 机制,备用字段使用field关键字声明,field 关键词只能用于属性的访问器，如以上实例：

		var no: Int = 100
	        get() = field                // 后端变量
	        set(value) {
	            if (value < 10) {       // 如果传入的值小于 10 返回该值
	                field = value
	            } else {
	                field = -1         // 如果传入的值大于等于 10 返回 -1
	            }
	        }

	3、非空属性必须在定义的时候初始化,kotlin提供了一种可以延迟初始化的方案,使用 lateinit 关键字描述属性：
		public class MyTest {
		    lateinit var subject: TestSubject
		
		    @SetUp fun setup() {
		        subject = TestSubject()
		    }
		
		    @Test fun test() {
		        subject.method()  // dereference directly
		    }
		}

	4、主构造器：不能包含任何代码，初始化代码可以放在初始化代码段中，初始化代码段使用 init 关键字作为前缀

	class Person constructor(firstName: String) {
	    init {
	        println("FirstName is $firstName")
	    }
	}