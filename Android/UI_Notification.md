##通知
###示例：
示例1：

		//创建通知 的构建器对象
		NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
		
		//设置通知的相关属性
		builder.setContentTitle("通知");
		
		builder.setContentText("下雨了");
		
		//必须设置，否则程序会崩溃
		builder.setSmallIcon(R.drawable.ic_launcher);
		
		//得到通知管理对象---属于系统服务
		NotificationManager manager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
		
		//得到通知对象
		Notification notification = builder.build();
		
		//使用通知管理对象发送通知
		manager.notify((int)System.currentTimeMillis(), notification);//如果id相同，即使发送多个通知，也只显示一个

示例2：

    NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
	builder.setContentTitle("你有短信");
	builder.setContentText("你有3条短信");
	builder.setSmallIcon(R.drawable.ic_launcher);
	builder.setNumber(3);//设置数量
	//收到通知可以 是声音，灯光，震动
	//DEFAULT_VIBRATE  需要权限
	builder.setDefaults(Notification.DEFAULT_VIBRATE);
	
	Notification notification = builder.build();
	
	NotificationManager manager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
	
	manager.notify(168, notification);

示例3：

	NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
	builder.setContentTitle("下载");
	builder.setContentText("正在下载");
	builder.setSmallIcon(R.drawable.ic_launcher);
	builder.setOngoing(true);//设置不可以被删除
	
	
	Notification n = builder.build();
	
	NotificationManager manager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
	
	manager.notify(188, n);

示例4：

	NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
		
	builder.setContentTitle("你有短信")
	       .setContentText("你有6条短信")
	       .setSmallIcon(R.drawable.ic_launcher)
	       .setNumber(6)
	       .setDefaults(Notification.DEFAULT_ALL);
	
	Intent intent = new Intent(this,MainActivity.class);
	
	//可以当通知被点击时自动执行 startActiivty()
	PendingIntent pending = PendingIntent.getActivity(this, 6, intent, PendingIntent.FLAG_CANCEL_CURRENT);
	
	builder.setContentIntent(pending);
	
	builder.setAutoCancel(true);//设置通知自动消失
	
	Notification ntf = builder.build();
	
	NotificationManager manager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
	
	manager.notify(88, ntf);

##Toast自定义

	Toast toast = new Toast(this);
		
		View view = getLayoutInflater().inflate(R.layout.toast_layout, null);
		
		toast.setView(view);//设置吐司使用的布局
		
		toast.setGravity(Gravity.CENTER, 0,0);//设置吐司的显示位置
		
		toast.setDuration(Toast.LENGTH_SHORT);
		
		toast.show();	