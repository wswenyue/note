##听课笔记
####名词解释
- JDK:java开发工具包
- JRE:java运行时环境
- JVM:java虚拟机
####java程序运行过程
javac  Demo01.java--->Demo01.class 字节码文件-->类加载器中-->运行java Demo01
####标识符、常量、变量
    标识符：字母  数字  _  $ 组成，其中数字不能开头，不能是关键字。
    类名：首字母大写
    变量名和方法名：驼峰命名法  myname  showinfo() addlist()
    常量名：全部大写字母组成
    包名：小写组成  域名倒置   com.baidu  com.wenyue
    常量：final 数据类型 常量名 = 值； 值不可以改变。
    变量：可以改变
    	 数据类型  变量名 = 值；

   >     final double PI;
    PI = 3.14;
    //PI = 3.23;  //常量只能赋值一次
    int $c = 100;
    System.out.println("$c="+$c);
    //进制：
    int  a = 010;
    System.out.println(a);
    int b = 0xa1;
    System.out.println(b);

####数据类型：
	基本数据类型：
		整型：byte（1字节 -128~127 Byte） short（2字节 -32768~32767 Short） int（4字节 -2147483648~2147483647  Integer） long（8字节  0L   Long）
		浮点型：float（4字节 Float 0.0f） double（8字节  Double）
		字符型：char（2字节  Character）
		布尔型：boolean  （true   false   Boolen）
	引用数据类型:
		类  class
		数组  array
		接口 interface

     byte b1 = 10;
    //byte b2 = 200;  越界

####类型转换
    数据类型转换：
    		自动类型转换：小--->大
    			byte,short,char-->int-->long-->float-->double
    			char可按照ASCII进行转换：a--》 97   A--》 65  0--》 48  
    		强制类型转换：大--》 小
    			（目标类型）变量
    			int a = 100;
    			byte b = (byte)a;
    byte b = 10;
    int a = 34;
    float f = 4.5f;
    short s = 45;
    double d = 10.5;
    double sum = b+a+f+s+d;//结果自动提升为double类型

####转义
      转义：有格式的输出。通过\改变后面跟随的字符的含义
    	\t:制表位
    	\n：换行
    	\\：斜杠
    	\"：双引号
    	\r\n:回车换行

#### 运算符
- 逻辑运算符: &&(逻辑与)  ||（逻辑或）  !（逻辑非）  &  |^异或
>
    		a&&b（短路）:如果a为false，则结果为false。b将不被判断
    		a&b:如果a为false，结果为false。b会进行判断。a与b都会执行。
    		a||b（短路）:如果a为true，则结果为true。b将不被判断
    		a|b:如果a为true，结果为true。b会进行判断。a与b都会执行。
    		a^b:相同为false，不同为true。

- 条件运算符： 三目运算符  ？ ：
>		条件表达式?语句1：语句2； //当表达式为true，结果为语句1，否则为语句2

- 字符串连接符 +
- 位运算符： &(按位与)  |（按位或）  ^（按位异或）   ~（按位取反）  >>（右移）  <<（左移）   >>>（无符号右移）
- 进制转换：
>			十进制---》二进制 : 除2取余  10--》1010
			二进制 --》十进制  ：位权 * 符号^(n-1)
			(1010)二---->（0*2^0+1*2^1+0*2^2+1*2^3） =10
			八进制---》二进制 
				1)通过十进制转换
				2) 773 八--> 111 111 011  二
    int a = 4, b =3;
    int c = a++ + ++b + ++a; // c = 4 + 4 + 6
    int d = --b + ++a/2 - b++%2; // d = 3 + 7/2 – 3%2 
    System.out.println("a=" + a + ",b=" + b + ",c=" + c + ",d=" + d);  //a=7; b=4; c=10; d=5
    
    System.out.println(5&3); //1
    System.out.println(5|3); //7
    System.out.println(5^3);  //6
			5 : 0000 0101
			3 : 0000 0011
			
			0101
		  & 0011
		    0001   //1

			0101
	      | 0011
		    0111   //7

			0101
		  ^ 0011
		    0110   //6
    System.out.println(~5);
    /*
       ~5 = -6
       0000 0101
       1111 1010  (取反)-->  -1 1111 1001  -->取反 0000 0110
       每一位数取反，0即为1,1即为0.
       如果出现负数，就按照负数的方式进行转换
       如果正数取反，+1 ，得到真实的数
    
       正数：原码 --》3  11
       负数:  -3
      原码： 0000 0011
      反码:  1111 1100
      +1 1111 1101
    */
    //二进制输出使用：Integer.toBinaryString()
    System.out.println(Integer.toBinaryString(10));
       System.out.println(Integer.toBinaryString(-5));
    /*
       -5：
       5的原码：  0000 0101
       5的补码：  1111 1010
     +1:  1111 1011
    负数的二进制 就是正数的补码+1
    
    9二进制：0000 0000 0000 0000 0000 0000 0000 1001
    -9二进制：
    9 原码：0000 0000 0000 0000 0000 0000 0000 1001
    9 反码：1111 1111 1111 1111 1111 1111 1111 0110
      +11111 1111 1111 1111 1111 1111 1111 0111
    
    */
    System.out.println(Integer.toBinaryString(-9));
    Integer.toBinaryString(a) :二进制
    Integer.toHexString(a):十六进制
    Integer.toOctalString(a):八进制
    Integer.MAX_VALUE :int类型的最大值
    Integer.MIN_VALUE:int类型最小值
    
    十六进制转十进制
    Integer.valueOf(a+"",16)
    八进制转十进制
    Integer.valueOf(a+"",8)
    二进制转十进制
    Integer.valueOf(a+"",2)
    位操作举例：
    System.out.println(6>>2);//6*1/4=1
    System.out.println(6<<2);//6*4=24
    System.out.println("-6>>2="+(-6>>2));//-6*1/4=-2
    System.out.println("-13>>2="+(-13>>2));//-14*1/4=-4
    System.out.println("-12>>2="+(-12>>2));//-12*1/4=-3
    System.out.println("-6/4="+(-6/4));//-6*1/4=-1
    System.out.println("-1/4="+(-1/4));//-1*1/4=0
    System.out.println("-6>>>2="+(-6>>>2));//21073741822

**最有效的方式计算2\*8  = 2 \* 2^3**

`	System.out.println(2<<3);`
    
进行两个数互换
    
    int a = 5 , b = 10;
    //第一种方法
    int c = a;
    a = b ;
    b = c;
    // 第二种方法
    a = a+b;  //可能溢出
    b = a - b;
    a = a - b;
    //第三种方法（优先使用）
    a = a ^ b ;
    b = a^ b;//a ^ b ^ b
    a = a ^ b;  //a ^ b ^ a
    System.out.println("a="+a+",b="+b);

---
有三个数，获得最大值
	
		int  x = 10 , y = 20 , c = 5;
		int t = x > y ? x : y;
		int max = t > c ? t : c;
		System.out.println("max="+max);
		*/

---
将一个十进制转换为十六进制（位运算符）
		
		//思路：
		/*
			1 转换成二进制，取第四位，判断是不是大于10，如果大于10，则转换成a~f
			2 右移四位，再重复第一步，直到值为0时，中断
		*/
		int num  = 30;   //0001 1110
		int n = num & 15;//0000 1110   低四位
		num = num >> 4 ;//取下一个四位数
		char c = (char)(n - 10 + 'a');
		System.out.println("0x"+num+c);

---
判断闰年

		int year = input.nextInt();
		if(year %4 ==0 && year %100!=0 || year % 400==0){
			System.out.println("闰年");
		}
		else{
			System.out.println("平年");
		}

---
产生随机数

    double Math.random()--->0-1
	//Math.random()*(大数-小数+1)+小数 //(小数，大数)范围的数

---
九九乘法表

    int i = 1;
		while(i<=9){
			int j = 1;
			do{
				System.out.print(j+"*"+i+"="+i*j+"\t");
				j++;
			}while(j<=i);
			System.out.println();
			i++;
		}

---
菱形

		/*
				*
			   ***
			  *****
			 *******
			  *****
			   ***
			    *
		
		*/
		for(int i = 1;i<=4;i++){ //行
			//空格
			for(int j=1;j<=4-i;j++){
				System.out.print(" ");
			}
			//变量作用域：从声明的位置开始，到它所在的结构结束为止
			//星号
			for(int j=1;j<= 2*i-1;j++){
				System.out.print("*");
			}
				//换行
			System.out.println();
		}
		for(int i = 1 ; i<=3 ;i++){
			//空格
			for(int j=1;j<=i;j++){
				System.out.print(" ");
			}
			//星号
			for(int j=1;j<=7-2*i;j++){
				System.out.print("*");
			}
			//换行
			System.out.println();
		}

---
空心菱形

     /*
		   *
		  * *
		 *   *
		*     *
		 *   *
		  * *
		   *
     */
       for(int i = 1;i<=4;i++){ //行
			//空格
			for(int j=1;j<=4-i;j++){
				System.out.print(" ");
			}
			//变量作用域：从声明的位置开始，到它所在的结构结束为止
			//星号
			for(int j=1;j<= 2*i-1;j++){
				if(j==1 || j==2*i-1)
					System.out.print("*");
				else
					System.out.print(" ");
			}
				//换行
			System.out.println();
		}
		for(int i = 1 ; i<=3 ;i++){
			//空格
			for(int j=1;j<=i;j++){
				System.out.print(" ");
			}
			//星号
			for(int j=1;j<=7-2*i;j++){
				if(j==1 || j==7-2*i)
					System.out.print("*");
				else
					System.out.print(" ");
			}
			//换行
			System.out.println();
		}

---
####switch 结构
    switch(表达式){ //byte short int char 枚举（jdk1.5 enum） String（jdk1.7）
    	case 常量1：
    		语句1；
    		break；
    	case 常量2：
    		语句2；
    		break；
    	...
    	default：
    		语句n;
    		break;
    }

注意：

	1 break可有可无
	2 case后面的常量值一定不能相同。
	3 表达式类型
	4 case后的语句可以省略
	switch(表达式){
		case  常量1：
		case 常量2：
		case 常量3：
			语句1；
			break；

	}

----
- for、while、do-while何时采用？

>已知循环次数：首选for
循环次数未知：首选while、do-while
如果循环结束后，循环的变量不需要继续使用，可以for

- if和switch何时采用？

>区间判断：if
等值判断：switch