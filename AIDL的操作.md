## 对于AIDL的使用： ##
- 首先在Project的main的视图下新建一个aidl,注意AIDL的满足的类型可以是基本类型，也可以是自定义的序列化的实体类型。（如果是自己定义的实体类型，则需要导入完整的实体类型路径）
 
		import com.example.yangxiaoyu.ipctest.Book;
		interface IBookManager {
   			List<Book> getBookList();
   			void addBook(in Book book);
			}

- Make Project,系统会在generated中自动生成一个Java文件的接口，在其中是存在一些Binder的方法。
- 在service中运用这个生成的接口文件去创建一个Binder对象，在其中去实现里面的add,get方法。（注意在AndroidManifest中去注册）
- 在Activity中通过BindService的方式去启动这个Service.
 
	bindService(intent,connection, Context.BIND_AUTO_CREATE);
	其中的connection是通过ServiceConnection去构建的一个对象，在onServiceConnected的时候去做一些操作。
