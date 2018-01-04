---
layout:     post
title:      "视图声明周期方法执行顺序"
subtitle:   "老生常谈"
date:       2017-07-08 13:14:00
author:     "Lus"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - iOS
---

>当一个视图控制器被创建, 并在屏幕上显示的时候, 代码的执行顺序如下:

* `alloc `        创建对象, 分配空间

* `init`(`initWithNibName`) 初始化对象, 初始化数据

* `loadView`      从nib加载视图, 通常这一步不需要去干涉. 除非你没有使用xib文件创建视图
* `viewDidLoad`   载入完成, 可以进行自定义数据以及动态创建其他控件
* `viewWillAppear`视图将出现在屏幕之前, 马上这个视图就会被展现在屏幕上了
* `viewDidAppear` 视图已经在屏幕上渲染完成

>当一个视图被移除屏幕并且销毁的时候的执行顺序, 这个顺序差不多和上面相反

* `viewWillDisAppear`  视图将被从屏幕上移除以前执行



* `viewDidDissAppear`  视图已经从屏幕上移除完成, 用户看不到这个视图了
* `delloc `            视图被销毁


