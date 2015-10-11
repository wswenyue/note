#picasso 毕加索
##介绍：
Square公司开源的一个Android图形缓存库
> A powerful image downloading and caching library for Android

项目地址： https://github.com/square/picasso
文档： http://square.github.io/picasso/
API： http://square.github.io/picasso/javadoc/index.html

如果使用ProGuard要加入如下依赖：

    -dontwarn com.squareup.okhttp.**

优点及特性：
以下在Picasso中会自动处理，无需使用者操心：

- Handling ImageView recycling and download cancelation in an adapter.
- Complex image transformations with minimal memory use.
- Automatic memory and disk caching.

##使用：
###添加jar：
Gradle：

	compile 'com.squareup.picasso:picasso:2.5.2'

Maven：

```maven
<dependency>
  <groupId>com.squareup.picasso</groupId>
  <artifactId>picasso</artifactId>
  <version>2.5.2</version>
</dependency>	
```

###使用方法
最简单的使用：

```java
Picasso.with(context).load("http://i.imgur.com/DvpvklR.png").into(imageView);
```

可以加载的资源很多：

`Resources, assets, files, content providers are all supported as image sources.`

```java
Picasso.with(context).load(R.drawable.landing_screen).into(imageView1);
Picasso.with(context).load("file:///android_asset/DvpvklR.png").into(imageView2);
Picasso.with(context).load(new File(...)).into(imageView3);
```

在适配器中使用：

```java
@Override
public void getView(int position, View convertView, ViewGroup ) {

  Picasso.with(context).load(url).into(view);//这里注意Picasso.with(context)应该提取到外面，不应该被重复执行，否则可能出现Bug
}
```
设置图片的大小及填充方式

```java
Picasso.with(context)
  .load(url)
  .resize(50, 50)	//设置大小
  .centerCrop()		//设置填充方式，这个是推荐的
  .into(imageView)
```

指定加载完成之前显示的图片和加载出错显示的图片：

```java
Picasso.with(context)
    .load(url)
    .placeholder(R.drawable.user_placeholder)
    .error(R.drawable.user_placeholder_error)
    .into(imageView);
```

定制图片的转换方式

```java
public class CropSquareTransformation implements Transformation {
  @Override public Bitmap transform(Bitmap source) {
    int size = Math.min(source.getWidth(), source.getHeight());
    int x = (source.getWidth() - size) / 2;
    int y = (source.getHeight() - size) / 2;
    Bitmap result = Bitmap.createBitmap(source, x, y, size, size);//这个是用来裁剪的图片的
    if (result != source) {
      source.recycle();//这里必须回收
    }
    return result;
  }

  @Override public String key() {	//这个方法返回的key应该是唯一的，用来作为缓存的键
 	return "square()"; 
	}
}
```
使用变换：

```java
picasso.load(R.drawable.download)
           .skipMemoryCache() //不要把加载的图片放入缓存，也不要从缓存中取图片
        .transform(new CropSquareTransformation())    //执行自定义变换
        .into(view);
```

加载图片并自定义动作

```java
Target target = new Target(){
      @Override
      public void onBitmapLoaded(Bitmap bitmap, Picasso.LoadedFrom loadedFrom) {
             //当图片加载成功时调用，bitmap是加载的图片,loadFrom 标明图片的来源是网络、内存还是磁盘
             //可以在里面执行把图片保存到本地的操作
      }
      @Override
      public void onBitmapFailed(Drawable errorDrawable) {   //当图片加载失败时调用
      }
      @Override
      public void onPrepareLoad(Drawable placeHolderDrawable) {    //当任务被提交时调用
      }
};
picasso.load(new File("/1.jpg")).into(target); //指定target任务加载图片
```

加载图片到 ImageView:

```java
ImageView view = null;
Picasso picasso = Picasso.with(this);
picasso.setIndicatorsEnabled(true);   //开启调模式，它能够在图片左上角显示小三角形，这个小三角形的颜色标明了图片的来源：网络、内存缓存、磁盘缓存
picasso.setLoggingEnabled(true);  //打开日志，即log中会打印出目前下载的进度、情况
picasso.load("http://xxx.jpg")    //可以是本地图片或网络图片
       .placeholder(R.drawable.placeholder)   //当图片正在加载时显示的图片(optional)
       .error(R.drawable.error)           //当图片加载失败时显示的图片(optional)
       .into(view, new Callback() {   //将图片下载完后放进view中，回调是可选的
           @Override
           public void onSuccess() {
               //加载图片成功时回调
           }
           @Override
           public void onError() {
               //加载图片失败时回调
           }
       });

```

开发者可以打开调试功能：
打开之后如果图片是从网络中获得，则图片左上角会有红色三角形；如果图片从磁盘获得，则图片左上角会有黄色三角形；如果图片从内存获得，则图片左上角会有绿色三角形。如图所示：

![](http://square.github.io/picasso/static/debug.png)



