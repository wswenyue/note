#Broadcast 广播 四大组件之一
##接受广播BroadcastReceiver:

###使用三要素：频道(Action)、权限、数据字段

###定义：
广播接收器，即接收通过某一频道(Action)发送的广播，发送者可以是Activity和Service

###作用：
- 监听系统的广播，并做相应的处理，如电量过低时需要保存相关数据，或发出通知警告用户
- 后台运行的服务，如经过耗时操作后，获取了网络数据，通过广播的方式通知用户

###分类：
####系统广播
常见的系统广播：**注意：系统广播要严格按照官方API的说明方式使用**

	Intent.ACTION_BOOT_COMPLETED 系统开机启动完成
	Intent.ACTION_SHUTDOWN 关机提醒广播
	Intent.ACTION_BATTERY_LOW 低电量提醒广播
	Intent.ACTION_SCREEN_OFF 屏幕被关闭
	Intent.ACTION_SCREEN_ON 屏幕已经被打开
	Intent.ACTION_USER_PRESENT 屏幕解锁广播
	Intent.ACTION_NEW_OUTGOING_CALL 拨号广播
	TelephonyManager.ACTION_PHONE_STATE_CHANGED 来电时电话状态变化广播，如响玲、空置、挂断
	Telephony.Sms.Intents.SMS_RECEIVED_ACTION 接收短信的广播
	
`Intent.ACTION_BATTERY_CHANGED 电量发生变化广播` **这个仅支持代码中注册**

###用法及示例
1. 创建类，继承`BroadcastReceiver`类
2. 重写`onReceiver(Context,Intent)`方法
3. 注册广播（根据需要使用以下两种方式之一就行）
	1. 在`AndroidManifest.xml`中注册（全局）
	2. 动态注册（局部）：`registerReceiver(BroadcastReceiver,IntentFilter)`
4. 发送广播(依需任性其一)
	1. `Context.sendBroadcast(Intent)`
	2. `Context.sendBroadcast(Intent,String)` 带有权限发送广播

示例：**开机启动服务**

	第一步：
	public class BootBroadcastReceiver extends BroadcastReceiver {
	   @Override
	    public void onReceive(Context context, Intent intent) {
			//这里自定义启动你想要启动的服务，获取其他
	       Intent service = new Intent(context, MyService.class);
	       context.startService(service);
	       Log.d("DUBAG", "onReceive 接受到系统开机的广播");
	    }
	}

	第二步：配置xml文件，在receiver接收这种添加intent-filter配置  
     <receiver android:name="BootBroadcastReceiver">  
                <intent-filter>  
                    <action android:name="android.intent.action.BOOT_COMPLETED"></action>  
                    <category android:name="android.intent.category.LAUNCHER" />  
                </intent-filter>  
            </receiver>  
	第三步：添加权限 <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" /> 

###广播的生命周期
**生命周期方法：**
	
	public void onCreate() {}；//只执行一次，用于初始化Service
	public int onStartCommand(Intent intent, int flags, int startId){}；//每次启动Service都会执行的方法，在此实现核心的功能
	public void onDestroy() {}；//只执行一次，用于销毁Service组件
	public IBinder onBind(Intent intent) {}；

**注意：**

- 广播接收者的生命周期是非常短暂的，在接收到广播的时候创建，`onReceive()`方法结束之后销毁
- 广播接收者中不要做一些耗时的工作，否则会弹出`Application Not Response`错误对话框 
- 最好也不要在广播接收者中创建子线程做耗时的工作，因为广播接收者被销毁后进程就成为了空进程，很容易被系统杀掉
- 耗时的较长的工作最好放在服务中完成

###有序广播

有序广播：是通过Context.sendOrderedBroadcast来发送，所有的receiver依次执行，**有序广播被中断后就不在发送，其他未接受到的接受者将不会接受到**

	Context.sendOrderedBroadcast(Intent,String)//发送有序广播
	BroadcastReceiver.abortBroadcast() 中断广播，使级别低的广播接收器不能接收此广播
	声明广播接收级别：android:priority="1000"

###粘性广播

sendStickyBroadcast() 发送粘性的广播
android.Manifest.permission.BROADCAST_STICKY 声明权限

特点

- Intent会一直保留到广播事件结束，没有所谓的10秒限制
- 普通的广播如果onReceive方法执行时间太长，超过10秒的时候系统会将这个广播置为可以干掉的candidate，一旦系统资源不够的时候，就会干掉这个广播而让它不执行

###广播的安全性
发送广播时：
1. 发送带权限的广播
	- `sendBroadcast(Intent,String)`
2. 指定接收广播的应用包名
	- `Intent.setPackage("com.wswenyue.xxx")`
	
在AndroidManifest.xml清单中配置 

    <permisson android:name="" /> 定义权限
    <uses-permission android:name=".."> 使用权限

注册接收广播时：

    Context.registBroadcast(Intent,String),String为接收广播的权限<receiver androd:exported="false" ..>不接收外部应用的广播 


###本地广播LocalBroadcastManager：
**如果是在本应用中使用，官方推荐使用本地广播，因为它具有较高的安全性，和普通广播相比有较高的性能及优化**

	所在包：android.support.v4.content
	//获取本地广播管理器
	LocalBroadcastManager manager = LocalBroadcastManager.getInstance(Context)
	//注册本地广播接受者
	manager.registReceiver(BroadcastReceiver,IntentFilter)
	//发送本地广播
	manager.sendBroadcast(Intent)
	//异步发送本地广播
	manager.sendBroadcastSync(Intent intent)
	//注销本地广播
	manager.unregisterReceiver(BroadcastReceiver receiver)


##广播发送很简单

	 Intent serviceIntent = new Intent();
	//指定频道Action
     serviceIntent.setAction("wenyue.com.service");
    //如果有数据，给Intent加上数据
	//发送广播
	 sendBroadcast(serviceIntent)


