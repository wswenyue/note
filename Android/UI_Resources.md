#UI资源
##color资源：
使用方法：
###drawable的color资源
	
	1､ 在drawable目录下创建.xml文件，根标签为<color>
	2、在<color>标签中使用其android:color属性设置颜色值（如：#RGB或@color/xxx)
	3、一般作为背景使用，引用方式：@drawable/xxx

###values的color常量
在values目录下创建`colors.xml`文件
在`<resources>`标签中定义颜色：

	<resources>   
		        <color name="white">#ffffff</color><!--白色 -->   
		        <color name="ivory">#fffff0</color><!--象牙色 -->   
	</resources>  

**引用方式**：`@color/xxx`
例如：
设置字体的颜色

    android:textColor="@color/white"

设置布局的背景颜色

    android:background="@color/white"	

##selector背景选择器
作用：根据UI控件的不同状态选择相应的颜色资源作为控件的背景

常见状态：
	
	android:drawable 默认
	android:state_window_focused="false|true" UI所在窗口是否获得焦点的状态
	android:state_focused="false|true" 是否获得焦点，一般与按下状态组件使用
	android:state_checked="false|true" 是否勾选的状态
	android:state_selected="false|true" 是否选择的状态
	android:state_pressed="false|true" 点击或按下的状态
	android:state_enabled＝“false|true“ 是设置是否响应事件,指所有事件

示例：

	<?xml version="1.0" encoding="utf-8" ?>
	
	<selector xmlns:android="http://schemas.android.com/apk/res/android">
	
	    <!-- 默认时的背景图片-->
	    <item android:drawable="@drawable/p1" />
	
	    <!-- 没有焦点时的背景图片-->
	    <item android:state_window_focused="false" android:drawable="@drawable/p1" />
	
	    <!-- 非触摸模式下获得焦点并单击时的背景图片-->
	    <item android:state_focused="true" android:state_pressed="true"
	
	        android:drawable="@drawable/p2" />
	
	    <!-- 触摸模式下单击时的背景图片-->
	    <item android:state_focused="false" android:state_pressed="true"
	        android:drawable="@drawable/p3" />
	
	    <!--选中时的图片背景-->
	    <item android:state_selected="true" android:drawable="@drawable/p4" />
	
	    <!--获得焦点时的图片背景-->
	    <item android:state_focused="true" android:drawable="@drawable/p5" />
	
	</selector>

用法:
	
	1､ 在drawable目录下创建.xml文件，根标签为<selector>
	2､ 增加<item>标签，并设置不同状态和对应的资源
	3、在UI控件的background属性引用,属性值：@drawable/xx


##shape图形资源
**作用：** `XML` 中定义的几何形状
**位置：** `res/drawable/xxx.xml`

**根标签** `<shape>` 中的 `android:shape属性值`
	
	rectangle 矩形
	oval 椭圆
	line 水平直线
	ring 环形

`<gradient>` 渐变子标签

	android:startColor  起始颜色
	android:centerColor 中间颜色
	android:endColor  结束颜色   
	android:angle  渐变角度，0从左到右，90表示从下到上，数值为45的整数倍数，默认为0
	android:type  渐变的样式 liner线性渐变 radial环形渐变 sweep 扫视渐变
      
`<solid>`  填充颜色子标签

	android:color  填充的颜色

`<stroke>` 描边子标签

	android:width 描边的宽度
	android:color 描边的颜色
	android:dashWidth 表示'-' 破折号的宽度
	android:dashGap 表示'-' 破折号之间的距离

`<corners>` 圆角子标签

	android:radius  圆角的半径 值越大角越圆
	android:topRightRadius  右上圆角半径
	android:bottomLeftRadius 右下圆角角半径
	android:topLeftRadius 左上圆角半径
	android:bottomRightRadius 左下圆角半径

`<padding>`内边距子标签

	android:bottom 底部填充
	android:left 左边填充
	android:right 右边填充
	android:top 上面填充

圆角按钮示例：

	<?xml version="1.0" encoding="utf-8"?>
	<selector xmlns:android="http://schemas.android.com/apk/res/android" >
	   
	    <!-- 触摸方式按下按钮 -->
	     <item android:state_focused="false" android:state_pressed="true">
	       	<shape android:shape="rectangle">
	       	    <corners android:radius="10dp"/>
	       	    <solid android:color="#f00"/>
	       	</shape>
	    </item>
	
	    <item>
	         <shape android:shape="rectangle">
	       	    <corners android:radius="10dp"/>
	       	    <solid android:color="#00f"/>
	       	    <gradient android:startColor="#00f" android:centerColor="#fff" android:endColor="#00f"/>
	       	    
	       	    <!-- 如果设置填充颜色，则渐变色将失效 -->
	       	 	<solid android:color="#ffff"/>
	       	  	<size android:width="10dp" android:height="10dp" />
	       	  	<stroke android:color="#000" android:width="1dp"/>
	       	</shape>
	    </item>
	   
	   
	</selector>

##layer-list 图层资源
###作用： 将多个图片或图形按层关系排放，类似帧布局
###用法同selector，不过根标签为<layer-list>,必须指定id和drawable属性

实例：水平进度条：

	－－－必须提供一套进度图片（三张）－－－
	
	
	－－－图层资源文件－－－
	<?xml version="1.0" encoding="utf-8"?>
	<layer-list xmlns:android="http://schemas.android.com/apk/res/android" android:opacity="opaque">
	    
	    <!-- 进度条的背景，必须放在此处，即图层的最底部 -->
	    <item android:id="@android:id/background" android:drawable="@drawable/progress_bg"/>
	    
	    <!-- 第二进度值，在图层的中部 -->
	    <item android:id="@android:id/secondaryProgress" android:drawable="@drawable/progress_second"/>
	    
	    <!-- 当前进度值，在图层的最顶部 -->
	    <item android:id="@android:id/progress" android:drawable="@drawable/progress"/>
	
	</layer-list>
	
	
	----布局文件－－－－
	<ProgressBar 
	        style="?android:attr/progressBarStyleHorizontal"
	        android:id="@+id/progressId"
	        android:layout_width="match_parent"
	        android:layout_height="40dp" 
	        android:progressDrawable="@drawable/progress_bar"
	        android:layout_below="@id/listView"
	        />

##bitmap资源
###作用：防止小图片作为背景使用时被拉伸
###位置：`/res/drawable/*.xml`，根标签为`bitmap`
###常用属性

####src 图片资源位置
####tileMode 平铺模式

	disabled 不平铺，显示原大小
	clamp 固定的重复边缘颜色
	repeat 重复（双向平铺）
	mirror 反射平铺（镜面映照）

####gravity 原大小显示时的对齐方式

####antialias 是否抗锯齿

##大小(Size)资源
在values文件下新建dimens.xml文件
内容格式如下：
	
	<resources>
	
	    <!-- Default screen margins, per the Android Design guidelines. -->
	    <dimen name="activity_horizontal_margin">16dp</dimen>
	    <dimen name="activity_vertical_margin">16dp</dimen>
	
	    
	    <!-- 定义字体的大小 -->
	    <dimen name="fontSize">20sp</dimen>
	    <dimen name="smallFontSize">15sp</dimen>
	    <dimen name="largeFontSize">30sp</dimen>
	    
	    <dimen name="textPadding">10dp</dimen>
	    
	</resources>


##ID资源
在values文件下新建ids.xml文件
内容格式如下：

	<?xml version="1.0" encoding="utf-8"?>
	<resources>
	    <!-- Item布局中子控件使用的id资源 -->
	    <item type="id" name="titleId"/>
	    <item type="id" name="progressBarId"/>
	    <item type="id" name="nameId"/>
	    <item type="id" name="changedId"/>
	    
	    <!-- 一般AdapterView控件使用的id资源 -->
	    <item type="id" name="viewPagerId"/>
	    <item type="id" name="listViewId"/>
	    
	    <!-- 按钮控件中使用的id资源 -->
	    <item type="id" name="submitBtnId"/>
	    <item type="id" name="loginBtnId"/>
	    <item type="id" name="registBtnId"/>
	    
	    
	</resources>

##style样式
###作用：android中的样式和CSS样式作用相似，都是用于为界面元素定义显示风格，其包含一个或者多个view控件属性的集合。如：需要定义字体的颜色和大小
###位置：res/values/styles.xml
###定义样式：
	
	 <?xml version="1.0" encoding="utf-8"?>
	<resources>
	    <style name="CodeFont" parent="@android:style/TextAppearance.Medium">
	        <item name="android:layout_width">fill_parent</item>
	        <item name="android:layout_height">wrap_content</item>
	        <item name="android:textColor">#00FF00</item>
	        <item name="android:typeface">monospace</item>
	    </style>
	</resources>
	
	
	--参考系统的style.xml,路径：
	adt_bundle/sdk/platforms/android-19/data/res/values/styles.xml
	
	--参考系统的themes.xml，路径：
	adt_bundle/sdk/platforms/android-19/data/res/values/themes.xml

###引用样式：在指定UI控件的style属性引用指定样式
	
	引用样式前：
	<TextView
	    android:layout_width="fill_parent"
	    android:layout_height="wrap_content"
	    android:textColor="#00FF00"
	    android:typeface="monospace"
	    android:text="@string/hello" />
	
	
	引用样式后：
	<TextView
	    style="@style/CodeFont"
	    android:text="@string/hello" />

###样式的继承
####parent属性方式

	<resources>
	    <style name="CodeFont" parent="@android:style/TextAppearance.Medium">
	        <item name="android:layout_width">fill_parent</item>
	        <item name="android:layout_height">wrap_content</item>
	        <item name="android:textColor">#00FF00</item>
	        <item name="android:typeface">monospace</item>
	    </style>
	</resources>


####前辍方式
	
	<style name="CodeFont.Red">
	        <item name="android:textColor">#FF0000</item>
	    </style>
	
	<style name="CodeFont.Red.Big">
	        <item name="android:textSize">30sp</item>
	 </style>



##theme 主题

###作用：主要是为了让应用中所有UI控件的样式保持一致，如所有界面的字体大小、颜色等

###<application android:theme="@style/CustomTheme"> 应用引用样式
###<activity android:theme="@android:style/Theme.Dialog"> activity引用样式

###常用的系统主题

	android:style/Theme.Holo.Light 白底黑字
	android:style/Theme.Holo 黑底白字
	android:style/Theme.NoTitleBar.Fullscreen 无标题栏且全屏


###消除应用启动闪黑屏

	<!--1、消除应用启动闪黑屏 -->
	 	<style name="Theme.splash" parent="@android:style/Theme.NoTitleBar.Fullscreen">
	        <item name="android:windowIsTranslucent">true</item>
	    </style>


------------------------

常见颜色示例：
res/values/colors.xml：

	<?xml version="1.0" encoding="utf-8" ?>   
	<resources>   
	        <color name="white">#ffffff</color><!--白色 -->   
	        <color name="ivory">#fffff0</color><!--象牙色 -->   
	        <color name="lightyellow">#ffffe0</color><!--亮黄色 -->   
	        <color name="yellow">#ffff00</color><!--黄色 -->   
	        <color name="snow">#fffafa</color><!--雪白色 -->   
	        <color name="floralwhite">#fffaf0</color><!--花白色 -->   
	        <color name="lemonchiffon">#fffacd</color><!--柠檬绸色 -->   
	        <color name="cornsilk">#fff8dc</color><!--米绸色 -->   
	        <color name="seaShell">#fff5ee</color><!--海贝色 -->   
	        <color name="lavenderblush">#fff0f5</color><!--淡紫红 -->   
	        <color name="papayawhip">#ffefd5</color><!--番木色 -->   
	        <color name="blanchedalmond">#ffebcd</color><!--白杏色 -->   
	        <color name="mistyrose">#ffe4e1</color><!--浅玫瑰色 -->   
	        <color name="bisque">#ffe4c4</color><!--桔黄色 -->   
	        <color name="moccasin">#ffe4b5</color><!--鹿皮色 -->   
	        <color name="navajowhite">#ffdead</color><!--纳瓦白 -->   
	        <color name="peachpuff">#ffdab9</color><!--桃色 -->   
	        <color name="gold">#ffd700</color><!--金色 -->   
	        <color name="pink">#ffc0cb</color><!--粉红色 -->   
	        <color name="lightpink">#ffb6c1</color><!--亮粉红色 -->   
	        <color name="orange">#ffa500</color><!--橙色 -->   
	        <color name="lightsalmon">#ffa07a</color><!--亮肉色 -->   
	        <color name="darkorange">#ff8c00</color><!--暗桔黄色 -->   
	        <color name="coral">#ff7f50</color><!--珊瑚色 -->   
	        <color name="hotpink">#ff69b4</color><!--热粉红色 -->   
	        <color name="tomato">#ff6347</color><!--西红柿色 -->   
	        <color name="orangered">#ff4500</color><!--红橙色 -->   
	        <color name="deeppink">#ff1493</color><!--深粉红色 -->   
	        <color name="fuchsia">#ff00ff</color><!--紫红色 -->   
	        <color name="magenta">#ff00ff</color><!--红紫色 -->   
	        <color name="red">#ff0000</color><!--红色 -->   
	        <color name="oldlace">#fdf5e6</color><!--老花色 -->   
	        <color name="lightgoldenrodyellow">#fafad2</color><!--亮金黄色 -->   
	        <color name="linen">#faf0e6</color><!--亚麻色 -->   
	        <color name="antiquewhite">#faebd7</color><!--古董白 -->   
	        <color name="salmon">#fa8072</color><!--鲜肉色 -->   
	        <color name="ghostwhite">#f8f8ff</color><!--幽灵白 -->   
	        <color name="mintcream">#f5fffa</color><!--薄荷色 -->   
	        <color name="whitesmoke">#f5f5f5</color><!--烟白色 -->   
	        <color name="beige">#f5f5dc</color><!--米色 -->   
	        <color name="wheat">#f5deb3</color><!--浅黄色 -->   
	        <color name="sandybrown">#f4a460</color><!--沙褐色 -->   
	        <color name="azure">#f0ffff</color><!--天蓝色 -->   
	        <color name="honeydew">#f0fff0</color><!--蜜色 -->   
	        <color name="aliceblue">#f0f8ff</color><!--艾利斯兰 -->   
	        <color name="khaki">#f0e68c</color><!--黄褐色 -->   
	        <color name="lightcoral">#f08080</color><!--亮珊瑚色 -->   
	        <color name="palegoldenrod">#eee8aa</color><!--苍麒麟色 -->   
	        <color name="violet">#ee82ee</color><!--紫罗兰色 -->   
	        <color name="darksalmon">#e9967a</color><!--暗肉色 -->   
	        <color name="lavender">#e6e6fa</color><!--淡紫色 -->   
	        <color name="lightcyan">#e0ffff</color><!--亮青色 -->   
	        <color name="burlywood">#deb887</color><!--实木色 -->   
	        <color name="plum">#dda0dd</color><!--洋李色 -->   
	        <color name="gainsboro">#dcdcdc</color><!--淡灰色 -->   
	        <color name="crimson">#dc143c</color><!--暗深红色 -->   
	        <color name="palevioletred">#db7093</color><!--苍紫罗兰色 -->   
	        <color name="goldenrod">#daa520</color><!--金麒麟色 -->   
	        <color name="orchid">#da70d6</color><!--淡紫色 -->   
	        <color name="thistle">#d8bfd8</color><!--蓟色 -->   
	        <color name="lightgray">#d3d3d3</color><!--亮灰色 -->   
	        <color name="lightgrey">#d3d3d3</color><!--亮灰色 -->   
	        <color name="tan">#d2b48c</color><!--茶色 -->   
	        <color name="chocolate">#d2691e</color><!--巧可力色 -->   
	        <color name="peru">#cd853f</color><!--秘鲁色 -->   
	        <color name="indianred">#cd5c5c</color><!--印第安红 -->   
	        <color name="mediumvioletred">#c71585</color><!--中紫罗兰色 -->   
	        <color name="silver">#c0c0c0</color><!--银色 -->   
	        <color name="darkkhaki">#bdb76b</color><!--暗黄褐色 -->   
	        <color name="rosybrown">#bc8f8f</color><!--褐玫瑰红 -->   
	        <color name="mediumorchid">#ba55d3</color><!--中粉紫色 -->   
	        <color name="darkgoldenrod">#b8860b</color><!--暗金黄色 -->   
	        <color name="firebrick">#b22222</color><!--火砖色 -->   
	        <color name="powderblue">#b0e0e6</color><!--粉蓝色 -->   
	        <color name="lightsteelblue">#b0c4de</color><!--亮钢兰色 -->   
	        <color name="paleturquoise">#afeeee</color><!--苍宝石绿 -->   
	        <color name="greenyellow">#adff2f</color><!--黄绿色 -->   
	        <color name="lightblue">#add8e6</color><!--亮蓝色 -->   
	        <color name="darkgray">#a9a9a9</color><!--暗灰色 -->   
	        <color name="darkgrey">#a9a9a9</color><!--暗灰色 -->   
	        <color name="brown">#a52a2a</color><!--褐色 -->   
	        <color name="sienna">#a0522d</color><!--赭色 -->   
	        <color name="darkorchid">#9932cc</color><!--暗紫色 -->   
	        <color name="palegreen">#98fb98</color><!--苍绿色 -->   
	        <color name="darkviolet">#9400d3</color><!--暗紫罗兰色 -->   
	        <color name="mediumpurple">#9370db</color><!--中紫色 -->   
	        <color name="lightgreen">#90ee90</color><!--亮绿色 -->   
	        <color name="darkseagreen">#8fbc8f</color><!--暗海兰色 -->   
	        <color name="saddlebrown">#8b4513</color><!--重褐色 -->   
	        <color name="darkmagenta">#8b008b</color><!--暗洋红 -->   
	        <color name="darkred">#8b0000</color><!--暗红色 -->   
	        <color name="blueviolet">#8a2be2</color><!--紫罗兰蓝色 -->   
	        <color name="lightskyblue">#87cefa</color><!--亮天蓝色 -->   
	        <color name="skyblue">#87ceeb</color><!--天蓝色 -->   
	        <color name="gray">#808080</color><!--灰色 -->   
	        <color name="grey">#808080</color><!--灰色 -->   
	        <color name="olive">#808000</color><!--橄榄色 -->   
	        <color name="purple">#800080</color><!--紫色 -->   
	        <color name="maroon">#800000</color><!--粟色 -->   
	        <color name="aquamarine">#7fffd4</color><!--碧绿色 -->   
	        <color name="chartreuse">#7fff00</color><!--黄绿色 -->   
	        <color name="lawngreen">#7cfc00</color><!--草绿色 -->   
	        <color name="mediumslateblue">#7b68ee</color><!--中暗蓝色 -->   
	        <color name="lightslategray">#778899</color><!--亮蓝灰 -->   
	        <color name="lightslategrey">#778899</color><!--亮蓝灰 -->   
	        <color name="slategray">#708090</color><!--灰石色 -->   
	        <color name="slategrey">#708090</color><!--灰石色 -->   
	        <color name="olivedrab">#6b8e23</color><!--深绿褐色 -->   
	        <color name="slateblue">#6a5acd</color><!--石蓝色 -->   
	        <color name="dimgray">#696969</color><!--暗灰色 -->   
	        <color name="dimgrey">#696969</color><!--暗灰色 -->   
	        <color name="mediumaquamarine">#66cdaa</color><!--中绿色 -->   
	        <color name="cornflowerblue">#6495ed</color><!--菊兰色 -->   
	        <color name="cadetblue">#5f9ea0</color><!--军兰色 -->   
	        <color name="darkolivegreen">#556b2f</color><!--暗橄榄绿 -->   
	        <color name="indigo">#4b0082</color><!--靛青色 -->   
	        <color name="mediumturquoise">#48d1cc</color><!--中绿宝石 -->   
	        <color name="darkslateblue">#483d8b</color><!--暗灰蓝色 -->   
	        <color name="steelblue">#4682b4</color><!--钢兰色 -->   
	        <color name="royalblue">#4169e1</color><!--皇家蓝 -->   
	        <color name="turquoise">#40e0d0</color><!--青绿色 -->   
	        <color name="mediumseagreen">#3cb371</color><!--中海蓝 -->   
	        <color name="limegreen">#32cd32</color><!--橙绿色 -->   
	        <color name="darkslategray">#2f4f4f</color><!--暗瓦灰色 -->   
	        <color name="darkslategrey">#2f4f4f</color><!--暗瓦灰色 -->   
	        <color name="seagreen">#2e8b57</color><!--海绿色 -->   
	        <color name="forestgreen">#228b22</color><!--森林绿 -->   
	        <color name="lightseagreen">#20b2aa</color><!--亮海蓝色 -->   
	        <color name="dodgerblue">#1e90ff</color><!--闪兰色 -->   
	        <color name="midnightblue">#191970</color><!--中灰兰色 -->   
	        <color name="aqua">#00ffff</color><!--浅绿色 -->   
	        <color name="cyan">#00ffff</color><!--青色 -->   
	        <color name="springgreen">#00ff7f</color><!--春绿色 -->   
	        <color name="lime">#00ff00</color><!--酸橙色 -->   
	        <color name="mediumspringgreen">#00fa9a</color><!--中春绿色 -->   
	        <color name="darkturquoise">#00ced1</color><!--暗宝石绿 -->   
	        <color name="deepskyblue">#00bfff</color><!--深天蓝色 -->   
	        <color name="darkcyan">#008b8b</color><!--暗青色 -->   
	        <color name="teal">#008080</color><!--水鸭色 -->   
	        <color name="green">#008000</color><!--绿色 -->   
	        <color name="darkgreen">#006400</color><!--暗绿色 -->   
	        <color name="blue">#0000ff</color><!--蓝色 -->   
	        <color name="mediumblue">#0000cd</color><!--中兰色 -->   
	        <color name="darkblue">#00008b</color><!--暗蓝色 -->   
	        <color name="navy">#000080</color><!--海军色 -->   
	        <color name="black">#000000</color><!--黑色 -->   
	</resources>  
