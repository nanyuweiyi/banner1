### MZBannerView

现在的APP Banner大多数千篇一律，前几天看到魅族手机上所有魅族自家APP上的Banner效果不错，于是就想着来仿着做一个类似的效果。因此就有了这个库。但是为了使用方便，这个库不仅仅只有仿魅族效果的BannerView 来使用，还可以当作普通的BannerView 来使用，还可以当作一个ViewPager 来使用。使用很方便，具体使用方法和API 请看后面的示例。


**MZBannerView 有以下功能：**

1 . 仿魅族BannerView 效果。

2 . 当普通Banner 使用

3 . 当普通ViewPager 使用。

4 . 当普通ViewPager使用(有魅族Banner效果)

5 . 仿爱奇艺Banner效果。

### 更新日志

**v1.1.1 :** 增加按住Banner 停止轮播，松开开始自动轮播的功能

**v1.1.0 :** fix 在从网上获取数据后，banner 显示 造成 ANR 的bug(如果在onCreate()中设置资源显示则没问题)

**v1.1.2 :** fix 更改数据之后，调用setPages重新刷新数据会crush的bug

**v2.0.0 :**  
              1,add: 添加仿魅族Banner效果，中间Page覆盖两边。
              -- 2,fix 部分bug: 添加OnPageChangeListener 回调 pisition 不对的bug.


### Dependency
Add it in your root build.gradle at the end of repositories:

```java
allprojects {
     repositories {
          ...
          maven { url 'https://jitpack.io' }
     }
}
```
Step 2. Add the dependency
```
dependencies {
         compile 'com.github.pinguo-zhouwei:MZBannerView:v2.0.0'
}
```

### 自定义属性

| 属性名      | 属性意义   |  取值  |
| --------   | -----:   | :----: |
| open_mz_mode       | 是否开启魅族模式     |   true 为魅族Banner效果，false 则普通Banner效果   |
| canLoop        | 是否轮播     |   true 轮播，false 则为普通ViewPager   |
| indicatorPaddingLeft        | 设置指示器距离左侧的距离      |   单位为 dp 的值     |
| indicatorPaddingRight        | 设置指示器距离右侧的距离     |     单位为 dp 的值  |
| indicatorAlign        | 设置指示器的位置      |   有三个取值：left 左边，center 剧中显示，right 右侧显示   |
| middle_page_cover        | 设置中间Page是否覆盖（真正的魅族Banner效果）     |   true 覆盖，false 无覆盖效果 |


### 使用方法

1 . xml 布局文件
```java
 <com.zhouwei.mzbanner.MZBannerView
       android:id="@+id/banner"
       android:layout_width="match_parent"
       android:layout_height="200dp"
       android:layout_marginTop="10dp"
       app:open_mz_mode="true"
       app:canLoop="true"
       app:indicatorAlign="center"
       app:indicatorPaddingLeft="10dp"
       />
```
2 . activity中代码：
```java
        mMZBanner = (MZBannerView) view.findViewById(R.id.banner);
     
       // 设置数据
        mMZBanner.setPages(list, new MZHolderCreator<BannerViewHolder>() {
            @Override
            public BannerViewHolder createViewHolder() {
                return new BannerViewHolder();
            }
        });

 public static class BannerViewHolder implements MZViewHolder<Integer> {
        private ImageView mImageView;
        @Override
        public View createView(Context context) {
            // 返回页面布局
            View view = LayoutInflater.from(context).inflate(R.layout.banner_item,null);
            mImageView = (ImageView) view.findViewById(R.id.banner_image);
            return view;
        }

        @Override
        public void onBind(Context context, int position, Integer data) {
            // 数据绑定
            mImageView.setImageResource(data);
        }
    }
```
3 .如果是当Banner使用，注意在onResume 中调用start()方法，在onPause中调用 pause() 方法。如果当普通ViewPager使用，则不需要。
```java
 @Override
    public void onPause() {
        super.onPause();
        mMZBanner.pause();//暂停轮播
    }

    @Override
    public void onResume() {
        super.onResume();
        mMZBanner.start();//开始轮播
    }
```

**通过`open_mz_mode `、`middle_page_cover`和`canLoop `这3个属性来控制MZBannerView 是用作Banner还是普通ViewPager,能控制多种Banner展示效果：**

1 . 魅族Banner 效果，中间Page覆盖两边。

```java
 app:open_mz_mode="true"
 app:canLoop="true"
 app:middle_page_cover="true"
```

![Page 覆盖效果](http://upload-images.jianshu.io/upload_images/3513995-39f495bf8a915ad0.gif?imageMogr2/auto-orient/strip)

2 普通banner 使用。

```java
app:open_mz_mode="false"
app:canLoop="true"
```
![普通Banner效果](http://upload-images.jianshu.io/upload_images/3513995-39f495bf8a915ad0.gif?imageMogr2/auto-orient/strip)

上图中的底部BannerView 示例。

3 仿魅族Banner 效果，中间Page不覆盖。

```java
 app:open_mz_mode="true"
 app:canLoop="true"
 app:middle_page_cover="false"
```

![Page不覆盖效果](http://upload-images.jianshu.io/upload_images/3513995-9aa19c594f932982.gif?imageMogr2/auto-orient/strip)

4 仿爱奇艺Banner效果，Page 之间有间隔。

```java
 <com.zhouwei.mzbanner.MZBannerView
       android:id="@+id/banner_normal"
       android:layout_width="match_parent"
       android:layout_height="150dp"
       android:layout_marginTop="10dp"
       app:open_mz_mode="true"
       app:middle_page_cover="false"
       app:canLoop="true"
       app:indicatorAlign="center"
       />
```

除了上面的代码外，还需要在Page 的item 布局里面设置左右Margin:

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="match_parent"
              android:layout_height="match_parent">
    <ImageView
        android:id="@+id/banner_image"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scaleType="centerCrop"
        android:layout_marginLeft="4dp"
        android:layout_marginRight="4dp"
        />

```
效果如下：

![仿爱奇艺Banner效果](http://upload-images.jianshu.io/upload_images/3513995-f753f5449512c06c.gif?imageMogr2/auto-orient/strip)

5 有魅族Banner 效果的普通ViewPager 使用


```java
 app:open_mz_mode="true"
 app:canLoop="false"
```

![魅族Banner效果的普通ViewPager](http://upload-images.jianshu.io/upload_images/3513995-29d27def0753dd86.gif?imageMogr2/auto-orient/strip)

6 普通ViewPager 使用

```java
 app:canLoop="false"
 app:open_mz_mode="false"
```

 ![普通ViewPager](http://upload-images.jianshu.io/upload_images/3513995-6cb035771cda9870.gif?imageMogr2/auto-orient/strip)

上面都是用Banner 展示的本地数据，但是项目中我们一般都是从网络获取Banner 数据，具体参考：`RemoteTestFragment.java`



### 其他对外API
```java
    /******************************************************************************************************/
    /**                             对外API                                                               **/
    /******************************************************************************************************/
    //开始轮播
     start()
    //停止轮播
     pause()

    //设置BannerView 的切换时间间隔
     setDelayedTime(int delayedTime)
    // 设置页面改变监听器
    addPageChangeLisnter(ViewPager.OnPageChangeListener onPageChangeListener)

    //添加Page点击事件
     setBannerPageClickListener(BannerPageClickListener bannerPageClickListener)
    //设置是否显示Indicator
    setIndicatorVisible(boolean visible)
    // 获取ViewPager
    ViewPager getViewPager()
    // 设置 Indicator资源
    setIndicatorRes(int unSelectRes,int selectRes)
    //设置页面数据
    setPages(List<T> datas,MZHolderCreator mzHolderCreator)
    //设置指示器显示位置
    setIndicatorAlign(IndicatorAlign indicatorAlign)
    //设置ViewPager（Banner）切换速度
    setDuration(int duration)
```
因为是对ViewPager的包装，所有要设置某些ViewPager的属性，可以通过getViewPager 获取到ViewPager再设置对应属性
 
