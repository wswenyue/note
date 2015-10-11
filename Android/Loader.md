#Loader
##概念及使用
###概念：用于对数据进行异步加载

    一、引入：
       由于SimpleCursorAdapter处理大数据时会出现反应慢甚至ANR，故引用Loader，它可以对数据进行异步加载，提升了应用的稳定性。使用Loader时，要求SDK最低版本是11,即Android 3.0
    二、特点：
     1.对每一个Activity或fragment都有效
     2.提供异步加载数据的机制
     3.监视数据源的变化，并针对变化返回一个新的结果
     4.由于配置发生变化而重新被创建后，它们会自动重新连接到上一个加载器的游标，所以不必重新查询数据

###使用方法：
#### 使用Loader时包含的组件

    Activity或Fragment
    一个LoaderManager实例，用于管理Loader
    使用CursorLoader，用于查询ContentProvider的数据
    实现LoaderMnager.LoaderCallBacks<D>,它可以初始化一个Loader或者管理Loader
    要想显示Loader的数据，可以使用SimpleCursorAdapter并使用观察者模式
    当使用CursorLoader时，数据源会是一个ContentProvider

#### 启动Loader
**LoaderManager管理Activity或fragment中的一个或多个Loader**

**通常在Activity的onCreate()方法中对LoaderManager进行初始化**
    
    代码如下：
    getLoaderManager().initLoader(0, null, this);
    
    initLoader()方法中参数说明：
    -----------------------------
    第一个参数： Loader的ID，
    第二个参数： Bundle对象，用于存放数据
    第三个参数： LoaderMnager.LoaderCallBacks<D>回调
    
    注意：Loader的ID必须唯一

**保证一个Loader被初始化并激活，它具有两种可能的结果**

    a.如果初始化的Loader ID已存在，将重用原来的Loader
    b.如果初始化的Loader ID不存在，将使用LoaderMnager.LoaderCallBacks中的onCreateLoader()实例化一个Loader对象，它是创建Loader对象的发源地

#### 重启Loader
**当有新数据更新时，需要使用restartLoader()方法对该Loader进行重启**

**使用方法: getLoaderManager().restartLoader(0, null, this)**

### LoaderManager
    
    作用： 管理Loader的启动、重启和注销
    获取LoaderManager对象： Activity.getLoaderMaanager()
    initLoader(int id, Bundle args, LoaderCallbacks<D> callback)： 初始化和启动一个Loader
    restartLoader(int id, Bundle args, LoaderCallbacks<D> callback)： 通过loader id重新启动一个Loader
    destroyLoader(int id): 注销一个Loader

###LoaderMnager.LoaderCallBacks<D>

    使用LoaderMnager.LoaderCallBacks<D>回调
	
	①Loader<D> onCreateLoader(int id,Bundle args)：根据传入的ID，初始化并返回一个Loader
	一般在LoaderManager.initLoader()或LoaderManager.restartLoader()时调用
	
	②onLoadFinished(Loader<D> loader,D data)：
	当上一个Loader完成加载过程之后调用，在该方法中会进行数据的更新，比如adapter.swapCursor(data);
	
	③onLoaderReset(Loader<D> loader)：
	当一个已创建的Loader重启使其老数据无效时，调用该方法，在该方法中会将数据清空，比如adapter.swapCursor(null);

## CursorLoader

    CursorLoader(Context context, Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder)
    
    方法的参数说明
    --------------------------
    context→上下文对象
    uri→其他应用所提供的Uri
    projection→字段集合
    selection →查询条件
    selectionArgs →查询条件中包含的参数
    sortOrder →排序字段

**作用：加载ContentProvider提供的数据**

**父类： AsyncTaskLoader ->Loader**

**CursorLoader(Context, Uri, String[], String, String[], String)**

## AsyncTaskLoader<D>
父类：android.content.Loader<D>

概念： 抽像的Loader子类，并提供AsyncTask类来工作，用于加载大数据

主要方法

    onStartLoading() 开始启动Loader方法
		如果AsyncTaskLoader是第一次启动，必须执行 forceLoad()方法
    以下方法不需要重写
    D loadInBackground() 后台加载数据的处理方法
    deliverResult(D result) 发布数据的处理方法
    onContentChanged() 数据内容改变时的处理方法

使用方法：同CursorLoader

##SearchView
**父类**：LinearLayout

**概念**：文本搜索控件，可用于ActionBar上的ActionView控件

**常用属性**

	android:inputType="text" 设置输入内容的类型，同EditText的inputType属性
	android:iconifiedByDefault="true"  是否图标化，true为图标化，在点击图标时才显示文本输入框
	android:queryHint 设置提示文本信息

**设置查询文本事件监听器**：setOnQueryTextListener(SearchView.OnQueryTextListener)

	onQueryTextChange(String newText) 查询内容发生改变时
	onQueryTextSubmit(String query) 按“Enter“或"Search"键提交查询内容