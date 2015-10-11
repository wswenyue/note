#ActionBar
##了解ActionBar
概念：动作栏或导航控件，Action bar就是替换3.0以前的tittle bar和menu

**主要功能：**
1)突出显示一些重要操作（如“注册”、“登录”、“搜索”等），将平时隐藏的选项菜单显示成活动项ActionItem；
2)在程序中保持统一的页面导航和切换方式。这种基于Tab的导航方式，可以切换多个Fragment；
3)提供基于下拉的导航菜单；
4)使用程序logo，作为返回APP的HOME主页面或向上的导航操作。

ActionBar分成四个区域：
> 可显示APP的icon，也可用其他图标代替。当软件不在最高级页面时，图标左侧会显示一个左箭头，用户可以通过这个箭头向上导航

app icon 图标区：
> 可显示APP的icon，也可用其他图标代替。当软件不在最高级页面时，图标左侧会显示一个左箭头，用户可以通过这个箭头向上导航

View Control 视图切换：
> drop-down菜单或tab controls，也可以用来显示非交互的内容，例如app title或更长的品牌信息

action buttons 动作按钮：
> 这个放最重要的软件功能，放不下的按钮就自动进入Action overflow了

action overflow 溢出动作项:
> 不常用的操作项目自动进入Action overflow

##添加与删除ActionBar
###添加ActionBar
从Android3.0（API级别 11）开始


	默认的主题是Theme.Hole，Action bar被包含在这个主题中
	当targetSdkVersion或minSdkVersion属性被设置为“11”或更大的数值

Android 3.0以前使用(需要引入appcompat v7 Support Library)
把Activity的主题设置为Theme.Holo.NoActionBar就可以了
	
	在AndroidManifest.xml清单配置文件中：
	<activity android:theme="@android:style/Theme.Holo.NoActionBar">  

删除ActionBar

使用Action bar的 hide()方法

	在onCreate()方法中执行如下代码：
	ActionBar actionBar = getActionBar();  
	actionBar.hide();
  
###添加ActionBar动作项
与添加系统菜单相同，重写onCreateOptionsMenu()

通过给<item>元素声明android:showAsAction=”ifRoom”属性
    
    never ： 不将该MenuItem显示在ActionBar上（是默认值）
    always  ： 总是将该MenuItem显示在ActionBar上
    ifRoom  ： 当AcitonBar位置充裕时将该MenuItem显示在ActionBar上
    withText  ： 将该MenuItem显示在ActionBar上，并显示该菜单项的文本
    collapseActionView  ： 将该ActionView折叠成普通菜单项。最低API=14
    
重写onOptionsItemSelected()的回调方法，处理用户选择动作项的操作

###分隔操作栏
**仅Android4.0（API 级别 14）或以上的版本**
	
	在<application>或<activity>元素中添加uiOptions=”splitActionBarWhenNarrow”属性设置

###启动程序图标导航
作用：是让APP的LOGO也变成可以点击的导航图标

    ActionBar getActionBar() 获取当前Activity的ActionBar对象
    ActionBar.setDisplayHomeAsUpEnabled(true);  设置是否将LOGO图标转成可点击的按钮，并在图标前添加一个向左的箭头
    ActionBar.setDisplayShowHomeEnabled(true) 设置是否显示LOGO图标
    ActionBar.setHomeButtonEnabled(true) 设置是否将LOGO图标转变成可点击的按钮
    
###ActionView的使用
作用：可编辑的动作项，如SearchView可以直接在ActionBar上使用
两种方式添加ActionView

    actionLayout属性，指定一个布局文件
    actionViewClass属性，指定个实现CollapsibleActionView的子类

手动展开和折叠菜单栏

    MenuItem.expandActionView() 展开ActionView
    MenuItem.collapseActionView() 折叠ActionView
    MenuItem.setOnActionExpandListener() 设置展开或折叠的监听事件

###ActionProvider的使用
作用：指定动作项的意图，动作项被执行时会列出响应这个意图的所有应用程序
使用ShareActionProvider类
	
	在item里设置android:actionProviderClass="android.widget.ShareActionProvider"
	获取ShareActionProvider对象
	调用setShareIntent()方法

	注意：
		默认情况，ShareActionProvider对象会基于用户的使用频率来保留共享目标的排列顺序。
		默认情况下，排序信息被保存在由DEFAULT_SHARE_HISTORY_FILE_NAME指定名称的私有文件中。
		如果不同shareActionProvider需要分别保存顺序
			调用setShareHistoryFileName()方法，并且提供一个XML文件的名字（如custom_share_history.xml）
			mShareActionProvider.setShareHistoryFileName("custom_share_history.xml");
		尽管ShareActionProvider类是基于使用频率来排列共享目标的，但是这种行为是可扩展的，并且ShareActionProvider类的扩展能够基于历史文件执行不同的行为和排序。

创建一个自定义的动作提供者

 **继承ActionProvider类**
 **实现合适的回调方法**
- 重写ActionProvider(Context)
		
 `这个构造器把应用程序的Context对象传递个操作提供器，你应该把它保存在一个成员变量中，以便其他的回调方法使用。`

- 重写OnCreateActionView() 创建菜单项的操作视图，并返回View
 `这是你给菜单项定义操作视窗的地方。使用从构造器中接收的Context对象，获取一个LayoutInflater对象的实例，并且用XML资源来填充操作视窗，然后注册事件监听器,代码如下`：

 		public View onCreateActionView() {  
	    // Inflate the action view to be shown on the action bar.  
	    LayoutInflater layoutInflater = LayoutInflater.from(mContext);  
	    View view = layoutInflater.inflate(R.layout.action_provider, null);  
	    ImageButton button = (ImageButton) view.findViewById(R.id.button);  
	    button.setOnClickListener(new View.OnClickListener() {  
	    @Override  
	    public void onClick(View v) {  
	    // Do something...  
	    }  
	    });  
	    return view;  
	    } 

- onPerformDefaultAction() 选择上下文菜单项时调用
	
		在选中悬浮(上下文)菜单中的菜单时，系统会调用这个方法，并且操作提供器应该这对这个选中的菜单项执行默认的操作。

- onPrepareSubMenu() 选择子菜单项时调用
	  
		回调方法来显示子菜单。这样onPerformDefaultAction()在子菜单显示时就不会被调用。
	注意： 实现了onOptionsItemSelected()回调方法的Activity或Frament对象能够通过处理item-selected事件（并且返回true）来覆盖操作提供器的默认行为，这种情况下，系统不会调用onPerformDefaultAction()回调方法。

**在item里面引用 **   

	 android:actionProviderClass="com.example.actionbar.MyActionProvider"


###ActionBarTab的使用
作用：通常用选项标签使Fragmengt之间相互切换
使用步骤：
**实现ActionBar.TabListener接口**

	警告：针对每个回调中的Fragment事务，你都不必调用commit()方法---系统会调用这个方法，并且如果你自己调用了这个方法，有可能会抛出一个异常。你也不能把这些Fragment事务添加到回退堆栈中	

	 class MyTabListener implements  ActionBar.TabListener{
		@Override
		public void onTabSelected(Tab tab, FragmentTransaction ft) {
			int tag = Integer.parseInt(tab.getTag().toString());
			switch(tag){
			case 1:
				ft.replace(R.id.main, new FragmentA(),"fragmentA");
	  //ft.addToBackStack("fragmentA");不能加入回退栈
				break;
			case 2:
				ft.replace(R.id.main, new FragmentB(),"fragmentB");
				break;
			case 3:
				ft.replace(R.id.main, new FragmentC(),"fragmentC");
				break;
			}
	//ft.commit();不用调用提交方法
		}

		@Override
		public void onTabUnselected(Tab tab, FragmentTransaction ft) {
			// TODO Auto-generated method stub
			
		}

		@Override
		public void onTabReselected(Tab tab, FragmentTransaction ft) {
			// TODO Auto-generated method stub
			
		}
		
	 }

**必须调用ActionBar.setNavigationMode(NAVIGATION_MODE_TABS)方法让选项标签可见**

    actionBar = getActionBar();
    //设置导航模式
    actionBar.setNavigationMode(ActionBar.NAVIGATION_MODE_TABS);
    actionBar.setDisplayShowTitleEnabled(false);
    
    //如果选项标签的标题实际指示了当前的View对象，你也可以通过调用setDisplayShowTitleEnabled(false)方法来禁用Activity的标题

**创建ActionBar.Tab选项标签对象并设置监听事件**

    对于每个要添加的选项标签，都要实例化一个ActionBar.Tab对象，并且调用setTabListener()方法设置ActionBar.Tab对象的事件监听器，还可以用setText()或setIcon()方法来设置选项标签的标题或图标。
    
    Tab tab1 = actionBar.newTab();
    tab1.setIcon(R.drawable.calendar);
    tab1.setTabListener(new MyTabListener());
    tab1.setTag("1");

**调用ActionBar.addTab(Tab)方法，将选项标签添加到动作栏中**

###添加下拉式导航
 1.  创建一个给Spinner使用的item布局文件
 2.  实现ActionBar.OnNavigationListener回调接口，处理用户选择列表项的事件
 3.  调用ActionBar.setNavigationMode(ActionBar.NAVIGATION_MODE_LIST)方法启用导航模式
 4.  用setListNavigationCallbacks()方法给下拉列表设置回调方法
 	1. actionBar.setListNavigationCallbacks(mSpinnerAdapter, mNavigationCallback);  
 	2. 这个方法需要`SpinnerAdapter`和ActionBar.OnNavigationListener对象

###ActionBar相关类

    ActionBar  Activity.getActionBar() 获取ActionBar对象
    	hide() 隐藏
    	show() 显示
    	setNavigationMode(int mode) 设置导航模式
    		ActionBar.NAVIGATION_MODE_LIST 下拉列表
    		NAVIGATION_MODE_STANDARD 默认
    		NAVIGATION_MODE_TABS 页卡
    Tab newTab() 创建页卡
    
    Menu MenuItem findItem(int id) 查找菜单项或action item

###ViewPager
####ViewPager
特点：可以左右滑动的控件，需要PagerAdapter配合使用，由v4包提供

类全名： android.support.v4.view.ViewPager
使用步骤
- 在布局文件中使用<android.support.v4.view.ViewPager>标签
		
		<android.support.v4.view.ViewPager  
	    android:id="@+id/viewpager"  
	    android:layout_width="wrap_content"   
	    android:layout_height="wrap_content"   
	    android:layout_gravity="center" >   
	    
	    可以在ViewPager标签内增加选项卡控件<android.support.v4.view.PagerTabStrip>，如
	     <android.support.v4.view.ViewPager
	    android:id="@+id/viewpager"
	    android:layout_width="wrap_content"
	    android:layout_height="wrap_content"
	    android:layout_gravity="center" >
	      
	    <!-- 标题指示器控件 -->
	    <android.support.v4.view.PagerTabStrip  
	    android:id="@+id/pagertab"  
	    android:layout_width="wrap_content"  
	    android:layout_height="wrap_content"  
	    android:layout_gravity="top"/>  
	     
	    </android.support.v4.view.ViewPager>
	    
	    <!--注意事项:   
	    1.这里ViewPager和 PagerTabStrip都要把包名写全了，不然会ClassNotFount  
	    2.API中说：在布局xml把PagerTabStrip当做ViewPager的一个子标签来用，不能拿出来，不然还是会报错  
	    3.在PagerTabStrip标签中可以用属性android:layout_gravity=TOP|BOTTOM来指定title的位置  
	    4.如果要显示出PagerTabStrip某一页的title,需要在ViewPager的adapter中实现getPageTitle(int)--> 

- 代码中增加显示的页面
	
		LayoutInflater lf = getLayoutInflater().from(this);   

        view1 = lf.inflate(R.layout.layout1, null);     
        view2 = lf.inflate(R.layout.layout2, null);       
        view3 = lf.inflate(R.layout.layout3, null);      
        viewList = new ArrayList<View>();// 将要分页显示的View装入数组中     
        viewList.add(view1);         
        viewList.add(view2);          
        viewList.add(view3); 

- 在Activity里实例化ViewPager组件，并设置它的Adapter（即PagerAdapter）
	
		public class MyViewPagerAdapter extends PagerAdapter{
		private List<View> mListViews;
		
		public MyViewPagerAdapter(List<View> mListViews) {
			this.mListViews = mListViews;//构造方法，参数是我们的页卡，这样比较方便。
		}

		@Override
		public Object instantiateItem(ViewGroup container, int position) {	//这个方法用来实例化页卡		
			 container.addView(mListViews.get(position), 0);//添加页卡
			 return mListViews.get(position);
		}

		@Override
		public void destroyItem(ViewGroup container, int position, Object object) 	{	
			container.removeView(mListViews.get(position));//删除页卡
		}

		@Override
		public int getCount() {			
			return  mListViews.size();//返回页卡的数量
		}
		
		@Override
		public boolean isViewFromObject(View arg0, Object arg1) {			
			return arg0==arg1;//判断View是否来源于Object，官方提示写法：return view == object;
		}
	}

简单应用实例

	layout布局文件

	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <android.support.v4.view.ViewPager
        android:id="@+id/viewpager"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center" >
              
        <android.support.v4.view.PagerTabStrip  
            android:id="@+id/pagertab"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_gravity="top"/>  
         
    </android.support.v4.view.ViewPager>
	</LinearLayout>

	src代码：

	public class ViewPagerDemo extends Activity {

	private View view1, view2, view3;//需要滑动的页卡
	private ViewPager viewPager;//viewpager
	private PagerTitleStrip pagerTitleStrip;//viewpager的标题
	private PagerTabStrip pagerTabStrip;//一个viewpager的指示器，效果就是一个横的粗的下划线
	private List<View> viewList;//把需要滑动的页卡添加到这个list中
	private List<String> titleList;//viewpager的标题
	private Button weibo_button;//button对象，一会用来进入第二个Viewpager的示例
  	private Intent intent;
	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_view_pager_demo);
		initView();
	}

	private void initView() {
		viewPager = (ViewPager) findViewById(R.id.viewpager);
		//pagerTitleStrip = (PagerTitleStrip) findViewById(R.id.pagertitle);
		pagerTabStrip=(PagerTabStrip) findViewById(R.id.pagertab);
		
		pagerTabStrip.setTabIndicatorColor(getResources().getColor(R.color.gold)); //设置指示器颜色
		pagerTabStrip.setDrawFullUnderline(false); //是否全部显示指示器颜色（暗淡的）
		pagerTabStrip.setBackgroundColor(getResources().getColor(R.color.azure)); //设置背景颜色
		pagerTabStrip.setTextSpacing(50); //设置标题的字体间隔
		
		view1 = findViewById(R.layout.layout1);
		view2 = findViewById(R.layout.layout2);
		view3 = findViewById(R.layout.layout3);

		LayoutInflater lf = getLayoutInflater().from(this);
		view1 = lf.inflate(R.layout.layout1, null);
		view2 = lf.inflate(R.layout.layout2, null);
		view3 = lf.inflate(R.layout.layout3, null);

		viewList = new ArrayList<View>();// 将要分页显示的View装入数组中
		viewList.add(view1);
		viewList.add(view2);
		viewList.add(view3);

		titleList = new ArrayList<String>();// 每个页面的Title数据
		titleList.add("wp");
		titleList.add("jy");
		titleList.add("jh");

		PagerAdapter pagerAdapter = new PagerAdapter() {

			@Override
			public boolean isViewFromObject(View arg0, Object arg1) {

				return arg0 == arg1;
			}

			@Override
			public int getCount() {

				return viewList.size();
			}

			@Override
			public void destroyItem(ViewGroup container, int position,
					Object object) {
				container.removeView(viewList.get(position));

			}

			@Override
			public int getItemPosition(Object object) {

				return super.getItemPosition(object);
			}

			@Override
			public CharSequence getPageTitle(int position) {

				return titleList.get(position);//指示控件中显示的标题
			}

			@Override
			public Object instantiateItem(ViewGroup container, int position) {
				container.addView(viewList.get(position));
			
				weibo_button.setOnClickListener(new OnClickListener() {
					
					public void onClick(View v) {
						intent=new Intent(ViewPagerDemo.this,WeiBoActivity.class);
						startActivity(intent);
					}
				});
				return viewList.get(position);
			}
		};
		viewPager.setAdapter(pagerAdapter);
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		getMenuInflater().inflate(R.menu.activity_view_pager_demo, menu);
		return true;
	}

	}

实现引导界面功能

