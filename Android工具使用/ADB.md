#ADB
##常见问题集
###adb无法连接

	adb server is out of date.  killing...
	ADB server didn't ACK
	* failed to start daemon *
	error: unknown host service

或者

    * daemon not running. starting it now *   
    ADB server didn't ACK   
    * failed to start daemon * 
	
当出现上面问题就是`其他的进程占用了adb的5037端口`，导致adb服务不能开启

**解决方法：**

**查看5037端口被谁占用了**
开始--运行--CMD 到命令提示符，输入 `netstat -aon|findstr "5037"`
    
    C:\Users\wswenyue\Downloads>netstat -aon|findstr "5037"
      TCP127.0.0.1:5037 0.0.0.0:0  LISTENING   5076

看到是(PID)5076这个进程占用了，接着**查看该进程是不是adb进程**：

`tasklist|findstr "2748" `
    
    C:\Users\wswenyue\Downloads>tasklist|findstr "5076"
    shuame_helper.exe 5076 Console1  6,216 K

**不是adb进程去任务管理器把该进程关了。重启adb就好了**

-------------------


