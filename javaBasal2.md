####方法相关

- 何时采用参数和返回值？

		1 确定函数有没有结果：即确定是否有返回值
			有结果：有返回值
			没有结果：没有返回值
		2 确定函数在执行过程中，是否用到了不确定的数据：即确定参数
			用到了不确定的数据：有参数
			没用到不确定的数据：无参


- 没有返回值方法，也可以使用return

	    public static void t1(int a){
    		if(a>0){
    			return;
    		}
    		else{
    			System.out.println("a<=0");
    		}
    	}


> 方法调用：入栈，分配空间，初始化局部变量  

>方法的返回:出栈，释放空间，在方法中定义的局部变量消失了

####递归

求阶乘

    public static int f(int n){
    	if(n == 1)
			return 1;  //出口
		else 
			return n * f(n-1);  //逐步逼近出口
    }

---
斐波那契数列：1 1 2 3 5 8 13 21 34 55  

    public static int f(int n){
		if(n==1 || n==2){
			return 1;
		}
		else{
			return f(n-1) +f(n-2);
		}
	}

---
#### 数组问题
数组下标必须是 0 ~ arr.length - 1 .否则会出现
**`ArrayIndexOutOfBoundsException`**

System提供的数组拷贝工具使用：

**`arraycopy(Object src,int srcPos,Object dest,int destPos,int length)`**

	src:源数组
	srcPos:从源数组的那个下标开始拷贝
	dest：目标数组
	destPos:目标数组的复制的位置
	length：复制的数组元素的长度

	注意：下标越界

`java.util.Arrays`工具包的使用

			String toString（[]）:"[1,2,3,4]"
			void fill([],num):用num填充数组
			boolean equals([],[]):两个数组的元素一一比较
			void sort（[]）:排序
			int binarySearch（[],key）：二分法查找
			[] copyOf（[],newLength）:数组拷贝
	
示例：

    int[] a={2,3,4,6,1,23,45};
	//将数组转换成字符串  toString([])
	System.out.println(Arrays.toString(a));
	//排序 sort([])
	Arrays.sort(a);
	System.out.println("排序后的数组:");
	System.out.println(Arrays.toString(a));
	int[] b = new int[10];
	System.out.println(Arrays.toString(b));
	//用指定的数值填充数组b  fill([],num)
	Arrays.fill(b,5);
	System.out.println(Arrays.toString(b));
	int[] c = {1,2,3,4,5};
	int[] d = {1,2,3,5,4};
	//判断数组是否相等  equals([],[])
	System.out.println("数组c和d是否相等？"+Arrays.equals(c,d));
	//二分法查找 binarySearch([],key)
	int[] e = {10,20,30,1,2,-4};
	Arrays.sort(e);
	System.out.println(Arrays.binarySearch(e,-10));//如果是负数，没找到
	//数组的复制   [] copyOf([],newLength)
	int[] f = {1,2,3,4,5,6,7,8,9};
	int[] newF = Arrays.copyOf(f,20);
	System.out.println(Arrays.toString(newF));


####JVM
JVM内存一般分**五个区域**：

		方法区  堆  栈 本地方法区  寄存器
		方法区：存类的信息，常量池，静态方法区
		栈（值类型）：存放调用方法的局部变量
			存储在栈中的变量，作用域结束立即消失
		堆（引用类型）：存数组或者引用对象
			特点：
				1 分配内存首地址
				2 有默认值
				3 gc（垃圾回收）
		本地方法区：实现类库的调用

		注意：在常量池中，java默认创建-128-127之间的常量对象
		对于字符串常量会首先去常量池查找，如果不存在就创建字符串常量
	

**值类型**和**引用类型参数**的区别：两个数的互换
	
**如果参数为值类型：传递的是值的副本，形参不能改变实参的值**  

**如果参数类型为引用类型：传递的是地址，形参可以改变实参的值**

---

- 字符串常量对象:保存在字符串常量池中
    
        public static void main(String[] args) 
    	{
	    	String str = "你好";
	    	String str1 = "你好";
	    	System.out.println(str==str1);  //结果为true
	    	show(str);//将str变量的内容“你好”传递给方法
	    	System.out.println(str);  //你好
	    	//下面这条语句创建了几个对象？此时只会创建一个对象，因为”你好“已经在常量池中存在
	    	String str3 = new String("你好");
	       	String str4 = str3;
	    	str3 = "hi";  //“hi”字符串在常量池不存在，则会在常量池中创建一个对象，str3就指向该对象
	    	System.out.println(str4); //你好
    	}
       	static  void  show(String str){
    		str = "hello";  //方法中重新赋值
		}

####可变参数：jdk5.0新增

	语法：
		返回类型  方法名（数据类型...参数名）{
		
		}
	用途：可以动态接受0个或者多个实参，主要应用于方法参数个数不固定
		根据实参的个数，jvm把它处理成数组

可变参数：**参数数量可变，类型不能变**

	public static int getSum(int...nums){
		int sum = 0 ;
		for(int i=0;i<nums.length;i++){
			sum += nums[i] ;
		}
		return sum;
	}

	