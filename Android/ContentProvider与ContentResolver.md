#ContentProvider(内容提供者)---四大组件之一
 **ContentProvider组件的作用**：为外界应用提供本应用中私有数据库中的数据
**ContentResolver的作用**：访问ContentProvider提供的内容

##使用ContentResolver访问ContentProvider提供的资源
1. 获取相应资源的Uri

	   系统提供的Uri有：

       CallLog.Calls.CONTENT_URI;//

2. 	得到ContentResolver对象，并查询uri代表的资源

		    ContentResolver  resolver = getContentResolver();
    		//查询uri代表的资源(从 Uri代表的表中进行查询)
    		Cursor cursor = resolver.query(callUri, columns, null, null, null);
			//也可以一起写
			getContentResolver().query(callUri, columns, null, null, null);//执行相应的指令就行

3.  解析获取的Cursor结果集对象
4.  
