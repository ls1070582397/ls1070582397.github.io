---
layout:     post
title:      "两个控件同时访问相册并赋值"
subtitle:   "a simple way of thinking"
date:       2017-10-10 13:14:00
author:     "Lus"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - iOS
---

## 壹 : 前言
项目中有一个需求: 在一个`UITableView的Cell`里面有两个按钮, 是用来上传身份证正面照 & 身份证反面照, 下面是实现思路.

![](media/14999384776526/14999387714058.jpg)


## 贰 : 两个按钮都访问相册
>当两个按钮都访问相册时, 考虑到按钮含有tag属性, 所以就使用tag值. 
>声明一个int 类型的tag 属性, 然后给按钮的响应方法传入此参数

```js
#pragma mark - 访问相册&相机
- (void)didClickCardBtn: (UIButton *)sender {
    
    self.tag = sender.tag;
    // 访问相册及相机action
    [self showActionSheet];
}
```

## 叁 : 访问相册及相机的回调
访问相册及相机的成功回调用到`UIImagePickerControllerDelegate`的一个方法

```
- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary<NSString *,id> *)info;
```

## 肆 : 遇到的另外一个问题
>此时会遇到一个问题: 在按钮一成功赋值图片后, 回调后, reloadData, 
>此时再给按钮二赋值图片的时候, 回调后, reloadData, `按钮一的图片会消失`

主要思路: 

* 声明两个UIImage属性`img1, img2`记录`身份证正面照按钮` & `身份证反面照按钮`
* 通过此时控制器的`tag值` 来判断是哪个按钮访问了相机成功回调了代理方法.
* 如果是按钮1访问了相册, 回调成功并`给按钮一赋值完图片`后, 
* `reloadData`, 
* 然后将`self.img2 赋值给按钮二`
* 最后把新拿到的回调的图片对象`editedImage赋值给记录self.img1`
* 如果点击了按钮2 ` 同理`.

## 伍 : 最终代码

```js
/** 记录正面照*/
@property (nonatomic,strong) UIImage *img1;

/** 记录反面照*/
@property (nonatomic,strong) UIImage *img2;
```

```js
#pragma mark - 访问相册&相机
- (void)didClickCardBtn: (UIButton *)sender {
    
    self.tag = sender.tag;
    // 访问相册及相机action
    [self showActionSheet];
}
```


```js
- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary<NSString *,id> *)info;{
    
    //获得编辑后的图片
    UIImage *editedImage = (UIImage *)info[UIImagePickerControllerEditedImage];
    
    [picker dismissViewControllerAnimated:YES completion:^{
        
        NSIndexPath *indexPath = [NSIndexPath indexPathForRow:4 inSection:0];
        
        CHInfoCell *infoCell = [self.infoTableView dequeueReusableCellWithIdentifier:infoCellID forIndexPath:indexPath];
        
        if (self.tag == infoCell.Btn1.tag) {
            //给按钮一赋值图片
            [infoCell.Btn1 setImage:editedImage forState:UIControlStateNormal];
            //reloadData
            [self.infoTableView reloadRowsAtIndexPaths:[NSArray arrayWithObjects:indexPath, nil] withRowAnimation:UITableViewRowAnimationNone];
            //将`self.img2 赋值给按钮二`
            [infoCell.Btn2 setImage:self.img2 forState:UIControlStateNormal];
            //新拿到的回调的图片对象`editedImage赋值给记录self.img1`
            self.img1 = editedImage;

        }
        if (self.tag == infoCell.Btn2.tag) {
            
            [infoCell.Btn2 setImage:editedImage forState:UIControlStateNormal];
            
            [self.infoTableView reloadRowsAtIndexPaths:[NSArray arrayWithObjects:indexPath, nil] withRowAnimation:UITableViewRowAnimationNone];

            [infoCell.Btn1 setImage:self.img1 forState:UIControlStateNormal];
            
            self.img2 = editedImage;

        }
        
    }];
}
```



