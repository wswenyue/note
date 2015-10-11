#Activity

##Activity之间传值

Activity跳转，返回数据/结果
   需要返回数据或结果的，则使用startActivityForResult (Intent intent, int requestCode)
，requestCode的值是自定义的，用于识别跳转的目标Activity。
   跳转的目标Activity所要做的就是返回数据/结果，setResult(int resultCode)只返回结果不带数据，或者setResult(int resultCode, Intent data)两者都返回！
   而接收返回的数据/结果的处理函数是onActivityResult(int requestCode, int resultCode, Intent data)，这里的requestCode就是startActivityForResult的requestCode，resultCode就是setResult里面的resultCode，返回的数据在data里面。

###A Activity启动B Activity，并向B传值
	
	Intent intent = new Intent(A.this,B.class);
	//TODO 向intent中放入数据
	startActivity(intent)

###A 启动 B ，并向B传值，且希望B处理完后回传给A数据

A中：

	Intent intent = new Intent(A.this,B.class);
	//TODO 向intent中放入数据
	startActivityForResult (intent, requestCode);

然后：

	@Override 
	protected void onActivityResult(int requestCode, int resultCode, Intent data) { 
	   //在这里做相应判断获取数据
	} 

B中：

	
	