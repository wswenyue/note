#SQL 结构化的查询语言 Structured Query Language

------------
###分类
- DDL 数据定义语言: `create 、drop 、 alter`
- DML：数据操作语言: `CURD：增删改查`
- DCL：数据控制语言
- 数据查询语言（DQL:Data Query Language）
- 指针控制语言（CCL）
- 事务处理语言（TPL）

--------------
##SQLite常用语法（只记录SQLite独特的部分）

**sqlite： 以`.`开头以`;`结尾**

	sqlite>.help 	//输出帮助信息
    sqlite>.open xxx.db //创建数据库
	sqlite>.header on	//显示标题
	sqlite>.mode column	//设置显示模式(默认为list，设置为column，其他模式可通过.help查看mode相关内容)
	退出sqlite终端命令：
		sqlite>.quit
		或
		sqlite>.exit
	sqlite>.show	//列出当前显示格式的配置：
	sqlite>.schema table_name	//查看指定表的创建语句
	sqlite>.schema	//查看所有表的创建语句
	sqlite>.database	//查看数据库文件信息命令
	sqlite>.dump table_name	//以sql语句的形式列出表内容
	sqlite>.separator symble	//设置显示信息的分隔符
	sqlite>.width width_value	//设置每一列的显示宽度


**在SQLite中需要注意**：
设置外键约束之后要启用外键约束（启用之前插入的不合法的数据，是被允许的，所以要谨慎，这可能是一个bug）

启用外键语法：

	pragma foreign_keys=on;

**SQLite支持哪些数据类型**

	NULL 值为NULL
	INTEGER 值为带符号的整型，根据类别用1，2，3，4，6，8字节存储
	REAL 值为浮点型，8字节存储
	TEXT 值为text字符串，使用数据库编码(UTF-8, UTF-16BE or UTF-16-LE)存储
	BLOB 值为二进制数据，具体看实际输入
	
	但实际上，sqlite3也接受如下的数据类型：
	smallint  16 位元的整数
	interger  32 位元的整数
	decimal(p,s)  p 精确值和 s 大小的十进位整数，精确值p是指全部有几个数(digits)大小值 ，s是指小数点後有几位数。如果没有特别指定，则系统会设为 p=5; s=0 。
	float   32位元的实数。
	double   64位元的实数。
	char(n)   n 长度的字串，n不能超过 254。
	varchar(n)  长度不固定且其最大长度为 n 的字串，n不能超过 4000。
	graphic(n)  和 char(n) 一样，不过其单位是两个字元 double-bytes， n不能超过127。这个形态是为了支援两个字元长度的字体，例如中文字。
	vargraphic(n)  可变长度且其最大长度为 n 的双字元字串，n不能超过 2000。
	date   包含了 年份、月份、日期。
	time   包含了 小时、分钟、秒。
	timestamp  包含了 年、月、日、时、分、秒、千分之一秒。

	实际上：SQLite是无类型的. 这意味着你可以保存任何类型的数据到你所想要保存的任何表的任何列中, 
	无论这列声明的数据类型是什么(只有自动递增Integer Primary Key才有用). 对于SQLite来说对字段不指定类型是完全有效的.

--------------
##SQL语法
###创建表以及增、删、改、查

    create table 表名（
						字段名 数据类型 说明 ,
						...	    ...	  ... ,
						...     ...   ...
					  ）;

外键约束：

    字段名 references 主表（主键）

示例：

    create table user( 
	 	uid int primary key auto_increment, //主键自增
	 	uname varchar(40) UNIQUE NOT NULL,	//唯一且不为空
	 	password varchar(40), 
	 	email varchar(60),
	 	phone varchar(20) UNIQUE NOT NULL,
	 	balance int(10) NOT NULL DEFAULT 999,//不不空且默认值是999
	 	 );

外键约束示例：
    
    create table type(
    	tid Integer primary key autoincrement,  //注意把autoincrement写到后面，放到前面可能会不起作用 并且只能用Integer类型（在SQLite中）
    	tname varchar(20)
    	 );
    
    create table goods(
    	id Integer  primary key autoincrement,
    	name varchar(20) not null,
    	num int check(name>=0),	//check约束
    	price int,
    	address varchar(50),
    	tid references type(tid)	//外键约束：goods表中的tid字段依赖于type表中的tid字段
    )


约束条件：

	主键约束：primary key
	外键约束：foreigh key
	唯一约束：unique
	检查约束：check（表达式）
	默认约束：default(默认值)
	非空约束：not null


增删改查语法：

	C(Create 增加)：
		insert into 表名 （字段名） values（值）[,(值),(值)]
	R(Retrieve 查询)
		select 字段名 from 表名
		[where 条件]
		[group by 分组条件]
		[having 筛选条件]
		[order by 排序字段[asc|desc]]
		[limit n]
			limit 2 前两位
			limit 3,4 跳过3条后的再显示4条
	U(Update 修改)
		update 表名  set 字段=值[,字段=值] [where 条件]
	D(Delete 删除)
		delete from 表名[where 条件]


where 条件：
`>`、 `< `、 `=` 、`!=` 、`>=` 、`<=` 、`<> (注：不等于)`

`not` `and` `or`

`in(值，值，值)`

`between ... and ...`

模糊查询：
`%` **：任意多个字符**
`_` ** : 任意一个字符**

###表链接
**注：能用链接查询尽量不要用嵌套子查询（效率问题）**

分类：

	内连接：两张表中同时存在的记录
			inner join //可以简写成 join
	外连接：
			左外连接：以A表作为主表，主表的信息会全部显示，如果B表有对应的数据显示数据，否则显示null
			A left join B
			
			右外连接：A right join B	<=等价于=> B left join A 	//(sqlite不支持)
			全连接：A full join B  <=等价于=>   左外+右外	//(sqlite不支持)

语法：

	select 字段 
	from 表名1  join 表名2
	on 表1.键 = 表2.键
		
扩展：自链接（自己连接自己）
示例：

	type表：
	id tname   pid
	1  食品      0
	2  饼干      1
	3  饮料      1
	4  可乐      3
	5  可口可乐   4
	6  百事      4
	7  家电      0
	8  冰箱      7

需要解决的问题
现要求查询显示如下：

	编号 类别名称  父类名称
	2  饼干      食品
	3  饮料      食品
	4  可乐      饮料
	5  可口可乐   可乐
	6  百事      可乐	
	8  冰箱      家电

解决（使用自链接）
查询语句如下：

	select a.id as 编号  a.tname as 类别名称 ,b.tname as 父类名称
	from type as a join type as b
	on a.pid = b.id

###查询语句练习
现有如下表：

	sqlite> select * from goods;
	id          name        num         address     tid
	----------  ----------  ----------  ----------  ----------
	1           bb          20          beijing     1
	2           asdf        40          beijing     2
	3           nljksdff    40          shanghai    2
	4           sdff        100         shanghai    1

	sqlite> select * from type;
	tid         tname
	----------  ----------
	1           玩具
	2           服装
	3           饮料

查询：
    
	//按照address分组，并显示各组的总数（条目）
    sqlite> select address,count(*) from goods group by address;

	address     count(*)
	----------  ----------
	beijing     2
	shanghai    2

	//按照address分组，查询各地的产品数量和总量
	sqlite> select address,count(*) as 产品数量,sum(num) as 总量 from goods g
	 address;

	address     产品数量    总量
	----------  ----------  ----------
	beijing     2           60
	shanghai    2           140

	//按照address分组，查询各地的产品数量和总量，并筛选总量大于100的
	sqlite> select address,count(*) as 产品数量,sum(num) as 总量 from goods g
	 address having sum(num)>100;

	address     产品数量    总量
	----------  ----------  ----------
	shanghai    2           140

	//按照产品数量降序排列，并取得前两条数据
	sqlite> select * from goods order by num desc limit 2;

	id          name        num         address     tid
	----------  ----------  ----------  ----------  ----------
	4           sdff        100         shanghai    1
	2           asdf        40          beijing     2

	//按照产品数量降序排列，并跳过结果集中的第一条数据，再显示两条数据
	sqlite> select * from goods order by num desc limit 1,2;

	id          name        num         address     tid
	----------  ----------  ----------  ----------  ----------
	2           asdf        40          beijing     2
	3           nljksdff    40          shanghai    2

	//查询商品地址在'beijing'或'shanghai'中的商品信息
	sqlite> select * from goods where address in('beijing','shanghai');

	id          name        num         address     tid
	----------  ----------  ----------  ----------  ----------
	1           bb          20          beijing     1
	2           asdf        40          beijing     2
	3           nljksdff    40          shanghai    2
	4           sdff        100         shanghai    1

	//查询商品表中商品数量最大的商品信息
	sqlite> select * from goods where num = (select max(num) from goods);
	id          name        num         address     tid
	----------  ----------  ----------  ----------  ----------
	4           sdff        100         shanghai    1

	//查询goods表中name中包含f的所有商品类别信息
	sqlite> select * from type where tid in(select tid from goods where name like %f%');

	tid         tname
	----------  ----------
	1           玩具
	2           服装
	
	//查询goods中不存在的商品类别信息
	sqlite> select * from type where tid not in(select tid from goods);

	tid         tname
	----------  ----------
	3           饮料

	//先按照tid对goods分组，计算各组的num的平均值，列出平均值小于100的tid
	sqlite> select tid from goods group by tid having avg(num)<100;

	tid
	----------
	1
	2

	//先按照tid对goods分组，计算各组的num的平均值，获取平均值小于100的tid并显示其类别信息
	sqlite> select * from type where tid in (select tid from goods group by tid having avg(num)<100);

	tid         tname
	----------  ----------
	1           玩具
	2           服装
	
	//goods与type 内链接
	sqlite> select type.*,goods.* from type join goods on type.tid = goods.tid;

	tid         tname       id          name        num         address     tid
	
	----------  ----------  ----------  ----------  ----------  ----------  -
	--
	1           玩具        1           bb          20          beijing     1
	
	2           服装        2           asdf        40          beijing     2
	
	2           服装        3           nljksdff    40          shanghai    2
	
	1           玩具        4           sdff        100         shanghai    1
	
	//goods与type 内链接
	sqlite> select goods.*,tname from type join goods on type.tid = goods.tid;

	id          name        num         address     tid         tname
	----------  ----------  ----------  ----------  ----------  ----------
	1           bb          20          beijing     1           玩具
	2           asdf        40          beijing     2           服装
	3           nljksdff    40          shanghai    2           服装
	4           sdff        100         shanghai    1           玩具

	//type与goods 左链接
	sqlite> select goods.*,tname from type left join goods on type.tid = goods.tid;

	id          name        num         address     tid         tname
	----------  ----------  ----------  ----------  ----------  ----------
	1           bb          20          beijing     1           玩具
	4           sdff        100         shanghai    1           玩具
	2           asdf        40          beijing     2           服装
	3           nljksdff    40          shanghai    2           服装
	                                                            饮料
	//goods与type 左链接 相当于 type与goods 右链接
	sqlite> select goods.*,tname from goods left join type on type.tid = goods.tid;

	id          name        num         address     tid         tname
	----------  ----------  ----------  ----------  ----------  ----------
	1           bb          20          beijing     1           玩具
	2           asdf        40          beijing     2           服装
	3           nljksdff    40          shanghai    2           服装
	4           sdff        100         shanghai    1           玩具
	
	//自链接（这个没啥意义）
	sqlite> select a.*,b.* from type as a join type as b on a.tid = b.tid;

	tid         tname       tid         tname
	----------  ----------  ----------  ----------
	1           玩具        1           玩具
	2           服装        2           服装
	3           饮料        3           饮料

    
    查询男女的人数
    Select sex,count(*)
    From student
    Group by sex;
    男同学的平均年龄比女同学的平均年龄大几岁？
    select (select avg(age) from student where sex = '男')-(select avg(age)
    from student where sex='女') as 最美年龄差;
    
    查询和学号是2的同名的同学
    Select * from student
    Where name=(select name from student where id=2)
    
    查询年龄第二大的女生
    Select * from student where sex = ‘女’ and age=(select max(age) from student 
    Where age <(select max(age) from student)
      )
    
    查询年龄最大的同学
    Select * from student order by age desc limit 1;
    该sql语句不能把所有年龄最大的同学全部包含,如果最大的年龄的同学不唯一
    Select * from student where age =(select max(age) from student)
    
    Select * from stduent where age=(select age from student order by age desc limit 1)
    
    查询年龄最大的男女同学
    Select max(age) from student group by sex
    
    男女最大年龄差
    Select ((select max(age)from student where sex=’男’)-(select min(age) from student where sex=’女’)) as 最大年龄差
    

	
---------------
