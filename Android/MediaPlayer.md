#MediaPlayer
使用MediaPlayer播放音频、视频文件要遵循Google官方给出的MediaPlayer状态图，来调用。
##状态图
状态图定义了在某个状态以该干什么，或者能做什么，如果不按照状态图操作，会报出状态调用异常

![mediaplayer_state_diagram](http://7xj2yt.com1.z0.glb.clouddn.com/android_mediaplayer_state_diagram.gif)

--------

##方法及使用
###创建MediaPlayer可以使用静态方法create
    
    static MediaPlayer create(Context context, int resid)
    static MediaPlayer create(Context context, Uri uri)

使用该方法创建成功后就进入了`prepare`的状态

###常用方法

    setDataSource(String path) 设置媒体源
    setLooping(boolean) 设置是否循环播放
    prepare() 预处理，调用start()前必须先调用此方法
    start() 开始播放
    pause()暂停播放
    stop() 停止播放
    isPlaying() 是否正在播放
    reset() 重置播放器，进入idle空闲状态
    release() 释放资源
    getDuration() 获取播放文件的总时长，单位：毫秒
    getCurrentPosition() 获取当前播放的位置
    seekTo(int position) 定位到指定的播放时间

使用示例：    

	/**
	 *MediaPlayer属于长生命周期的类，所以一般使用Service管理：
	 * 用于管理MediaPlayer的服务组件
	 */
	public class PlayerService extends Service {
	
		private MediaPlayer mPlayer; // 媒体文件的播放组件
	
		private int sumLen; // 总时长
	
		private LocalBroadcastManager lbMgr;// 本地广播管理器
	
		private SeektoReceiver seekReceiver;
		@Override
		public void onCreate() {
			// 初始化Service组件
			super.onCreate();
	
			// 实例化MediaPlayer
			mPlayer = MediaPlayer.create(getApplicationContext(), R.raw.ghsy);
	
			// 获取本地广播管理器对象
			lbMgr = LocalBroadcastManager.getInstance(getApplicationContext());
			
			seekReceiver=new SeektoReceiver();
			lbMgr.registerReceiver(seekReceiver, new IntentFilter(Config.ACTION_SEEKTO));
			
		}
	
		@Override
		public int onStartCommand(Intent intent, int flags, int startId) {
			//判断当前意图是否改变播放新的音频文件
			if(intent.getBooleanExtra(Config.EXTRA_CHANGE, false)){
				mPlayer.reset();//重置播放器，进入idle空闲状态
				try {
					//设置新的数据源（新的音频文件）,进入initial初始化状态
					mPlayer.setDataSource(intent.getStringExtra(Config.EXTRA_PATH));
					
					mPlayer.prepare();//进入就绪状态（Prepared）
					
				}  catch (Exception e) {
					e.printStackTrace();
				}
			}
			
			
			// 判断当前的MediaPlayer是否正在播放
			if (mPlayer.isPlaying()) {
				mPlayer.pause(); // 暂停
			} else {
				mPlayer.start(); // 播放
	
				new ProgressThread().start();// 启动计算播放进度的线程
			}
	
			return super.onStartCommand(intent, flags, startId);
		}
	
		@Override
		public void onDestroy() {
			super.onDestroy();
	
			// 停止播放
			mPlayer.stop();
			mPlayer.release();// 释放资源
			
			lbMgr.unregisterReceiver(seekReceiver); //注销广播接收器
		}
	
		@Override
		public IBinder onBind(Intent intent) {
			return null;
		}
	
		/**
		 * 定义计算播放进度的线程类
		 * 
		 * @author apple
		 * 
		 */
		class ProgressThread extends Thread {
			@Override
			public void run() {
	
				try {
					while (mPlayer != null && mPlayer.isPlaying()) {
	
						sumLen = mPlayer.getDuration();// 获取播放文件的总时长，单位：毫秒
						int currentPosition = mPlayer.getCurrentPosition(); // 获取当前播放的位置
	
						// 准备发送进度广播
						Intent intent = new Intent(Config.ACTION_PROGRESS);
						intent.putExtra(Config.EXTRA_PROGREES_MAX, sumLen); // 总时长
						intent.putExtra(Config.EXTRA_PROGREES_CUR, currentPosition);// 当前进度
	
						lbMgr.sendBroadcast(intent);// 发送播放的进度广播
	
						Thread.sleep(200);
	
					}
	
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		}
		
		class SeektoReceiver extends BroadcastReceiver{
			@Override
			public void onReceive(Context context, Intent intent) {
				// TODO Auto-generated method stub
				int seekPosition=intent.getIntExtra(Config.EXTRA_PROGREES_CUR, 0);
				
				mPlayer.seekTo(seekPosition);//设置播放器的播放位置
			}
		}
	}
