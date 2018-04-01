##  一、概念： ## 
- android消息机制主要指Handler的运行机制，Handler的运行需要与底层的MessageQueue和Looper的支撑。
- messageQueue:内部的存储结构并不是真正的队列，而是采用单链表的数据结构来存储消息的列表。
- Handler创建的时候，会采用当前线程的Looper来构造消息循环系统。
- Handler的主要作用是解决在子线程中无法访问UI的矛盾。

## MessageQueue  ## 
- MessageQueue主要包含两个操作：插入（enqueueMessage)和删除（next）
- 插入：往消息队列中插入一条信息
- 删除:从消息队列中取出一条消息并将其从消息队列中移除

## Looper的工作原理 ##
- 如何为一个线程创建Looper?通过Looper.prepare()就可为当前的线程创建一个looper,接着通过Looper.loop来开启消息循环。
- loop的内部原理是一个死循环，通过for(;;)的形式，
- Android的主线程就是ActivityThread,主线程的入口方法为main,在main中会通过Looper.prepareMainLooper()来创建主线程的looper以及MessageQueue,并通过Looper.loop()来开启主线程的消息循环。