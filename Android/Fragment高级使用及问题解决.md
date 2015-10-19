#fragment相关问题
##复用

**实现Fragmnet复用的方法：**

```java
private void showFragment(String tag, Class<? extends Fragment> modelFragment, int layoutId) {
    Fragment f = fm.findFragmentByTag(tag);
    if (f == null) {
        //首次显示
        try {
            f = modelFragment.newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }


        if (curFragment == null) {
            //显示的第一页面

            fm.beginTransaction()
                    .add(layoutId, f, tag)
                    .commit();
        } else {
            //隐藏当前正显示的Fragment,并增加第一次显示的model1-f
            fm.beginTransaction()
                    .hide(curFragment)
                    .add(layoutId, f, tag)
                    .commit();
        }
    } else { //之前已显示过了，再次显示
        if (curFragment == f) return;

        fm.beginTransaction()
                .hide(curFragment)
                .show(f)
                .commit();
    }

    curFragment = f;
}
```