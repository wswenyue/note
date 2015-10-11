#Service
主要内容：
- BindService 绑定服务
- IntentService 带有子线程的服务组件，其内部使用了单线程池模式
- AIDL 进程间（应用间）通信【方法调用、接口】
- 粘性Service【流氓服务，关不掉的服务】

##BindService 绑定服务
BindService用于短生命周期的服务，BindService和Activity进行绑定后，其生命周期和Activity的生命周期一致，当Activity销毁了BindService也就销毁了。

###bindService方式启动服务组件的生命周期方法

	onCreate()
	IBinder onBind(Intent)//返回实例化Binder类的子类
	onUnbind()
	onDestroy()

###bindService的过程
- **在Service组件中，声明Binder类的子类（公共），在其子类中声明组件之间的接口方法**

- **在onBind()方法中，实例化Binder子类的对象，并返回**


- ** 在绑定服务组件端（Activity），实例化ServiceConnection接口对象，并在此接口的onServiceConnected()方法中，将方法的第二个参数IBinder对象强转成Binder子类对象**


- **在适当的位置 调用 Context.bindService()来绑定服务。**

示例（一个管理定时器的服务组件）：

	public class TimerService extends Service {
	
		private Timer timer; //定时器
		
		private NotificationManager notifyMgr;
		@Override
		public void onCreate() {
			super.onCreate();
			Log.i("debug", "--onCreate--");
			
			timer=new Timer();
			
			notifyMgr=(NotificationManager) getSystemService(NOTIFICATION_SERVICE);
		}
		
		@Override
		public IBinder onBind(Intent intent) {
			Log.i("debug", "--onBind--");
			return new TimerBinder();//2. 实例化Binder的子类，并返回
		}
		
		@Override
		public boolean onUnbind(Intent intent) {
			Log.i("debug", "--onUnbind--");
			return super.onUnbind(intent);
		}
		
		@Override
		public void onDestroy() {
			Log.i("debug", "--onDestroy--");
			super.onDestroy();
		}
		
		//1. 声明Binder类的子类，用于Service与绑定的Activity的组件进行通信
		public class TimerBinder extends Binder{
			public void start(){
				Log.i("debug", "----start---");
				
				//通过定时器，来安排时间计划
				timer.schedule(new TimerTask(){
					@Override
					public void run() {
						// TODO 在指定的时间执行的任务
						
						NotificationCompat.Builder builder=
								new NotificationCompat.Builder(getApplicationContext());
						builder.setSmallIcon(android.R.drawable.ic_menu_today)
								.setContentTitle("提醒")
								.setContentText("时间了，该起床了....")
								.setTicker("时间了，该起床了....")
								.setDefaults(Notification.DEFAULT_SOUND)
								.setOngoing(true);
								
						notifyMgr.notify(2, builder.build());
						
					}
				},10*1000, 5*1000);
				
			}
			
			public void stop(){
				Log.i("debug", "----stop---");
				//关闭所有的定时任务
				timer.cancel();
				
				notifyMgr.cancel(2);
			}
		}
	
	}


绑定服务组件端（Activity），实例化ServiceConnection接口对象
在ServiceConnection接口对象的onServiceConnected()方法中，将方法的第二个参数IBinder对象强转成Binder子类对象

在Activity中绑定服务示例：
	
	public class MainActivity extends Activity {
	
		//声明Binder子类对象
		TimerBinder timerBinder;
		
		//3. 实例化ServiceConnection接口（绑定服务组件时使用的回调接口）
		ServiceConnection conn=new ServiceConnection(){
			@Override
			public void onServiceConnected(ComponentName name, IBinder service) {
				// TODO 绑定成功的方法
				timerBinder=(TimerBinder) service;
			}
			
			@Override
			public void onServiceDisconnected(ComponentName name) {
				// TODO 与绑定服务组件断开连接（发生的时机：由于系统原因造成了服务组件停止或销毁）
				timerBinder=null;
			}
		};
		
		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);
		}
	
		public void bindService(View v) {
			//绑定服务组件
			bindService(new Intent(getApplicationContext(),TimerService.class),
					conn, BIND_AUTO_CREATE);
			
			//BIND_AUTO_CREATE标识表示：绑定的服务组件如果不存，则会自动创建，
			//注：由bindService方式启动的Service，其生命周期会受到绑定组件的影响，即当绑定组件Activity销毁时，Service也会停止 
		}
	
		public void unbindService(View v) {
			unbindService(conn); //解除绑定
		}
	
		public void startTime(View v) {
			if(timerBinder!=null){
				timerBinder.start();
			}
		}
	
		public void stopTime(View v) {
			if(timerBinder!=null){
				timerBinder.stop();
			}
		}
	
	}

###绑定类型

绑定服务的类型：
	
	BIND_AUTO_CREATE
	绑定的service不存在时，会自动创建
	BIND_ADJUST_WITH_ACTIVITY
	service的优先级别与根据所绑定的Activity的重要程度有关，Activity处于前台，service的级别高
	BIND_NOT_FOREGROUND
	Service永远不拥有运行前台的优先级
	BIND_WAIVE_PRIORITY
	Service的优先级不会改变
	BIND_IMPORTANT | BIND_ABOVE_CLIENT
	所绑定的Activity处于前台时，Service也处于前台
	BIND_ABOVE_CLIENT 指定内存很低的情况下，运行时在终止绑定的Service之前终止Activity

##IntentService

**IntentService是带有子线程的服务组件，其内部使用了单线程池模式，当所有的任务执行完成后，会自动关闭本服务（如果没有任务，大概2秒左右关闭）。**

###生命周期方法

    onCreate()	//该方法只在创建IntentService时执行，只执行一次
	onStartCommand()	//每次启动都执行
	onHandleIntent() 	// 在子线程中执行的方法,所有的任务都按照顺序依次执行
    onDestroy()

一个执行下载任务的示例：

	public class DownloadService extends IntentService {
		public DownloadService(){
			super(null);//参数：是设置子线程的名称
		}
		
		@Override
		public void onCreate() {
			super.onCreate();
			Log.i("debug", "--onCreate---");
		}
		@Override
		public int onStartCommand(Intent intent, int flags, int startId) {
			Log.i("debug", "--onStartCommand---");
			return super.onStartCommand(intent, flags, startId);
		}
		
		@Override
		protected void onHandleIntent(Intent intent) {
			// TODO 在子线程中执行的方法
			Log.i("debug", "--onHandleIntent---");
			//获取下载图片的地址
			String path=intent.getStringExtra("path");
			try{
				URL url=new URL(path);
				HttpURLConnection conn=(HttpURLConnection) url.openConnection();
				InputStream is=conn.getInputStream();
				byte[] buffer=new byte[10*1024];//每次读取字节的最大数
				int len=-1;
				
				ByteArrayOutputStream baos=new ByteArrayOutputStream();
				if(conn.getResponseCode()==200){
					while((len=is.read(buffer))!=-1){
						baos.write(buffer, 0, len);
					}
					
					byte[] bytes=baos.toByteArray();
					//将下载完成的字节数组转成图片对象
					Bitmap bitmap=BitmapFactory.decodeByteArray(bytes, 0, bytes.length);
					
					//将图片对象发送给Activity进行显示
					Intent bitmapIntent=new Intent(Config.ACTION_DOWNLOAD_COMPLETED);
					bitmapIntent.putExtra(Config.EXTRA_BITMAP,bitmap);
					bitmapIntent.putExtra(Config.EXTRA_URL, path);
					
					sendBroadcast(bitmapIntent);
					
				}
				
			}catch(Exception e){
				e.printStackTrace();
			}
			
		}
		
		@Override
		public void onDestroy() {
			super.onDestroy();
			Log.i("debug", "--onDestroy---");
		}
	}

Activity端调用下载服务【广播接收之类省略】：

	public void startDownload(View view){
		Intent intent=new Intent(getApplicationContext(),DownloadService.class);
		intent.putExtra("path", Config.URL1);
		startService(intent); //下载第一个图片
		
		intent.putExtra("path", Config.URL2);
		startService(intent); //下载第二个图片
		
		intent.putExtra("path", Config.URL3);
		startService(intent); //下载第三个图片
		
	}


##AIDL实现跨进程服务

###AIDL用于实现进程之间通信[调用服务]的接口

###使用方法及步骤：
#### 服务端

**1. 声明.aidl文件，在此文件中声明进程间的通信接口（类似于Binder子类的作用）**

示例：

```java
package com.wenyue.aidl;

interface IRemoteService{

	void print(String msg);
	
	String getName();
}
```

**2.在Service组件的onBind()方法中，返回aidl接口的Stub对象**
示例：

```java							
public class RemoteSerice extends Service {

	//2.1 实例化.aidl接口的Stub对象
	private IRemoteService.Stub stub=new IRemoteService.Stub() {
		
		@Override
		public void print(String msg) throws RemoteException {
			Log.i("debug", "--RemoteSerice---"+msg);
		}
		
		@Override
		public String getName() throws RemoteException {
			return "RemoteSerice";
		}
	};
	
	@Override
	public IBinder onBind(Intent intent) {
		return stub;//2.2 返回aidl接口的Stub对象
	}
}	
	
```

**3. 注册Service组件是，必须要提供一个启动或绑定此组件的Action**

	 <!-- 注册Service组件，注：此组件可被其应用访问，因此必须要提供一个Action -->
    <service android:name="com.qf.serivce04_aidl_server.RemoteSerice">
        <intent-filter>
            <action android:name="com.qf.serivce04_aidl_server.RemoteSerice"/>
        </intent-filter>
    </service>


####客户端

**1.将服务端提供的.aidl文件复制到src下**

**2.在Activity类中，实例化ServiceConnection接口对象，实现两个回调方法，在onServiceConnected()方法中将第二个参数转化.aidl接口对象**

**3.在适当的位置调用Context.bindService()方法，进行绑定。此方法的第一个参数：Intent意图，在实例化时，需要指定Service组件的Action**

客户端使用示例：

	public class MainActivity extends Activity {
	
		//声明aild接口对象
		IRemoteService remoteService;
		
		//实例化绑定组件之间的通信接口
		ServiceConnection conn=new ServiceConnection() {
			
			@Override
			public void onServiceDisconnected(ComponentName name) {
				// TODO Auto-generated method stub
				
			}
			
			@Override
			public void onServiceConnected(ComponentName name, IBinder service) {
				//将第二个参数转化为.aidl接口对象
				remoteService=IRemoteService.Stub.asInterface(service);
			}
		};
		
		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);
			
			//绑定其它应用的Service组件，在绑定成功之后，将返回的Stub对象转化为aidl接口对象
			bindService(new Intent("com.qf.serivce04_aidl_server.RemoteSerice"),
					conn, BIND_AUTO_CREATE);
			
		}
	
		
		public void getRemoteName(View v) throws RemoteException{
			if(remoteService!=null){
				setTitle(remoteService.getName());
			}
		}
	
		
		public void print(View v) throws RemoteException{
			if(remoteService!=null){
				remoteService.print("hello,service!!!");
			}
		}
	}

##粘性Service
和普通的Service一样只是在onStartCommand方法中返回Service.START_STICKY **粘性Service标识**即可
	
	@Override
	public int onStartCommand(Intent intent, int flags, int startId) {
		return Service.START_STICKY;//粘性Service标识，当Service组件在非意愿时被停止后，有机率重启
	}

###关不掉的服务
思想：开启一个线程在线程run方法里，死循环执行查看服务是否在运行，如果服务死掉，重启服务。

	//查找当前组件的线程，如果当前正运行的服务组件不包含PushService组件时，则启动
	class SeekServiceThread extends Thread{
		
		@Override
		public void run() {
			
			while(true){
				
				//1.获取Activity组件管理器（管理当前应用的进程、服务组件、任务或回退栈）
				ActivityManager mgr=(ActivityManager) getSystemService(ACTIVITY_SERVICE);
				
				//2. 获取正运行的服务组件
				List<RunningServiceInfo> rServices = mgr.getRunningServices(100);
				
				boolean isFinded=false;//标识是否查找到当前的service组件
				for(RunningServiceInfo rService:rServices){
					if(rService.getClass().getName().equals(PushService.class.getName())){
						isFinded=true;
					}
				}
				
				if(!isFinded){ //如果没有查找到，则启动本服务组件
					
					startService(new Intent(getApplicationContext(),PushService.class));
				}
				
				try {
					Thread.sleep(2000);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
			
		}
	}


**注：当然最好的做法是开启两个服务互相查找，如果一方死了，另一方则重启它，利用之间的时间差就行**