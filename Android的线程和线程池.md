## 一、概念 ##
- 线程分为主线程（UI线程）和子线程，主线程主要处理和界面相关的事情，子线程则往往用于执行耗时操作。
- asyncTask封装了线程池和Handler,主要是为了方便开发者再子线程更新UI。
- HandlerThread:是一种具有消息循环的线程，内部使用的是Handler.
- IntentService:是一个服务，系统对于其执行了封装，使其可以更方便的执行后台任务，IntentService内部采用HandlerThread的方式执行任务，当任务执行完毕之后，IntentService可以自动退出。
- 线程是操作系统调度的最小单元，同时线程又是一种受限的系统资源，即线程不能不可能无限制的产生，并且线程的创建和销毁都会有相应的开销。

## 二、Android中的线程形态 ##
## AsyncTask: ##
- AsyncTask:一种轻量级的异步任务类，可以在线程池中执行后台任务，然后把执行的进度和最终的结果传递给主线程并更新UI。但是AsyncTask并不建议做特别耗时的后台任务，对于特别耗时的操作建议使用线程池。
- AsyncTask是一个抽象类，其提供了4个核心的方法：

		①onPreExrcute():在主线程中执行，在异步任务执行之前，主要是用于做一些准备性的工作。
		②doInBackground(Params...params):在线程池中执行，主要用于执行异步任务，在此方法中可以通过publishProgress来更新任务的进度，publishProgress方法会调用onProgressUpdate,另外此方法需要计算结果给onPostExecute方法。
		③onProgressUpdate(Progress...values):在主线程中执行，当后台任务的执行进度发生改变的时候，此方法会被调用。
		④onPostExecute(Result result):在主线程中执行，在异步执行之后，result参数是后台任务的返回值，即doInbackground的返回值。
- AsyncTask还提供onCancelled()方法，用来取消任务，当这个方法被调用的时候，onPostExecute不会被调用。
- 在Android1.6之前是串行执行任务的，但是1.6的时候开始并行处理任务，但是3.0的时候，为了避免并行处理任务的的并发错误，开始串行的处理任务，但是3.0以后的版本默认是串行的，但是仍然可以AsyncTask的executeOnExecutor()方法来并行执行任务。

## HandlerThread: ##
- 实现方式就是从在run方法中通过Looper.prepared()来创建消息队列，并通过Looper.loop来开启消息循环。
- HandlerThrad的run方法是一个无限的循环，因此当明确不需要再使用HandlerThread的时候，可以通过他的quit或者quitSafely方法来终止线程的执行。

## IntentService ##
- IntentService可用于执行后台耗时的任务，当任务执行后它会自动停止，同时由于IntentService是服务的原因，这导致它的优先级比单纯的优先级高，不容易被系统杀死。
- IntentService封装了HandlerThread和Handler.
- 当onHandleIntent方法处理完成的时候，IntentService会通过stopSelf(int startId)的方法来停止服务，stopSelf(int id)会等待所有的消息都处理完毕的时候才会终止服务。（而不是用的stopSelf():会立刻停止服务，而这个时候可能还有其他的消息未处理）
- IntentService是顺序执行后台任务的，当有多个后台任务的同时存在，这些后台任务就会按照外界发起的顺序排队执行。（串行执行的）

## Android中的线程池 ##
- 重用线程池中的线程，避免因为线程的创建和销毁所带来的性能开销。
- 能有效的控制线程池的最大并发数，避免大量的线程之间因为互相抢占系统资源而导致的阻塞现象。
- 能够对线程的进行简单的管理，并提供定时执行以及指定间隔循环执行等功能。

## ThreadPoolExecutor ##
- ThreadPoolEecutor提供了一系列的参数来配置线程池。
- corePoolSize:核心线程数，一般核心线程会一直存活。
- keepAliveTime:非核心线程闲置时的超时时长，超过这个时长，非核心线程就会被回收。
- 线程池的分类：
 
		fixedThreadPool:只有核心线程，并且这些核心线程不会被没收，并且核心线程没有超时机制。线程数量固定。（execute)
		CachedThreadPool:线程数量不固定的线程池，只有非核心线程即空闲线程，超时时机为60s,超过就会被回收。(execute)
		ScheduledThreadPool:核心线程数量是固定的，但是非核心的线程数量没有限制。(schedule)
		SingleThreadExecutor:只有一个核心线程。(execute)
