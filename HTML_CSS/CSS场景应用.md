# CSS场景应用

## 实现一个三角形

```css
div {
    width: 0;
    height: 0;
    border: 100px solid;
    border-color: orange blue red green;
}
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cba8731fea9842a8b8103c2b387fe64f~tplv-k3u1fbpfcp-watermark.awebp)

```css
div {    
  width: 0;    
  height: 0;    
  border-top: 50px solid red;    
  border-right: 50px solid transparent;   
  border-left: 50px solid transparent;
}
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ab996951a0cc42cf9e6d9e12eb827f8b~tplv-k3u1fbpfcp-watermark.awebp)

```css
div {
    width: 0;
    height: 0;
    border-top: 100px solid red;
    border-right: 100px solid transparent;
}
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a1ac630463164e42a027b54bb95f56ba~tplv-k3u1fbpfcp-watermark.awebp)



## 实现一个扇形

```css
div{
    border: 100px solid transparent;
    width: 0;
    height: 0;
    border-radius: 100px;
    border-top-color: red;
}
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/db5e46aea0ce4805a0c2bbec2743546e~tplv-k3u1fbpfcp-watermark.awebp)



##  画一条0.5px的线

CSS中设置height: 0.5px，结果仍然是1px的，可通过1px后再缩小解决;

- **采用transform: scale()的方式**，该方法用来定义元素的2D 缩放转换：

```css
transform: scale(0.5,0.5);
```

- **采用meta viewport的方式**

```css
<meta name="viewport" content="width=device-width, initial-scale=0.5, minimum-scale=0.5, maximum-scale=0.5"/>
```

这样就能缩放到原来的0.5倍，如果是1px那么就会变成0.5px。viewport只针对于移动端，只在移动端上才能看到效果。



## 设置小于12px的字体

在谷歌下css设置字体大小为12px及以下时，显示都是一样大小，都是默认12px，思路：使用css3的transform缩放属性-webkit-transform:scale(0.5); display：block / inline-block;。

```css
font-size: 12px;
display: inline-block;
transform: scale(0.5,0.5);
-webkit-transform: scale(0.5,0.5);
//此时字体为6px
```



