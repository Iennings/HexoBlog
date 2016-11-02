---
title: 第一次Android学习总结
date: 2016-05-22 22:09:31
categories:
tags: Android
---
# 今天总结的是由郭霖大神写的《第一行代码Android》
这本书很值得新人去看我的第一本Android书就是这个，在一个月前就看完了，可是一直没有时间去总结这本书的知识点！这对于学习来说很是被动的，不去总结很难获得什么经验，特别是对于我这种记忆力不知道有多差的人来说更是灾难！首先上一张图，我自己画的思维导图:
<!--more-->
![第一行代码](http://o8r84kul8.bkt.clouddn.com/first_Android.png)

## 我们就从这思维导图的顺序来总结吧
### 第一个是Activity(活动)
我觉得直接翻译为活动对于新手来说有点迷糊，我觉得理解为界面的意思比较好，因为它在书上的定义是:
	它是一种可以包含用户界面的组件，主要用于和用户进行交互。
再来总结一下这一章的知识点
1. 隐藏标题栏
` requestWindowFeature(Window.FEATURE_NO_TITLE)`
这句代码是放在`SetContentView()`之前执行的，不然会报错!
2. Toast 的使用
Toast 是Android提供的一种提醒方式，一般代码如下
```
Toast.makeText(FirstActivity.this, "提醒的信息(Alert Message)",Toast.LENGTH_SHORT).show()
```
其中FirstActivity.this是一个Toast要求的上下文对象，Toast.LENGTH_SHORT可以是设置为消息提醒留的时间更长Toast.LENGTH_LONG。
3. 活动中使用菜单Menu
菜单在Layout文件夹下建立Menu给出如下代码
```
	<menu xmlns:android="http://schemas.android.com/apk/res/android">
		<item
			android:id="@+id/add_item"
			android:title="Add"/>
		<item
			android:id="@+id/remove_item"
			android:title="Remove"/>
	</menu>
	public boolean onCreateOptionsMenu(Menu menu) {
		getMenuInflater().inflate(R.menu.main, menu);
		return true;
	}
```
上面的<item>是标签用于创建某个具体的菜单项，然后是重写onCreateOptionsMenu返回为true时才能显示菜单，返回为false时显示不了菜单，还有重写显示菜单响应事件，重写onOptionsItemSelected()方法
4. 销毁活动用finish()方法
5. 使用Intent，Intent(Context packageContext, Class<?>cls) 第一个参数是要求启动活动的上下文，第二个参数是要启动的目标活动。上面这个是显示Intent,接下来是隐式的Intent,直接是在AndroidManifest.xml文件里要启动的目标活动里添加代码
```
	<intent-filter>
		<action android:name="com.example.activitytest.ACTION_START" />
		<category android:name="android.intent.category.DEFAULT" />
	</intent-filter>
	Intent intent = new Intent("com.example.activitytest.ACTION_START");
	startActivity(intent);
```
每个Intent只能指定一个action,但是可以指定多个category
6. Intent的putExtra()方法是用于活动之间传送数据的putExtra()接收两个参数，是键值对，第一个参数是键，第二个参数是值就是所谓的传送的数据,返回数据给上一个活动需要再这基础上加一个setResult()方法，有两个参数，第一个参数是向上一个活动返回处理结果，一般使用RESULT_OK, RESULT_CANCELED两个参数，第二个就是Intent的对象，携带数据返回上一个活动
7. Activity生命周期，一张图表示吧
![Activity_period](http://o8r84kul8.bkt.clouddn.com/Activity.png)
8. onSaveInstanceState()方法携带一个Bundle类型的参数，onSavedInstance()方法用于保存临时数据
9. 活动启动模式分四种,1.standard这个是默认的模式，会重复创建位于栈顶的Activity；2.singleTop当活动不是位于栈顶时可以接着创建新的该活动；3. SingleTask. 当启动一个活动时会在栈堆里查找有没有这个活动，如果有就会创建，否则不会创建，会调用存在的活动; 4.SingleInstance具有SingleTask的功能外，还具有单独新建一个返回栈用来管理这个各个应用可以调用的活动，包括自己的应用也是可以调用的！
### 2.控件
1. 常见控件介绍TextView 文本框,Button 按钮,EditText 编辑框,ImageView,ProgressBar,AlertDialog,ProgressDialog, ListView等，其实第一行代码里的知识不是很多，《Android疯狂讲义》这本书可以当做我们的字典用，介绍了比较多，当然可以使用Google文档，我现在还在看这本书，但是自己缺少实践，需要加强实践！
2. 四大基本布局1. LinearLayout 线性布局, TableLayout 表格布局, RelativeLayout 相对布局, FrameLayout 帧布局
3. ListView是Android最常用的控件之一，也差不多算是最难用的吧，如何给ListView传递数据呢？可以有很多选择，如果是数组，那么数组里的数据怎么传进去呢？那就用适配器，可以用ArrayAdapter
4. 尺寸和单位 在手机上一般用dp和sp，dp可以在不同密度的屏幕中显示比例保持一直，sp是可伸缩像素，采用和dp一样的设计理念，解决文字适配问题
5. 制作Nine-Patch图片，可以指定哪些区域可以被拉长，那些区域不可以，用在气泡聊天上就不错，在SDK中的tools文件夹里找到draw9patch.bat打开就是这个工具！
### 3.Fragment
1. Fragment 碎片化管理，现在使用的频率比Activity还高，可以嵌入在Activity里面，Fragment动态添加碎片，
	1. 创建待添加的碎片实例。
	2. 获取到FragmentManager,在活动中可以直接调用getFragmentManager()方法得到。
	3. 开启一个事物，通过调用beginTransaction()方法开启。
	4. 向容器内添加碎片，一般使用replace()方法实现，需要传入容器的id和待添加的碎片实例。
	5. 提交事务，调用commit()方法来完成。
2. Fragment的生命周期：
![Fragment_period](http://o8r84kul8.bkt.clouddn.com/Fragment.png)
	1. onAttach()
		当碎片和活动建立关联的时候调用。
	2. onCreate()
		为碎片创建视图(加载布局)时调用。
	3. onActivityCreated()
		确保与碎片相关联的活动一定已经创建完毕的时候调用。
	4. onDestroyView()
		当与碎片关联的视图被移除的时候调用。
	5. onDetach()
		当碎片和活动接触关联的时候调用。
3. Fragment使用限定符进行动态加载，限定符可以根据屏幕大小，分辨率，方向使用，根据屏幕自动判断，适合的
# 总结的知识点暂时先到这里，还是以后分块来讲，这次从图中了解大概吧！
