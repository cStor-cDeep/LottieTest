###Lottie是什么？
Lottie是Airbnb开源的一个支持 Android、iOS 以及 ReactNative，利用json文件的方式快速实现动画效果的库。这么看可能很难理解，接下来我将详细的讲解如何使用。
Lottie项目地址：[https://github.com/airbnb/lottie-android](https://github.com/airbnb/lottie-android)
***
首先先无耻的把我自己写的demo程序和源码放上来。
* Demo体验apk下载地址:   https://fir.im/5j4e
* Demo程序的github地址 ：   https://github.com/panacena/LottieTest/


###Lottie如何使用？
#####一、Lottie能干什么？
在回答Lottie能干什么之前，我们先想下如下的动画如何实现？

![如何实现上方的动画效果?](https://raw.githubusercontent.com/panacena/LottieTest/master/gif/test.gif)
***
我想大概有几种方式:
* 使用帧动画。这种方式固然可行，但是一个需要动画添加很多张图片，势必会导致apk体积变大，并且还要根据不同的尺寸进行适配。
* 用 Gif。但是使用 Gif 占用空间较大，而且需要为各种屏幕尺寸、分辨率做适配，并且Android本是不支持gif直接展示的。
* 用代码加图片辅助。如之前写的 [仿照驾校一点通欢迎页](http://blog.csdn.net/zhang58246500/article/details/45248701)，这种方式繁琐并且每更新一次都需要重新写很多代码。
* Android 5.x 之后提供了对 SVG 的支持，通过 VectorDrawable、AnimatedVectorDrawable 的结合可以实现一些稍微复杂的动画，但是问题和前2个类似。
***
那么有没有什么方式是即可以方便的实现动画效果，又可以不用考虑适配的问题，而且Android、ios还可以兼容呢?

**Lottie就是支持Android, iOS, 和React Native,并且只需简单的代码就可以实现复杂动画效果的库**
#####二、Lottie在Android端怎么用？

假设我们要做一个缓冲数据时的一个loading动画，不用Lottie之前你们公司的美工一般都会给一个gif动画效果和一些切好的一帧一帧的图片。现在不需要这么操作了，只要你们公司的美工做如下的操作：
#####下面是公司帅帅的美工的工作
1. 让设计师使用Adobe 的 After Effects(简称 AE)工具(美工一般都会这个)制作这个动画。
AE的下载地址:链接： http://pan.baidu.com/s/1misKrzU 密码：f2t7  (安装破解什么的就自行百度吧)
2. 在AE中安装一个叫做Bodymovin的插件。
 下载  **[bodymovin](https://github.com/bodymovin/bodymovin)**，解压缩后只需要\build\extension\bodymovin.zxp这个档案就可以[![1478641334-9003-svgani1](http://upload-images.jianshu.io/upload_images/2825714-deb183aaf24cd84e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://a.mq2014.com/wp-content/uploads/2016/11/1478641334-9003-svgani1.png)
3.手动安装plugin，以windows系统而言，要先下载  **[ExMan Command Line tool](http://www.adobeexchange.com/ExManCmd_win.zip) **并解压缩。
再来把下载的bodymovin压缩后的 bodymovin-master\build\extension 目录下的bodymovin.zxp 这个档案复制进去同一个资料夹。[![1478641336-7754-svgani3](http://upload-images.jianshu.io/upload_images/2825714-a30115bf703e314a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://a.mq2014.com/wp-content/uploads/2016/11/1478641336-7754-svgani3.png)
 
4.去找cmd，并以系统管理员身分执行。[![1478641339-9310-svgani4](http://upload-images.jianshu.io/upload_images/2825714-d832631082636e12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://a.mq2014.com/wp-content/uploads/2016/11/1478641339-9310-svgani4.png)
 
5.打“cd C:/ExManCmd_win *所在的路径* “，进入ExManCmd的资料夹中[![1478641340-6123-svgani5](http://upload-images.jianshu.io/upload_images/2825714-58a6f07c4dd615e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://a.mq2014.com/wp-content/uploads/2016/11/1478641340-6123-svgani5.png)
 
6.接着打  ExManCmd.exe /install bodymovin.zxp  就完成了[![1478641343-7665-svgani6](http://upload-images.jianshu.io/upload_images/2825714-deaca5473619952c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://a.mq2014.com/wp-content/uploads/2016/11/1478641343-7665-svgani6.png)
 
7.再来进入AE 后，可以在windows/extentions/bodymovin 找到插件，开启后按下Render 就完成了。 **重点来了，这时会在你选的Destination Folder目录中生成一个json格式的文件，这个 json 文件描述了该动画的一些关键点的坐标以及运动轨迹。请帅帅的美工把这个文件发给你苦逼的程序员兄弟吧！** [![1478641345-4251-svgani7](http://upload-images.jianshu.io/upload_images/2825714-27d8195af7dde6ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://a.mq2014.com/wp-content/uploads/2016/11/1478641345-4251-svgani7.png)

***

#####下面就是苦逼Android程序员应该如何做咯

1.在build.gradle中添加
```
dependencies {  
  compile 'com.airbnb.android:lottie:1.0.1'
}
```
2.layout文件中添加
```
    <com.airbnb.lottie.LottieAnimationView
        android:id="@+id/animation_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:lottie_fileName="hello-world.json"
        app:lottie_loop="true"
        app:lottie_autoPlay="true" />
```
3.将帅帅的美工给你的json文件重命名为xml中lottie_fileName的值，如上就是hello-world.json。将hello-world.json放入 app/src/main/assets目录中。打包吧！！

**然后你就会发现奇迹出现了，没有一张图片，没有一个gif，但是动画效果出来了！就是这么简单，就是这么暴力！**
***
#####三、Lottie进阶，如何更加高效和方便的实用？

* 最简单的使用方式就是上方直接在xml中定义的方式。
* 使用代码的方式，支持从assets目录中直接读取json文件、json字符串的方式、stream流的方式等
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2825714-5575e563871db68b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 从网络获取json文件，直接显示动画。**这种方式很炫，你就可以不用不更新apk就不动声色的定期更新你的动画了。**
下方是我写的一个小demo，使用okhttp访问网络上一段json文件，然后显示动画。
```
client.newCall(request).enqueue(new Callback() {
            @Override public void onFailure(Call call, IOException e) {

            }

            @Override public void onResponse(Call call, Response response) throws IOException {
                if (!response.isSuccessful()) {
                }

                try {
                    JSONObject json = new JSONObject(response.body().string());
                    LottieComposition.fromJson(getResources(), json, new LottieComposition.OnCompositionLoadedListener() {
                                @Override
                                public void onCompositionLoaded(LottieComposition composition) {
                                    setComposition(composition);
                                }
                            });
                } catch (JSONException e) {
                }
            }
        });
```

#####四、Lottie例子程序
为了更好的让你了解这个库，我写了一个简单的demo，请大家帮忙点star  ! 

* Demo体验apk下载地址: http://fir.im/5j4e
* Demo程序的github地址 ：  https://github.com/panacena/LottieTest/


***
######感激
*感谢以下文章:*
[这个项目碉堡了](https://gold.xitu.io/post/58948f1b0ce4630056f3a629)   
[After Effect 转svg 动画– 神奇的bodymovin 插件](http://www.mq2014.com/after-effect-zhuan-svg-dong-hua-shen-qi-de-bodymovin-cha-jian.html)

