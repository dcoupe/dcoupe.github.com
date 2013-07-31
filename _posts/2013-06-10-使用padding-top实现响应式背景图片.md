---
layout : post
tltle : 使用padding-top实现响应式背景图片
tags : 响应式
---
处理响应式布局中背景图片的简单方法是等比例缩放背景图片。我们知道宽度设为百分比的`<img>`元素，其高度会随着宽度的变化自动调整，且其宽高比不变。如果想在背景图片中实现同样的效果，我们必须先解决如何保持HTML元素的宽高比。

##固定宽高比
我们将用到一个保持元素宽高比的技巧：为元素添加垂直方向的`padding`值，`padding`值使用百分比。这是因为垂直方向的`padding`取值使用百分比时，其值是相对于包含块的宽度而定的[参考Box model](http://www.w3.org/TR/CSS2/box.html#padding-properties)。这个技巧最早在[Creating Intrinsic Ratios for](http://www.alistapart.com/articles/creating-intrinsic-ratios-for-video/) Video一文中用来创建固有比率的视频，[查看demo](http://alistapart.com/d/creating-intrinsic-ratios-for-video/example5.html)。

![padding-top](http://p11.qhimg.com/t01c27ad220b5a838d5.png)

假设我们有一张800*450px的图片，我们需要创建一个元素在其宽度变化时，它的宽高比仍保持16:9。代码如下：

```html
<div class="column">
  <div class="figure"></div>
</div>
```

```css
.column{
  max-width: 800px;
}
.figure{
  padding-top: 56.25%;  /* 450px/800px = 0.5625 */
}
```
[自己动手试试吧](http://voormedia.com/blog/2012/11/responsive-background-images-with-fixed-or-fluid-aspect-ratios/example-basic)

##添加背景图片

上面我们实现了元素缩放并保持宽高比。但是此时如果我们添加了背景图片，它并不能跟随元素一起自动缩放。还需要添加`background-size:cover`。使用这个属性让背景铺满元素的缺点是IE8及以下浏览器不支持，为了使IE下的效果可以接受，可以使用`background-position`将背景图片居中显式。我们必须要保证图片的宽度至少要与元素的`max-width`一样大。这样的话元素的宽度永远不会比图片大，如果元素小于图片时，图片将被裁剪。

```css
div.column {
  /* The background image must be 800px wide */
  max-width: 800px;
}

figure.fixedratio {
  padding-top: 56.25%;  /* 450px/800px = 0.5625 */
  background-image: url(img/sample.jpg);
  background-size: cover;
  background-position: center;  /* Internet Explorer 7/8 */
}
```
[自己动手试试吧](http://voormedia.com/blog/2012/11/responsive-background-images-with-fixed-or-fluid-aspect-ratios/example-with-image)

##流动宽高比

我们可以更深入一步。假设我们有一张在桌面浏览器下显式很好的宽屏图片，在移动设备上我们不想使用相同的宽高比，要不然图片会很小。又或者是我们不想使用相同的高度，因为图片可能会过高。

![padding-top](http://p11.qhimg.com/t01ce51648707bdd386.png)

这个效果可以通过较少`padding`的百分比值和为元素设置一个高度来实现。假设我们的大图是800*200px，我们打算在元素的宽度减少到300px的时候，背景图片的高度为150px。现在我们计算下`height`和`padding-top`属性值。

![padding-top](http://p11.qhimg.com/t01bac6036b1a8cbc66.png)

上图显式了两个尺寸的关系。坡度线(slop)对应于`padding-top`属性，开始高度(start height)对应于`height`属性，它表示元素在宽度为零时的高度。调整样式如下：

```css
div.column {
  /* The background image must be 800px wide */
  max-width: 800px;
}
figure.fluidratio {
  padding-top: 10%;  /* slope */
  height: 120px;  /* start height */
  background-image: url(img/sample.jpg);
  background-size: cover;
  background-position: center;  /* Internet Explorer 7/8 */
}
```
[自己动手试试吧](http://voormedia.com/blog/2012/11/responsive-background-images-with-fixed-or-fluid-aspect-ratios/example-with-fluid-image)

##REF:

+[Responsive background images with fixed or fluid aspect ratios](http://voormedia.com/blog/2012/11/responsive-background-images-with-fixed-or-fluid-aspect-ratios)
+[Creating Intrinsic Ratios for Video](http://alistapart.com/article/creating-intrinsic-ratios-for-video)