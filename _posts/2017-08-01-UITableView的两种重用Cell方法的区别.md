---
layout:     post
title:      "UITableView的两种重用Cell方法的区别"
subtitle:   ""
date:       2017-7-8 13:14:00
author:     "Lus"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - iOS
---


## 写在前面
在使用UITableView的时候, 需要面临`两种重用Cell`方法的选择, 这就需要了解`什么时候使用什么方法`, 以及`两种方法到底做了什么`.
## 区别
* 两种方法的的选择情况

>在iOS 6中`dequeueReusableCellWithIdentifier:`被`dequeueReusableCellWithIdentifier:forIndexPath:`所取代。

* 优缺点对比

>如此一来，`dequeueReusableCellWithIdentifier:forIndexPath:`在表格视图中创建并添加`UITableViewCell`对象会变得更为精简而流畅。

* `dequeueReusableCellWithIdentifier:forIndexPath:`做了什么

> `dequeueReusableCellWithIdentifier:forIndexPath:`一定会返回cell，系统在默认没有cell可复用的时候会自动`创建一个新的cell出来`。


## dequeueReusableCellWithIdentifier:forIndexPath:的使用

* 使用`dequeueReusableCellWithIdentifier:forIndexPath:`的话，必须和下面的两个配套方法`配合起来`使用

```js
// Beginning in iOS 6, clients can register a nib or class for each cell.
// If all reuse identifiers are registered, use the newer -dequeueReusableCellWithIdentifier:forIndexPath: to guarantee that a cell instance is returned.
// Instances returned from the new dequeue method will also be properly sized when they are returned.
- (void)registerNib:(UINib *)nib forCellReuseIdentifier:(NSString *)identifier NS_AVAILABLE_IOS(5_0);
- (void)registerClass:(Class)cellClass forCellReuseIdentifier:(NSString *)identifier NS_AVAILABLE_IOS(6_0);

```
* 这样在`tableView:cellForRowAtIndexPath:`方法中就可以省掉下面这些代码


```js
static NSString *CellIdentifier = @"Cell";
 if (cell == nil){
     cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:CellIdentifier];}
```
* 取而代之的是下面这句代码：

```js
UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"Cell" forIndexPath:indexPath];
```
## More
在`tableView:cellForRowAtIndexPath:`中使用`dequeueReusableCellWithIdentifier:forIndexPath:`获取重用的cell，如果没有重用的cell，将自动使用`提供的class类`创建cell并返回

* 纯代码的情况

```js
CustomCell *cell = [tableView dequeueReusableCellWithIdentifier:kCellIdentify forIndexPath:indexPath];
```
获取cell时如果没有可重用的cell，将调用cell中的`initWithStyle:withReuseableCellIdentifier:`方法创建新的cell

