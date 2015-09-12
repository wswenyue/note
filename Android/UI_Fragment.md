#Fragment
##特点及用途

- 利用碎片化来解决Android屏幕尺寸差距带来的适配问题
- Fragment拥有自己的生命周期和能接收、处理用户的事件
- 你可以把Fragment当成Activity的一个界面的一个组成部分
- 你可以动态的添加、替换和移除某个Fragment

##Fragment的生命周期
Fragment必须是依存与Activity而存在的，因此Activity的生命周期会直接影响到Fragment的生命周期。官网这张图很好的说明了两者生命周期的关系：

![Fragment的生命周期](http://7xj2yt.com1.z0.glb.clouddn.com/android_Fragment的生命周期.png)

可以看到Fragment比Activity多了几个额外的生命周期回调方法：
onAttach(Activity)
当Fragment与Activity发生关联时调用。
onCreateView(LayoutInflater, ViewGroup,Bundle)
创建该Fragment的视图
onActivityCreated(Bundle)
当Activity的onCreate方法返回时调用
onDestoryView()
与onCreateView想对应，当该Fragment的视图被移除时调用
onDetach()
与onAttach相对应，当Fragment与Activity关联被取消时调用
注意：除了onCreateView，其他的所有方法如果你重写了，必须调用父类对于该方法的实现，

##使用Fragment
###静态使用
