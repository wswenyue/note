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

## ContentProvider
概念： ContentProvider提供的数据访问接口，又称统一资源标识符，类同Url

数据库在Android当中是私有的，不能将数据库设为WORLD_READABLE,进程间传递数据,或者访问数据，可以使用ContentProvider来实现。

ContentProvider的功能和意义：

     ContentProvider是不同应用程序之间进行数据交换的标准API。当一个应用程序需要把自己的数据暴露给其他应用程序使用时，该应用程序可以通过提供ContentProvider来实现；而其他应用程序需要使用这些数据时，可以通过ContentResolver来操作ContentProvider暴露的数据。
     一旦某个应用程序通过ContentProvider暴露了自己的数据操作接口，那么不管该应用程序是否启动，其他应用程序都可以通过该接口来操作被暴露的内部数据，包括增加数据、删除数据、修改数据、查询数据等。
     虽然大部分使用ContentProvider操作的数据都来自于数据库，但是也可以来自于文件、SharedPreferences、XML或网络等其他存储方式。


格式： `content://Authority/path/Matches`
完整示例：

	content://com.example.app.provider/table1/*
	content://com.example.app.provider/table1/#:


UriMatcher
	
	UriMatcher.addURI(String authority,String table, int code)
	int UriMatcher.match(Uri)

MIME Types

	ContentProvider.getType(Uri) 获取Uri资源的数据类型
	格式：vnd.<Subtype>.<path>
	Type: vnd
	Subtype	
		单行： android.cursor.item/
		多行： android.cursor.dir/
	完整的数据类型
		单行： vnd.android.cursor.item/name
		多行： vnd.android.cursor.dir/contacts

###服务端
####服务端声明公共的常量
- AUTHORITY 访问某一ContentProvider的标识，
一般为特定ContentProvider的全类名（小写）
- CONTENT_URI 访问的完整Uri路径
- 访问字段,包含BaseColumns 基本字段
 `_ID`,`_COUNT`

####在AndroidManifest.xml清单中注册ContentProvider并声明访问权限

###客户端
- Context.getContentResolver() 获取ContentProvider查询对象
- 在AndroidManifest.xml清单中声明ContentProvider要求的权限

##ContentResolver类
`query(Uri,String[],String,String [],String)` 查询

    第一个参数：访问某一个ContentProvider的数据接口
    第二个参数：查询数据的字段
    第三个参数：查询数据的条件
    第四个参数：第三个参数中包含 ？时，按照？的位置指定其替换的内容
    第五个参数：排序的字段

`insert(Uri,ContentValues)`插入数据

`update(Uri,ContentValues,String,String[]) `更新数据

`delete(Uri,String,String[]) `删除数据

`getType(Uri)` 获取数据类型

##系统自带ContentProvider的常用Uri地址
	
	（一）、Android系统管理联系人的Uri如下：
		ContactsContract.Contacts.CONTENT_URI 管理联系人的Uri
		ContactsContract.CommonDataKinds.Phone.CONTENT_URI 管理联系人的电话的Uri
		ContactsContract.CommonDataKinds.Email.CONTENT_URI 管理联系人的Email的Uri
	【数据库中主要字段：】
		联系人id字段名称为：ContactsContract.Contacts._ID
		联系人name 字段为：ContactContract.Contracts.DISPLAY_NAME
		电话信息表的外键id为：ContactsContract.CommonDataKinds.Phone.CONTACT_ID
		电话号码 字段为：ContactsContract.CommonDataKinds.Phone.NUMBER.
		Email 字段为：ContactsContract.CommonDataKinds.Email.DATA
		其外键为：ContactsContract.CommonDataKinds.Email.CONTACT_ID
	
	（二）、Android为多媒体提供的ContentProvider的Uri如下：
		MediaStore.Audio.Media.EXTERNAL_CONTENT_URI   存储在SD卡上的音频文件
		MediaStore.Audio.Video.EXTERNAL_CONTENT_URI   存储在 SD卡上的视频
		MediaStore.Audio.Images.EXTERNAL_CONTENT_URI  存储在 SD卡上的图片文件内容
		MediaStore.Audio.Media.INTERNAL_CONTENT_URI   手机内部存储器上的音频文件 
		MediaStore.Audio.Video.INTERNAL_CONTENT_URI   手机内部存储器上的视频
		MediaStore.Audio.Images.INTERNAL_CONTENT_URI  手机内部存储器上的图片
	【数据库中主要字段：】
		图片名称字段：Media.DISPLAY_NAME
		图片的详细描述字段：Media.DESCRIPTION  
		图片的保存位置字段：Media.DATA
	（三）、短信Uri：
	 content://sms所有短信
		 content://sms/outbox  发送箱中的短信 
	 content://sms/inbox   收件箱中短信
	【数据库中主要字段：】
	 短信手机号码 ： address
	 短信标题：  subject
	 短信内容：body
	 短信发送时间戳： date
	 短信类型：type

##系统内置的ContentProvider：

    1、Contacts：获取、修改、保存联系人的信息；
    2、MediaStore：访问声音、视频、图片等多媒体文件；
    3、CallLog ：查看或更新通话记录；
    4、Browser：读取或修改浏览历史、网络搜索、书签；
    5、Setting：查看和获取蓝牙设置、铃声设置等设备首选项。

##CallLog 电话记录
`Calls.CONTENT_URI`：Uri 常量->`content://call_log/calls`

`CallLog.Calls` 表字段

    _ID 主键
    NUMBER 电话号码
    DATE 打电话的时间
    TYPE 电话类型，1:拨入、2:拨出、3:未接 

需要的权限：`android.permission.READ_CALL_LOG`

##短信记录
**Uri常量:**
`content://sms` 所有短信
`content://sms/outbox` 发送的短信
`content://sms/inbox`  接收的短信

**常用的字段:**

    _id 主键
    address 地址
    date 日期
    body 内容
    type 类型, 1:接收  2:  发送  3: 草稿  4: 未读  5: 消息发送失败

**需要的权限**：`android.permission.READ_SMS`

##ContactsProvider 联系人
**定义： 管理联系人的ContentProvider**

**Android系统中提供的联系人数据库位置**：`data/data/com.android.providers.contacts/databases`

**Uri常量：**
    
    content://com.android.contacts/contacts  联系人账户表，可认为手机和sim卡
    content://com.android.contacts/raw_contacts 联系人表，包含联系人id和姓名
    content://com.android.contacts/data 数据表，包含每一个联系人的相关数据
    电话号码的Uri==> content://com.android.contacts/data/phones
    EMAIL的URI==> content://com.android.contacts/data/emails

**联系人相关的类:**

    ContactsContract  联系人类
    ContactsContract.RawContacts类 ，操作raw_contacts表
    ContactsContract.Data类，操作data表
    ContactsContract.CommonDataKinds.Phone类，操作raw_contacts和data表，且类型为电话号码
    ContactsContract.CommonDataKinds.Email类，操作raw_contacts和data表，且类型为email

**常用的表及字段:**
`raw_contacts`表

    _id 主键
    display_name 姓名

`data`表

    _id  主键
    raw_contact_id 联系人的_id
    data1 姓名 或 电话 或 邮箱
    data2  是data1数据的类型
    data3 是自定义data1数据的类型
    mimetype_id 记录数据的类型，是引用mimetypes表的_id值，即是一个外键

`mimetypes`表

	_id 主键
	mimetype 类型名
		vnd.android.cursor.item/email_v2 :  1  邮箱
		vnd.android.cursor.item/phone_v2： 5 电话
		vnd.android.cursor.item/name：7  姓名

###封装操作类
	
	public class ContactsHelper {
		private static String uri_rawcontacts = "content://com.android.contacts/raw_contacts";
		private static String uri_contacts_phones = "content://com.android.contacts/data/phones";
		private static String uri_contacts_emails = "content://com.android.contacts/data/emails";
		private String uri_contacts_data = "content://com.android.contacts/data";
	
	
		// 查询联系人的信息
		public static List<Map<String, Object>> selectContactsInfo(
				ContentResolver resolver) {
			List<Map<String, Object>> list = new ArrayList<Map<String, Object>>();
			
	      Cursor contactsCursor = resolver.query(Uri.parse(uri_rawcontacts),
					new String[] { "_id", "display_name" }, null, null, null);
	
			while (contactsCursor.moveToNext()) {
				Map<String, Object> map = new HashMap<String, Object>();
				int contactsId = contactsCursor.getInt(contactsCursor
						.getColumnIndex("_id"));
				String displayName = contactsCursor.getString(contactsCursor
						.getColumnIndex("display_name"));
				map.put("_id", contactsId);
				map.put("display_name", displayName);
	
				// 根据联系人的id去data表获取电话号码的信息
				Cursor phoneCursor = resolver.query(Uri.parse(uri_contacts_phones),
						new String[] { "raw_contact_id", "data1" },
						"raw_contact_id=?", new String[] { contactsId + "" }, null);
	
				StringBuilder sb = new StringBuilder();
				while (phoneCursor.moveToNext()) {
					sb.append(phoneCursor.getString(1));
					sb.append("|");
				}
				map.put("phones", sb.toString());
	
				// 根据联系人的id去data表获取email的信息
				Cursor emailCursor = resolver.query(Uri.parse(uri_contacts_emails),
						new String[] { "raw_contact_id", "data1" },
						"raw_contact_id=?", new String[] { contactsId + "" }, null);
	
				StringBuilder sb2 = new StringBuilder();
				while (emailCursor.moveToNext()) {
					sb2.append(emailCursor.getString(1));
					sb2.append("|");
				}
				map.put("emails", sb2.toString());
	
				list.add(map);
			}
			return list;
		}
	
		// 修改联系人的姓名
		public boolean updateContactsName(ContentResolver resolver,
				Map<String, Object> map, String id) {
	
			ContentValues values = new ContentValues();
			// 更改raw_contacts表中的姓名
			values.put("display_name", map.get("display_name").toString());
			values.put("display_name_alt", map.get("display_name").toString());
	
			int result1 = resolver.update(Uri.parse(uri_rawcontacts), values,
					"_id=?", new String[] { id });
	        
			// 更改data表中的姓名
			values.clear(); //清除字段及字段值
	
			values.put("data1", map.get("display_name").toString());
			values.put("data2", map.get("display_name").toString());
			int result2 = resolver.update(Uri.parse(uri_contacts_data), values,
					"raw_contact_id=? and mimetype_id=?", new String[] { id, "7" });
	
			// 更改data表中的phone
			values.clear();
			values.put("data1", map.get("phone").toString());
			values.put("data2", 2);
			int result3 = resolver.update(Uri.parse(uri_contacts_data), values,
					"raw_contact_id=? and mimetype_id=?", new String[] { id, "5" });
			// 更改data表中的email
			values.clear();
			values.put("data1", map.get("email").toString());
			values.put("data2", 1);
			int result4 = resolver.update(Uri.parse(uri_contacts_data), values,
					"raw_contact_id=? and mimetype_id=?", new String[] { id, "1" });
	
			// 更改data表中的email
			if (result1 > 0 && result2 > 0 && result3 > 0 && result4 > 0) {
				return true;
			} else {
				return false;
			}
		}
	
		// 删除联系人信息
		public boolean deleteContacts(ContentResolver resolver, String displayName) {
			int data = resolver.delete(Uri.parse(uri_rawcontacts),
					"display_name=?", new String[] { displayName });
			if (data > 0) {
				return true;
			}
			return false;
		}
	
		// 新增数据
		public void insertContact(ContentResolver resolver, Map<String, Object> map) {
			ContentValues values = new ContentValues();
			// 往raw_contacts表中插入一条空数据，目的是获取联系人的id
			Uri newUri = resolver.insert(Uri.parse(uri_rawcontacts), values);
			long id = ContentUris.parseId(newUri);
	
			// 往data表中插入联系人姓名的数据
			values.put("raw_contact_id", id);
			// values.put("mimetype_id", 7);//必须要插入mimetype字段，而不可以直接插入mimetype_id.
			values.put("mimetype", "vnd.android.cursor.item/name");
			values.put("data1", map.get("display_name").toString());
			values.put("data2", map.get("display_name").toString());
			resolver.insert(Uri.parse(uri_contacts_data), values);
	
			// 往data表中插入联系人的电话信息
			values.clear();
			values.put("raw_contact_id", id);
			values.put("mimetype", "vnd.android.cursor.item/phone_v2");
			values.put("data1", map.get("phone").toString());
			values.put("data2", 2);
			resolver.insert(Uri.parse(uri_contacts_data), values);
	
			// 往data表中插入联系人的email
			values.clear();
			values.put("raw_contact_id", id);
			values.put("mimetype", "vnd.android.cursor.item/email_v2");
			values.put("data1", map.get("email").toString());
			values.put("data2", 1);
			resolver.insert(Uri.parse(uri_contacts_data), values);
	
		}
	
	}

###在AndroidManifest.xml文件中配置权限
`<uses-permission android:name="android.permission.READ_CONTACTS" />`

`<uses-permission android:name="android.permission.WRITE_CONTACTS" />`

##自定义ContentProvider
###ContentProvider提供方
####创建类,继承ContentProvider抽象类

	ContentProvider类中六个抽象方法的说明：
	1、onCreate()   初始化provider
	2、query()      返回数据给调用者
	3、insert()     插入新数据到ContentProvider
	4、update()     更新ContentProvider已经存在的数据
	5、delete()     从ContentProvider中删除数据
	6、getType()    返回ContentProvider数据的Mime类型

	备注：】
	   ContentProvider是单例模式的，当多个应用程序通过使用ContentResolver 来操作使用ContentProvider 提供的数据时，ContentResolver 调用的数据操作会委托给同一个ContentProvider 来处理。这样就能保证数据的一致性。

#### 定义公共的Authority和Code的访问字段

    //类似服务器地址，必须保证应用环境中唯一性，一般要小写类的全名
    private static final String AUTHORITY="com.train.content.studentcontentprovider";
    //声明查询不同表的类型
	private static final int STUDENTS=0; //查询学生表
	private static final int CITYS=1;    //查询城市表

#### 定义UriMatcher,并增加相应的操作类型
    
    UriMatcher：
       其他应用在通过ContentResolver对象执行CRUD操作时，都需要一个重要的参数Uri。为了能顺利提供这个Uri参数，Android系统提供了一个UriMatcher工具类。
    
    UriMatcher工具类提供了两个方法：
      1、void addURI(String authority , String path , int code)
    
    该方法用于向UriMatcher对象注册Uri。其中authority和path是Uri中的重要部分。而code代表该Uri对应的标示符。
    
      2、int match(Uri uri) ： 根据注册的Uri来判断指定的Uri对应的标示符。如果找不到匹配的标示符，该方法返回-1。
    
    示例：
    
    	private static UriMatcher uriMatcher; //用于区分当前请求中的Uri操作类型
    	static{
    		//实例化Uri匹配对象中
    		uriMatcher=new UriMatcher(UriMatcher.NO_MATCH);
    		
    		uriMatcher.addURI(AUTHORITY, "students", STUDENTS); //增加操作student表的Uri
    		uriMatcher.addURI(AUTHORITY, "tcity", CITYS); //增加操作tcity表的Uri
    	}

#### 在相应的方法中使用UriMatcher.match(Uri)方法获取操作类型

#### 在AndroidManifest.xml清单中注册ContentProvider

    使用<permission>标签声明权限
    使用<provider>标签注册ContentProvider，并声明相关的属性

#### **注意：path的路径中不能加"/"前辍**

###ContentResolver使用方
通过Context.getContentResolver()调用相关方法

    query(Uri,String[],String,String [],String)
    insert(Uri,ContentValues)
    update(Uri,ContentValues,String,String[])
    delete(Uri,String,String[])
    getType(Uri)

在AndroidManifest.xml清单中声明ContentProvider要求的权限

    例如：
       <uses-permission android:name="android.permission.READ_CONTACTS" />

###截取Uri地址和拼接Uri地址

Uri对象的截取方法
    
    public abstract String getLastPathSegment() 获取最一个“/”后的内容
    public abstract String getPath () 获取path部分
    public abstract List<String> getPathSegments () 获取path中的每一个"/"后的内容

Uri对象的拼接方法

	public static Uri withAppendedPath(Uri baseUri, String pathSegment)

ContentUris对象的截取id的方法

	public static long parseId (Uri contentUri)

ContentUris对象的拼接id的方法
	
	public static Uri withAppendedId (Uri contentUri, long id)

