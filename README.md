需求背景：如何用css来制作这样的一张优惠券？尤其是金棕色的齿轮部分：（设计稿宽高为690px * 210px）


我想到的最直接快速的方法就是用background的URL来设置：直接把下面整个图片作为元素的背景，搞定！(宽高为690px * 210px，注意四边有1px,#e6e6e6,6px的圆角描边)




这样固然不失为一种快速实现的办法，但能否优化下呢？加载这么大一个图片，消耗的资源（加载速度、带宽等）肯定不少，能否只加载图片的一部分呢？

手工PS制作一张这样的图片：(宽高为210px * 32px)

实现的原理是：通过border属性来进行图片的拉伸。
CSS代码：(border-image的数值是border-width数值的200倍)
[css] view plain copy print?
.cell {  
    position: relative;  
    height: 100%;  
}  
  
.cell::before {  
    content: " ";  
    overflow: hidden;  
    position: absolute;  
    top: 0;  
    left: 0;  
    right: 0;  
    bottom: 0;  
    border-width: .03rem .03rem .03rem 1.01rem;  
    border-style: solid;  
    border-image: url("coupon_border_b59f76.png")  6 6 6 202 repeat round;  
}  
注意部分：
border-width: .03rem .03rem .03rem 1.01rem;
border-style: solid;
border-image: url("coupon_border_b59f76.png") 6 6 6 202 repeat round;

至于图片的宽高为什么是210px * 32px呢？
1、高度为32px：一个齿轮的高度为30px,加上上下各1px的描边，所以总高度为32px


2、宽度为210px：
2.1棕色部分的宽度为200px

2.2左边有1px的描边

2.3右边有6px的圆角

2.4中间留3px的带上下1px描边的白色距离用于横向拉伸（实际上只需要留1px即可，但因为我要适配移动端，移动端的Retina屏幕如果只拉伸1px，会模糊掉，所以一般针对移动端需要留2px来拉伸以至于像素不会模糊。另外这里又多留了1px是因为移动端适配，需要用设计稿的像素宽度除以2，若是209px除以2会有小数，我就在可以循环的部分多留了1px。）

这时候，200+1+6+3=210px，就是所需要的宽度。
