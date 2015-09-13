####static

    修饰变量：类的变量 类名调用
	修饰方法：类的方法  类名调用。只能直接访问静态成员
	修饰内部类：只能修饰静态内部类
	修饰代码块：给静态变量初始化，只执行一次

####final关键字：最终的

	final修饰变量：常量，值不能修改
	final修饰方法：不能被重写
	final修饰类：不能被继承

####abstract关键字：抽象的

    抽象类：abstract修饰的类，抽象类中可以有抽象方法，也可以有非抽象方法。
    抽象类不能实例化对象。
    如果子类继承了抽象类，必须实现抽象类中所有的抽象方法。
    抽象方法：abstract修饰的方法，只有方法声明没有方法的实现。抽象方法必须在抽象类中。

思考：

    1 抽象类一定是父类吗？ 一定是父类
	2 抽象类可以有构造方法吗？有，用于子类对象的初始化
	3 有抽象方法的类一定是抽象类吗？一定
	4 抽象类中一定有抽象方法吗？不一定
	5 抽象方法不可以和哪些关键字同时使用？
		final：抽象类必须有子类，被final修饰的类不能有子类
		static：抽象方法不能被调用，静态方法通过类名调用
		private：抽象方法必须是可以被子类重写的，私有方法不能被重写

####interface 接口

实现接口可以得到该继承体系之外的额外的功能  
接口之间可以多继承

    interface 接口名{
    		抽象方法（public abstract）
    		全局常量（public static  final）
    	}
    
    	接口：interface 规范
    	接口可以解决单继承的问题。接口支持多继承。
    	子类实现接口是，必须重写接口中的抽象方法。
    	创建子类对象时，调用子类重写的方法。
    
    	接口好处：
    		1 规范
    		2 提高程序可扩展性
    		3 降低类之间的依赖关系
    
示例

    /*
    	接口之间可以多继承
    */
    interface Ia
    {
    }
    interface Ib
    {
    }
    interface Ic extends   Ia,Ib
    {
    }

接口是一种标准（规范）

    class Demo07 
    {
    	public static void main(String[] args) 
    	{
    		IUsb usb = new Computer();
    		usb.insert();
    		usb.pop();
    	}
    }
    
    interface IUsb
    {
    	void insert();
    	void pop();
    }
    class Computer implements IUsb
    {
    	public void insert(){
    		System.out.println("插入usb");
    	}
    	public void pop(){
    		System.out.println("弹出usb");
    	}
    }
    
####多态

    多态：多种形态
		运行时多态：重写  父类类型引用指向了子类的对象  父类  对象名 = new  子类()；  
 	    编译时多态：重载

    前提条件：继承或者实现


    子类  对象名 = new 子类（）；
	    可调用的方法：父类继承的方法、子类重写的方法、子类新增的方法
	    可执行的方法体：子类重写方法

    父类  对象名 = new 子类（）；
	    可调用的方法：父类继承的方法、子类重写的方法
	    可执行的方法体：子类重写方法

#####类型转换
    
    基本数据类型：
    	自动类型转换   小-->大
    	强制类型转换  （小类型）变量
    
    引用类型：父类可以看做是大的类型，子类是小的类型
    
    子--（向上转型）--》父--（向下转型）--》子
    
    猫--->动物--->猫
    狗--->动物--×-->猫  ：ClassCastException 类型转换异常

注意：

    向上转型：父类 对象名 = new 子类（）；
	向下转型：子类 对象名 = （子类）父类对象；
		前提条件：向上是向下的前提，需要强转
		可能会抛出异常：.ClassCastException
		  解决：判断  
			if（对象 instanceof  类名）{}

	多态中成员的特点：
		成员变量：编译时能访问哪些变量看父类，执行结果看父类
		成员函数：编译时期能访问哪些方法看父类，执行结果看子类（前提子类重写父类的方法,没有重写还是看父类）
		静态成员函数：编译执行都看父类


多态示例：
    
    class Demo10 
    {
    	/*
    		多态：
    	*/
    	public static void main(String[] args) 
    	{
    		//父类 对象名 = new 子类（）；
    		Animal cat = new Cat();
    		cat.eat();  //从父类继承的方法、子类重写的方法
    		cat.f(); //调用、执行均看父类
    
    		Animal dog = new Dog();
    		dog.eat();
    
    	
    		chi(dog);
    		System.out.println("------------------");
    		
    		Animal a1 = new Cat();
    		Animal a2 = new Dog();
    
    		if(a1 instanceof Cat){
    			Cat c1 = (Cat)a1 ; //父类继承的方法  子类重写的方法  子类新增的方法
    			c1.eat();
    			c1.catchMouse();
    			System.out.println("恭喜你，成功变回猫");
    		}else{
    			System.out.println("你本不是一只猫");
    		}
    
    		System.out.println("------------------");
    		if(a2 instanceof Cat){
    			System.out.println("恭喜你，成功变回猫");
    			Cat c2 = (Cat)a2;  //狗不能转成猫
    			c2.eat();
    			c2.catchMouse();
    		}
    		else{
    			System.out.println("你本不是一只猫");
    		}
    		System.out.println("------------------");
    		if(a2 instanceof Dog){
    			((Dog)a2).eat();
    			((Dog)a2).watchHome();
    			System.out.println("终于变回狗了");
    		}
       	}
    /*
    	public static void chi(Cat c){
    		c.eat();
    	}
    	public static void chi(Dog d){
    		d.eat();
    	}*/
    	public static void chi(Animal a){
    		a.eat();
    	}
    }
    abstract class Animal
    {
    	public abstract void eat();
    	public static void f(){
    		System.out.println("Animal static ");
    	}
    }
    class Cat extends Animal
    {
    	public void eat(){
    		System.out.println("猫吃鱼");
    	}
    	public void catchMouse(){
    	
    		System.out.println("猫抓老鼠");
    	}
    	public static void f(){
    		System.out.println("Cat static ");
    	}
    }
    class Dog extends Animal
    {
    		public void eat(){
    			System.out.println("狗吃骨头");
    		}
    		public void watchHome(){
    			System.out.println("狗看家");
    		}
    }



----

####内部类
- 成员内部类
- 静态内部类
- 局部内部类
- 匿名内部类

特点：
> 内部类可以直接访问外部类成员   
> 外部类要访问内部类的成员，必须建立内部类的对象

作用：
>   1 隐藏部分信息
	2 可以访问外部类的私有成员
	3 弥补了单继承的局限性
	4 解决问题：如果要描述一个事物中还包括另一个事物,同时这个事物要访问被描述的事物


生成字节码文件：外部类$内部类.class

#####成员内部类
成员内部类处在了外部类成员的位置上，所以内部类可以直接访问外部类的成员

代码示例：

    [public/default]class 外部类{
		属性
		方法
		内部类：
		[访问修饰符] class 内部类{ //public private default protected
			属性
			方法
		}
	}

说明：

    1 内部类可以直接调用外部类的成员，包括private
    2 如果内部类与外部类的属性或者方法同名时，则内部类默认调用内部类自己的。
    	如果想调用外部类的
    		外部类.this.成员
    3 创建内部类对象
    	1)
    		外部类 外部类对象 = new 外部类（）；
    		外部类.内部类 内部类对象 = 外部类对象.new 内部类（）；
    	2)
    		外部类.内部类 内部类对象 = new 外部类（）.new 内部类（）;
    4 如果外部类想调用内部类成员，需要通过创建内部类对象来调用。
    5 编译之后生成的内部类字节码文件为：外部类$内部类.class

示例：

    class  Demo13
    {
    	public static void main(String[] args) 
    	{
    		Outer1 out = new Outer1();
    		out.test();
    		System.out.println("通过创建内部类对象访问内部类成员");
    		Outer1.Inner1 inner = new Outer1().new Inner1();
    		inner.show();
    	}
    }
    
    class Outer1
    {
    	private int num = 5;
    	class Inner1  //成员内部类
    	{
    		int num = 500;
    		public void show(){
    			System.out.println("inner-->num="+num);
    			System.out.println("outer-->num="+Outer1.this.num);
    		}
    	}
    
    	public void test(){  //外部类的方法
    		Inner1 in = new Inner1();//外部类访问内部类成员，需要通过对象访问
    		in.show();
    	}
    }

#####静态内部类
静态内部类:相当于外部类，因为static修饰在初始化外部类时就已经被加载到内存中了

写法示例：

    [public|default] class 外部类{
    	属性
    	方法
    	//静态内部类
    	[访问修饰符] static class 内部类{
    		属性
    		方法
    	}
    }

注意：

	1 不能调用外部类非静态成员
	2 允许定义非静态的变量和方法
	3 内部类中不能使用 外部类.this
	4 静态内部类使用的格式（不依赖于外部类对象）
	  外部类.内部类 内部类对象 = new 外部类.内部类（）

示例：

    class Demo14 
    {
    	public static void main(String[] args) 
    	{
    		Outer2 out = new Outer2();
    		out.test();
    		System.out.println("直接访问内部类方法");
    		//调用静态内部类中的非静态方法
    		Outer2.Inner2 in = new Outer2.Inner2();
    		in.show();
    		System.out.println("-------------");
    		//调用静态内部类中的静态方法
    		Outer2.Inner2.show();
    	}
    }
    class Outer2
    {
    	static int num = 10;
    	static class Inner2  //静态内部类
    	{
    		public static void show(){
    			System.out.println("num="+num);
    		}
    
    	}
    
    	public void test(){
    		//Inner2.show();
    		Inner2 in = new Inner2();
    		in.show();
    	}
    }

#####局部内部类
局部内部类：定义在方法的内部

写法示例：

	[public|default] class  外部类{
		属性
		方法（）{
			class 内部类{
			
			}
		}
	}

应用：多线程、网络下载

注意：

- 1 局部内部类或者局部方法的访问修饰符只能是default的
- 2 局部内部类可以访问外部类成员
- 3 **局部内部类如果想使用所在方法的局部变量时，该变量必须是final的（因为final存在于方法区，生命周期较长，不影响局部内部类的使用）**
- 4 局部内部类定义在方法中，适用范围仅在方法内

示例代码：

    class Demo16 
    {
    	public static void main(String[] args) 
    	{
    		Outer3 out = new Outer3();
    		out.test();
    	}
    }
    class Outer3
    {
    	int a = 5;
    	void test(){
    		final int b = 100;
    		class Inner3
    		{
    				public void show(){
    					System.out.println("外部类成员a="+a);
    					System.out.println("方法中的成员b="+b);
    				}
    		}//end class
    		Inner3 in = new Inner3();
    		in.show();
    	}//end test()
    }
    

##### 匿名内部类

匿名内部类：
> 内部类的简写，没有类名。只使用一次对象时，才会定义匿名内部类

前提：
> 匿名内部类必须继承或者实现一个外部类或者接口

应用：
> 应用于抽象类和接口，增加程序的灵活性

语法：

	类名  对象名 = new 类名(){  //抽象类或者接口的名字
		将抽象类或者接口的方法实现
	};
    

示例代码一：

    class Demo18 
    {
    	/*
    	匿名内部类没有类名
    	*/
    	class Inner5 implements IUsb
    	{
    		public void insert(){
    				System.out.println("插入");
    			}
    			public void pop(){
    				System.out.println("弹出");
    			}
    	}
    
    	public static void main(String[] args) 
    	{
    		Demo18.Inner5 i = new Demo18().new Inner5();
    		i.insert();
    		i.pop();
    
    		IUsb computer = new IUsb(){
    			public void insert(){
    				System.out.println("插入");
    			}
    			public void pop(){
    				System.out.println("弹出");
    			}
    		};
    		computer.insert();
    		computer.pop();
    
    		new IUsb(){
    			public void insert(){
    				System.out.println("插入1");
    			}
    			public void pop(){
    				System.out.println("弹出1");
    			}
    		}.insert();
    	}
    }
    
    interface IUsb
    {
    		void insert();
    		void pop();
    }


示例代码二：

    class Demo19 
    {
    	public static void main(String[] args) 
    	{
    		Person p = new Person(){
    			public void run(){
    				System.out.println("人跑步");
    			}	
    		};
    		p.run();
    
    		Person p1 = new Person(){
    			public void run(){
    				System.out.println("人飞奔");
    			}	
    		};
    		p1.run();
    
    
    		 new Person(){
    			public void run(){
    				System.out.println("散步");
    			}	
    		}.run();
    
    	}
    }
    abstract class Person{
    	public abstract void run();
    }

示例代码三（点击事件监听器的应用）：

    class Demo20 
    {
    	public static void main(String[] args) 
    	{
    		MyButton myBtn = new MyButton();
    		myBtn.isClick = true;
    		myBtn.setOnClickListener(new OnClickListener(){
    			public void onClick(){
    				System.out.println("点击了，可以登录！！");
    			}
    		});
    
    		MyButton myBtn1 = new MyButton();
    		myBtn1.isClick = false;
    		myBtn1.setOnClickListener(new OnClickListener(){
    			public void onClick(){
    				System.out.println("点击了，找回密码");
    			}
    		});
    	}
    }
    interface OnClickListener
    {
    		void onClick();
    }
    class MyButton
    {
    	boolean isClick;  //模拟点击
    		public void setOnClickListener(OnClickListener ocl){
    			if(isClick){
    				ocl.onClick();
    			}else{
    				System.out.println("没有点击");
    			}
    		}
    }
    


    