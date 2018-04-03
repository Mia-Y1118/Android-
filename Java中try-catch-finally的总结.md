## try-catch-finally ##
一、在try语句的return块中，return返回的引用变量（t是引用类型）并不是try语句外定义的引用变量t，而是系统重新定义的一个局部引用t,这个引用指向了引用t对应的值，也就是try,即使在finally语句中把引用t指向了值finally，因为return的返回引用已经不是t ，所以引用t的对应的值和try语句中的返回值无关了。

	public class TryCatchFinally {
		   
		@SuppressWarnings("finally")
		   public static final String test() {
		       String t = "";
		       try {
		           t = "try";
		           return t;
		       } catch (Exception e) {
		           t = "catch";
		           return t;
		       } finally {
		           t = "finally";
		       }
		   }
		
		   public static void main(String[] args) {
		       System.out.print(TryCatchFinally.test());
		   }
		
		}

二、当try和finally中均有return语句的时候，直接起作用的是finally中的return语句。

		public class TryCatchFinally {
		 
		     @SuppressWarnings("finally")
		     public static final String test() {
		         String t = "";
		 
		         try {
		             t = "try";
		             return t;
		         } catch (Exception e) {
		             // result = "catch";
		             t = "catch";
		             return t;
		         } finally {
		             t = "finally";
		             return t;
		         }
		     }
		 
		     public static void main(String[] args) {
		         System.out.print(TryCatchFinally.test());
		     }
		 
		 }
三、当catch中有返回语句的时候，其表现形式和try的返回语句相似。如果finally中有return语句，则最终需要执行finally的return语句。

四、注意查看catch中捕获的异常是不是try中的异常，如果catch的确实是这个异常，则才进入catch.

五、所有总结：

	1 try、catch、finally语句中，在如果try语句有return语句，则返回的之后当前try中变量此时对应的值，此后对变量做任何的修改，都不影响try中return的返回值
	
	2 如果finally块中有return 语句，则返回try或catch中的返回语句忽略。
	
	3 如果finally块中抛出异常，则整个try、catch、finally块中抛出异常


    注意事项：
	1 尽量在try或者catch中使用return语句。通过finally块中达到对try或者catch返回值修改是不可行的。
	
	2 finally块中避免使用return语句，因为finally块中如果使用return语句，会显示的消化掉try、catch块中的异常信息，屏蔽了错误的发生
	
	3 finally块中避免再次抛出异常，否则整个包含try语句块的方法回抛出异常，并且会消化掉try、catch块中的异常