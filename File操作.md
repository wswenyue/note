#File及Properties类的使用
> 说明：File类代表系统文件或目录,即磁盘上的文件和目录都是通过File类的实例表示

###构造文件对象

    File(File dir, String name)	//第一个参数：指定所在目录的文件对象;第二个参数：包含扩展名的文件名
    File(String dirPath, String name)	//第一个参数：指定所在目录的路径;第二个参数：包含扩展名的文件名
    File(String path)	//指定文件的绝对路径
    File(URI uri)	//指定资源唯一标识的对象

###访问文件的属性

    public boolean canRead() 是否可读
    public boolean canWrite() 是否可写
    public boolean exists() 文件或目录是否存在
    public boolean isDirectory() 是否目录/路径
    public boolean isFile() 是否是文件
    public boolean isHidden() 是否是隐藏的
    public boolean isHidden() 是否是隐藏的
    public long length()获取文件长度，单位：字节
    public String getName() 获取文件名称
    public String getPath() 获取文件路径
    public String getAbsolutePath()  返回此File对象的绝对路径
    public String getParent() 获取父目录，没有则为null
	public static final String separator 文件路径分隔符(在 UNIX 系统上，此字段的值为 '/'；在 Microsoft Windows 系统上，它为 '\\')

###对文件的操作

    public boolean createNewFile() //不存在时创建此文件对象所代表的空文件
    public boolean delete()　//删除文件。如果是目录必须是空才能删除
    public boolean mkdir() 　//创建此抽象路径名指定的目录
    public boolean mkdirs()　//创建此抽象路径名指定的目录，包括所有必需但不存在的父目录
    public boolean renameTo(File dest)	//重新命名此抽象路径名表示的文件

###浏览目录中的文件和子目录

    public String[] list()返回此目录中的文件名和目录名的数组
    public File[] listFiles()返回此目录中的文件和目录的File实例数组
    public File[] listFiles(FilenameFilter filter)返回此目录中满足指定过滤器的文件和目录
	java.io.FilenameFilter接口：实现此接口的类实例可用于过滤文件名

##Properties类的使用
> 说明：HashTable的子类，属于集合类，存储属性类型的键值对，键和值默认都是 String

注意：**是集合中可以和流结合使用的一个集合类**

构造方法：

    Properties()
    Properties(Properties properties)

加载方法：

    load(InputStream in)
    load(Reader in)
    loadFromXML(InputStream in)

保存方法：

    store(OutputStream out, String comment)
    store(Writer writer, String comment)
    storeToXML(OutputStream os, String comment, String encoding)
    storeToXML(OutputStream os, String comment)

打印方法：

    list(PrintWriter out)
    list(PrintStream out)

获取方法：

    Enumeration<?>  propertyNames()
    String  getProperty(String name)
    String getProperty(String name, String defaultValue)
    Set<String>  stringPropertyNames()
    
修改方法：

    Object setProperty(String name, String value)






