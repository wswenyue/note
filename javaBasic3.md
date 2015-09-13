####局部变量和全局变量（成员变量）的区别：

	局部变量：随着方法入栈而初始化，方法出栈而消失
				没有默认值，必须声明后赋值
				保存在栈中
				作用域为声明的位置到它所在的结构结束

	成员变量：随着初始化对象而创建，会随着对象的回收而消失
			有默认值的
			保存在堆中
			作用域在整个类中
	注意：当局部变量和成员变量重名时，局部变量会覆盖全局变量

####访问修饰符：
	private:私有的，只有当前类的内部可以访问 
	[default]：默认的，可以在同一个包中访问
	protected：受保护的，可以在同一个包中或者不同包的子类中可以访问
	public：公有的，所有类均可以访问

	this:当前对象。当出现局部变量和成员变量重名时需要通过this来访问成员变量

####构造方法
    构造方法：方法名和类名相同个，没有返回值类型
		作用：初始化成员变量
		调用：new
	注意：如果没有显示声明构造方法，系统会提供默认无参构造方法.
	如果显示声明构造方法，系统不提供默认构造方法。
	类名(){	
	}

	类名(参数){
		初始化数据成员
	}

	this():可以表示当前构造函数，必须位于代码行的第一行

####static关键字：
	1 静态成员变量：
	特点：
	1） 和类同时加载，会在方法去开辟空间，初始化数据
	2） 通过类名访问。  类名.静态成员变量
	3） 通常用于存储所有类的共享数据

	2 静态成员方法：
	特点：
	1）静态方法中只能访问静态成员（不能使用this）
	2）非静态方法中可以访问静态成员也可以访问非静态成员
	
	静态成员变量和非静态成员变量区别：
	1）生命周期
		静态成员变量随着类的加载而加载，在方法区初始化，在程序结束即消失
		非静态成员变量随着对象的初始化，在堆中创建，在对象被垃圾回收时而消失

	2）存储
		静态成员变量通常保存对象共享的数据
		非静态成员变量是保存某一个对象的数据
	
	3）访问方式
		静态成员变量通过类名直接访问，也可以通过对象访问
		非静态成员变量只能通过对象访问
>

    class  Student
    {
    	static String sex = "男";  //静态成员变量
    
    	private int age;  //实例成员变量
    
    	public void setAge(int age){
    		this.age = age;
    	}
    
    	//静态方法
    	public static void setSex(String sex){
    		Student.sex = sex;
    		//age = 15;  //静态方法中不能访问非静态变量
    		/*Student s = new Student();  //在静态方法中如果想访问实例成员，需要通过对象访问
    		s.age = 15;*/
    	}
    	//实例方法
    	public void show(){
    		setSex("女");
    		System.out.println(sex+"   "+ age);
    	}
    }

---
####代码块

    构造代码块：优先于构造方法执行，主要用于初始化成员变量
				每次创建对象都会执行
	静态代码块：static{}，用来初始化静态成员变量。随着类的加载而执行，只执行一次
	注意：在程序优化中，不建议过多的使用static，因为它会长时间保留在内存中
>

    class Demo06 
    {
    public static void main(String[] args) 
		{
			System.out.println("main方法开始执行.....");
			{
				//普通代码块：作用域
				int i= 6;
				System.out.println("i-->"+i);
			}//i作用域消失
			int i = 10;
			System.out.println("iiii-->"+i);
	
			
			System.out.println("*****************");
			Student s1 = new Student();
			System.out.println("*****************");
			new Student();	
		}
		static int num = 10;
		int a = 5;
		//静态代码块
		static{
			System.out.println("静态代码块1");
			System.out.println(num);
			//System.out.println(a);  //静态代码块中不能直接引用非静态成员
		}
		static{
		System.out.println("静态代码块2");
		}
	
		/*如果执行该代码块程序将强制退出
		static{
			System.exit(-1);
		}*/
    }


    class Student
    {
		static  int age = 1;
	
		{
			System.out.println("构造代码块1");
		}
		{
			System.out.println("构造代码块2");
		}
		public Student(){
			System.out.println("Student构造方法");
		}
		{
			System.out.println("构造代码块3");
		}
    }
>程序运行结果
>    
    静态代码块1
        10
    静态代码块2
    main方法开始执行.
    i-->6
    iiii-->10
    *****************
    构造代码块1
    构造代码块2
    构造代码块3
    Student构造方法
    *****************
    构造代码块1
    构造代码块2
    构造代码块3
    Student构造方法


这里再看一个例子：

    package Demo;
    
    /**
     * Created by wswenyue 
     */
    public class Test1 {
    public static void main(String[] args) {
    System.out.println("------------new 子类-------------");
    new B().f();
    System.out.println("----------new 父类--------------");
    new A().f();
    }
    }
    
    class A {
    public int a ;
    static {
    System.out.println("父类静态代码块1");
    }
    public A(){
    System.out.println("父类无参构造方法");
    }
    public A(int a){
    this.a = a;
    System.out.println("父类有参参构造方法");
    }
    
    public void f(){
    {
    System.out.println("父类方法--普通代码块");
    }
    System.out.println("父类方法");
    }
    
    {
    System.out.println("父类普通代码块");
    }
    static {
    System.out.println("父类静态代码块2");
    }
    }
    
    class B extends A {
    public int a ;
    static {
    System.out.println("子类静态代码块1");
    }
    public B(){
    System.out.println("子类无参构造方法");
    }
    public B(int a){
    this.a = a;
    System.out.println("子类有参参构造方法");
    }
    
    public void f(){
    {
    System.out.println("子类方法--普通代码块");
    }
    System.out.println("子类方法");
    }
    
    {
    System.out.println("子类普通代码块");
    }
    static {
    System.out.println("子类静态代码块2");
    }
    }

执行结果：

    ------------new 子类-------------
    父类静态代码块1
    父类静态代码块2
    子类静态代码块1
    子类静态代码块2
    父类普通代码块
    父类无参构造方法
    子类普通代码块
    子类无参构造方法
    子类方法--普通代码块
    子类方法
    ----------new 父类--------------
    父类普通代码块
    父类无参构造方法
    父类方法--普通代码块
    父类方法

----
####对象初始化过程：
		1 加载类的字节码文件到jvm的方法区中
		2 为静态变量在静态区开辟内存空间，赋初始值
		3 加载静态代码块，初始化静态成员变量
		4 开辟堆中空间（成员变量），给成员变量初始化
		5 加载构造代码块
		6 加载构造方法
		7 将堆空间的首地址赋值给栈中对象的引用

示例：

    class Demo07 
    {
    	public static int age;
    	static{  //静态代码块
    		System.out.println("静态代码块");
    		System.out.println("age--->"+age);
    		age = 18; //初始化静态成员变量，在类加载的时候执行
    		System.out.println("age--->"+age);
    	}
    	{
    		System.out.println("构造代码块");
    		//构造代码块
    		age += 1;
    		System.out.println("age--->"+age);
    	}
    	Demo07(){
    		System.out.println("构造方法");
    	}
    	public static void main(String[] args) 
    	{
    		new Demo07();
    		new Demo07();
    		new Demo07();
    	}
    }
> 运行结果

    静态代码块
    age--->0
    age--->18
    构造代码块
    age--->19
    构造方法
    构造代码块
    age--->20
    构造方法
    构造代码块
    age--->21
    构造方法

---

####继承
    子类可以继承父类非私有的成员
	继承的传递性：---多层继承
	class  A{}
	class B extends A{}
	class C extends B{}
    
#####子类对象初始化过程：
	1 父类静态代码块
	2 子类静态代码块
	3 父类构造代码块
	4 父类构造方法
	5 子类构造代码块
	6 子类构造方法
示例
	
	//父类对象
    class Parent
    {
    	private String name;
    	public int age;
    	static{
    		System.out.println("父类的静态代码块");
    	}
    	{
    		System.out.println("父类的构造代码块");
    	}
    	Parent(){
    		System.out.println("父类的构造方法");
    	}
    	public void say(){
    		System.out.println("父类age:"+age);
    	}
    }
    
	//子类对象
    class Child extends Parent
    {
    	private int age;
    	static{
    		System.out.println("子类静态代码块");
    	}
    	{
    		System.out.println("子类构造代码块");
    	}
    	Child(){
    		System.out.println("子类构造方法");
    	}
    	public void sayHi(){
    		super.age = 100;
    		this.age = 50;
    		System.out.println("子类age:"+age);
    		say();
    	}
		
	//现在我们来调用测试一下
	public static void main(String[] args) 
	{
		Child c = new Child();
		c.sayHi();
	}

输出结果：

    父类的静态代码块
    子类静态代码块
    父类的构造代码块
    父类的构造方法
    子类构造代码块
    子类构造方法
    子类age:50
    父类age:100

---

**this和super：**

	this：代表当前对象。this.成员。
		   this（）本类的构造方法，必须在第一行
	super：代表父类对象，并不是一个引用，没有父类的指向（因此super不能作为参数传递）
			可以通过super.父类成员来访问父类的成员
			也可以通过super（）调用父类的构造方法，必须在第一行
		
	this（）和super（）不能同时出现
	this和super关键字不能出现在静态方法中

#####重写（覆盖）
	前提条件 必须继承
	注意：
		1 父类的私有方法不能被重写
		2 子类重写父类的方法时，重写的方法权限必须大于等于父类中的方法权限
		3 静态只能静态覆盖
		4 当父类中的方法有返回类型时，子类重写父类方法时，返回类型可以是父类类型也可以是父类的子类类型
