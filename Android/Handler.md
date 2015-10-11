#Handler
##总结
###handler负责发送消息，Looper负责接收Handler发送的消息，并直接把消息回传给Handler自己
###MessageQueue就是一个存储消息的容器

##handler是什么？
- Android中更新UI的一套机制
- 消息处理机制
	- 发送，处理消息

> 所有的Activity的生命周期的回调方法都是通过Handler机制发送相应消息，根据不同的Message做出不同的分支处理。
> AMS（ActivityManageService）发送消息给ActivityThread，然后由ActivityThread对相应消息做出相应方法的回调。

##为什么要使用Handler，不使用行吗？
Android在设计的时候就封装了这样一套消息创建，传递，处理机制，如果不遵循这样的规则就没有办法更新UI。就会抛出异常

###最根本的原因：为了解决多线程并发的问题
假如在一个Activity中，有多个线程去更新UI，并且都没有加锁机制，就会产生：

**更新界面错乱**

如果加锁：

**性能下降**

基于以上问题的处理：Android给我们提供了Handler机制，我们只要遵循这样的机制去更新UI就可以，而不用考虑上面的问题。


##Handler的使用
- sendMessage
- sendMessageDelayed
- post(Runnable)
- postDelayed(Runnable)

##消息机制处理的原理：

###一、Handler：封装了消息的发送（主要包括消息发送给谁）
###二、Looper
- 内部包含了一个MessageQueue(消息队列)，所有的Handler发送的消息都走向这个消息队列
- Looper.looper方法，就是一个死循环，它不断的从MessageQueue中取消息，如果有消息就处理消息，没有消息就阻塞。

###三、MessageQueue 就是一个消息队列，可以添加消息，并处理消息

###四、Handler，内部会跟Looper进行关联，Handler内部获取到Looper后也就找到了MessageQueue，Handler中发送消息其实就是向这个MessageQueue队列中发送消息

###总结：
- handler负责发送消息，Looper负责接收Handler发送的消息，并直接把消息回传给Handler自己
- MessageQueue就是一个存储消息的容器

##如何实现一个与线程相关的Handler
	Looper.prepare();  
                  
    handler = new Handler()  
    {  
        public void handleMessage(android.os.Message msg)  
        {  
           //TODO   
        };  
    };
	
	Looper.loop();

##更新UI的几种方式：
- runOnUiThread
- handler post
- handler sendMessage
- view post

##非Ui线程真的不能更新UI吗
