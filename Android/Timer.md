#Timer定时器
主要使用方法：

	schedule(TimerTask task, Date when) 按排任务在指定时间执行
	schedule(TimerTask task, long delay) 按排任务延迟多少毫秒之后执行
	schedule(TimerTask task, long delay, long period) 按排任务在延迟多个毫秒后执行，并间隔多少毫秒后重复
	cancel() 取消定时器
	TimerTask 定时任务

示例：

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