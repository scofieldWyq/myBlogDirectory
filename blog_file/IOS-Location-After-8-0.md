title: IOS_Location_After_8.0
date: 2016-02-25 22:23:55
tags:
- IOS
- CLLocationManager
- Location
- Delegate
---

# >>> IOS 定位失败 <<<

# 目录
[A - IOS 定位过程中没有任何反应](#A)

[B - 现在我大概走一下流程](#B)

[C - 问题出现了。。。。。。。。。。。](#C)

[D - 开始解决问题](#D)

[Warning](#Warning)


# <span id="A">A - IOS 定位过程中没有任何反应</span>
> 在你很高兴的按照一些网上的教程设置好你的location之后，很不幸你却发现你的定位delegate并没有任何的响应。如果你使用button作为测试的时候，点击你的button，就是没有任何反应。

如果不想了解前提，可以直接跳到 [问题的解决方案](#dealing)
-----------------------------------------------------------

# <span id="B">B - 现在我大概走一下流程</span>

## 设置好你的代码
\> ![image](http://7xiwtw.com1.z0.glb.clouddn.com/code_1.png)

\> ![image](http://7xiwtw.com1.z0.glb.clouddn.com/code_2.png)

\> ![image](http://7xiwtw.com1.z0.glb.clouddn.com/code_3.png)

\> ![image](http://7xiwtw.com1.z0.glb.clouddn.com/code_4.png)

-------------------------------------------------------------

# <span id="C">C - 问题出现了。。。。。。。。。。。</span>
## 1.高兴之后却发现定位竟然没有任何反应。。。坑死了 -\_-!


## 2.然后就是一堆问题的搜索

![image](http://7xiwtw.com1.z0.glb.clouddn.com/code_5.png)

**然后就发现这些参考：**

 [iOS8中定位服务的变化(CLLocationManager协议方法不响应，无法回掉GPS方法，不出](http://my.oschina.net/zhhzhfya/blog/372653?p=1)
 [ios8.0下CLLocationManager定位服务需要授权了](http://blog.csdn.net/nogodoss/article/details/42268295)
 [iOS CLLocationManager定位,IOS8注意](http://www.2cto.com/kf/201504/393312.html)
 [iOS CLLocationManager定位](http://www.tuicool.com/articles/7JBRZn)
 [iOS8中定位服务的变化(CLLocationManager协议方法不响应，无法回掉GPS方法，不出现获取权限提示)](http://www.csdn123.com/html/topnews201408/70/4370.htm)

 **收获来啦**

 ![image](http://7xiwtw.com1.z0.glb.clouddn.com/code_6.png)

## 3.然后定位到 `Location Awareness PG Introduction`

 好多繁杂的搜索和过滤后，定位到了这个[Location and Maps Programming Guide](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/LocationAwarenessPG/Introduction/Introduction.html)

 ![image](http://7xiwtw.com1.z0.glb.clouddn.com/code_7.png)

 但是貌似没有发现 `8.0` 之后的说法

------------------------------------------------------

# <span id="D">D - 开始解决问题</span>

## 1.IOS 8.0 之后的改变

 很多的说法就是在 `8.0` 之后对于这一些安全措施进行了很多的设置，所以在你使用之前，需要去显式的开启它。不知道是真还是假，等以后有时间我在这个，目前还找不到。

 所以这个就先空白一下。。。。。不好意思！嘻嘻


## 2.<span id="dealing">问题的解决方案</span>

> 好，既然问题有了症状，那么就在下药。。。。

- 设置你的info.plist文件，在Surporting files 目录里面。

- 加上两个 `Key - Value`

  ![image](http://7xiwtw.com1.z0.glb.clouddn.com/code_8.png)

- 在启动视图之前做好一些准备

  ![image](http://7xiwtw.com1.z0.glb.clouddn.com/code_9.png)

- 现在开始调试

  ![image](http://7xiwtw.com1.z0.glb.clouddn.com/code_10.png)

--------------------------------------------------------

# E - Ok,我的定位也就 `working` 了, 你呢？

# <span id="Warning">Warning - 需要注意的事项</span>

- 如果你没有设置那两个 `Key-Value`在 info.plist 文件里，授权提示还是会出现，只是效果和没有设置的差不多。

- 然后呢，记得 `<CLLocationManagerDelegate>` 和 设置 `delegate` 。

- 如果使用的是模拟器，那么按照上面的步骤还不行的话，是这重置一下模拟器，我就是这么干的，it working!
