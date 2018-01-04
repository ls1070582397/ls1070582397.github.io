---
layout:     post
title:      "防止tableview的sectionHeaderView跟随屏幕滑动而移动"
subtitle:   ""
date:       2017-11-21 13:14:00
author:     "Lus"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - iOS
---



`转自简书:`[如何解决tableView的section header不随cell一起滚动的问题](http://www.jianshu.com/p/264bd4d33c7f)


## 适用场景: 
通过创建sectionHeaderView会比在tableview的cell里面创建视图方便的多, 而且不希望sectionHeaderView跟随屏幕滑动而滑动

* 原型图及思路
    ![](media/15112294490194/15112304091312.jpg)

> 将红色框内区域用sectionHeaderView展示, 并且不让这个区域内容跟随屏幕滑动一直显示. 

## 效果图如下
    ![简书02](media/15112294490194/%E7%AE%80%E4%B9%A602.gif)



## code
```js
#pragma mark - ``````````````
- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    
    if (scrollView == self.parentInfoTableView)
    {
        
        CGFloat sectionHeaderHeight = 94;
        if (scrollView.contentOffset.y <= sectionHeaderHeight && scrollView.contentOffset.y >= 0)
        {
            scrollView.contentInset = UIEdgeInsetsMake(-scrollView.contentOffset.y, 0, 0, 0);
        }
        else if (scrollView.contentOffset.y >= sectionHeaderHeight)
        {
            scrollView.contentInset = UIEdgeInsetsMake(-sectionHeaderHeight, 0, 0, 0);
        }
    }
}

```

