---
title: Rem布局
date: 2019-04-08 16:54:23
tags:
---
# 一、rem是什么：
rem（font size of the root element）是css3的一个长度单位，相对文档跟元素 html；比如设置html font-size=100px;那么1rem=100px;之后的所有元素都可以用这个基准值来设置大小；

# 二、rem使用方式：

### 1、Css：
html {
  font-size: 80px;
}

@media screen and (min-width: 320px) {
  html {
    font-size: calc(563% + 10 * (100vw - 320px) / 39);
    font-size: calc(90px + 10 * (100vw - 320px) / 39);
  }
}

@media screen and (min-width: 375px) {
  html {
    font-size: calc(625% + 20 * (100vw - 375px) / 39);
    font-size: calc(100px + 20 * (100vw - 375px) / 39);
  }
}

@media screen and (min-width: 414px) {
  html {
    font-size: calc(750% + 40 * (100vw - 414px) / 186);
    font-size: calc(120px + 40 * (100vw - 414px) / 186);
  }
}

@media screen and (min-width: 600px) {
  html {
    font-size: calc(1000% + 40 * (100vw - 600px) / 425);
    font-size: calc(160px + 40 * (100vw - 600px) / 425);
  }
}

@media screen and (min-width: 1025px) {
  html {
    font-size: calc(1250% + 60 * (100vw - 1000px) / 1000);
    font-size: calc(20px + 60 * (100vw - 1000px) / 1000);
  }
}
vw是代表视窗的宽度，基于浏览器内部的可视区域大小innerWidth，100vw为整个视区的100%宽度。如今的vw除了ie8下以及Opera Mini之外的浏览区都兼容了。

### 2、Js：
window.onload=function(){
  (function() {
    var docEls = document.documentElement
    var newRem = function() {
      var html = document.documentElement;
      html.style.fontSize = window.innerWidth / 10 + 'px';
    };
    window.addEventListener('resize', newRem, false);
    newRem();
  })();
}
这段js需要设置meat标签，可以通过屏幕大小改变时改变font-size的大小。

# 三、兼容性
ios：6.1系统以上都支持
android：2.1系统以上都支持
大部分主流浏览器都支持

# 四、设置字体大小
根据网页的根元素来设置字体大小，和em（font size of the element）的区别是，em是根据其父元素的字体大小来设置，而rem是根据网页的跟元素（html）来设置字体大小的

# 五、rem进行屏幕适配
移动端适配的方法，一般可分为以下几种：
1、简单一点的页面，一般高度直接设置成固定值，宽度一般撑满整个屏幕。
2、稍复杂一些的是利用百分比设置元素的大小来进行适配，或者利用flex等css去设置一些需要定制的宽度。
3、再复杂一些的响应式页面，需要利用css3的media query属性来进行适配，大致思路是根据屏幕不同大小，来设置对应的css样式。

# 六、rem适配
根据网页根元素字体大小来确定元素尺寸

# 七、rem数值计算
如果利用rem来设置css的值，一般要通过一层计算才行，比如如果要设置一个长宽为100px的div，那么就需要计算出100px对应的rem值是 100 / 16 =6.25rem

# 八、对于没有使用sass的工程：
为了方便起见，可以将html的font-size设置成100px，这样在写单位时，直接将数值除以100在加上rem的单位就可以了。

# 九、对于使用sass的工程：
前端构建中，完全可以利用scss来解决这个问题，例如我们可以写一个scss的function px2rem即：
@function px2rem($px){
  $rem : 37.5px;
  @return ($px/$rem) + rem;
}

# 十、rem基准值计算

关于rem的基准值，也就是上面那个37.5px其实是根据我们所拿到的视觉稿来决定的，由于我们所写出的页面是要在不同的屏幕大小设备上运行的，所以我们在写样式的时候必须要先以一个确定的屏幕来作为参考，这个就由我们拿到的视觉稿来定，假如我们拿到的视觉稿是以iphone6的屏幕为基准设计的，iPhone6的屏幕大小是375px

rem = window.innerWidth  / 10
这样计算出来的rem基准值就是37.5（iphone6的视觉稿），这里为什么要除以10呢，其实这个值是随便定义的,因为不想让html的font-size太大，当然也可以选择不除，只要在后面动态js计算时保证一样的值就可以动态设置html的font-size

1、利用css的media query来设置即
@media (min-device-width : 375px) and (max-device-width : 667px) and (-webkit-min-device-pixel-ratio : 2){
  html{font-size: 37.5px;}
}
2、利用javascript来动态设置 根据我们之前算出的基准值，我们可以利用js动态算出当前屏幕所适配的font-size即
.con {
  width: px2rem(200px);
  height: px2rem(200px);
  background-color: red;
}

document.addEventListener('DOMContentLoaded', function(e) {
  document.getElementsByTagName('html')[0].style.fontSize = window.innerWidth / 10 + 'px';
}, false);

由此可见我们可以通过设置不同的html基础值来达到在不同页面适配的目的，当然在使用js来设置时，需要绑定页面的resize事件来达到变化时更新html的font-size。

# 十一、rem适配进阶

一般我们获取到的视觉稿大部分是iphone6的，所以我们看到的尺寸一般是双倍大小的，在使用rem之前，我们一般会自觉的将标注/2，其实这也并无道理，但是当我们配合rem使用时，完全可以按照视觉稿上的尺寸来设置。
1、设计给的稿子双倍的原因是iphone6这种屏幕属于高清屏，也即是设备像素比(device pixel ratio)dpr比较大，所以显示的像素较为清晰。
2、一般手机的dpr是1，iphone4，iphone5这种高清屏是2，iphone6s plus这种高清屏是3，可以通过js的window.devicePixelRatio获取到当前设备的dpr，所以iphone6给的视觉稿大小是（*2）750×1334了。
3、拿到了dpr之后，我们就可以在viewport meta头里，取消让浏览器自动缩放页面，而自己去设置viewport的content例如（这里之所以要设置viewport是因为我们要实现border1px的效果，加入我给border设置了1px，在scale的影响下，高清屏中就会显示成0.5px的效果）。
meta.setAttribute('content', 'initial-scale=' + 1/dpr + ', maximum-scale=' + 1/dpr + ', minimum-scale=' + 1/dpr + ', user-scalable=no');
4、设置完之后配合rem，修改
@function px2rem($px){
  $rem : 75px;
  @return ($px/$rem) + rem;
}

# 十二、三种适配方式

### 1、通用方案
1：设置根font-size：625%（或其它自定的值，但换算规则1rem不能小于12px）
2：通过媒体查询分别设置每个屏幕的根font-size
3：css直接除以2再除以100即可换算为rem

优：有一定适用性，换算也较为简单。
劣：有兼容性的坑，对不同手机适配不是非常精准；需要设置多个媒体查询来适应不同手机，单某款手机尺寸不在设置范围之内，会导致无法适配。

### 2、网易方案
1：拿到设计稿除以100，得到宽度rem值
2：通过给html的style设置font-size，把1里面得到的宽度rem值代入x document.documentElement.style.fontSize = document.documentElement.clientWidth / x + 'px';
3：设计稿px/100即可换算为rem

优：通过动态根font-size来做适配，基本无兼容性问题，适配较为精准，换算简便。
劣：无viewport缩放，且针对iPhone的Retina屏没有做适配，导致对一些手机的适配不是很到位。

### 3、手淘方案
1、拿到设计稿除以10，得到font-size基准值
2、引入flexible
3、不要设置meta的viewport缩放值
4、设计稿px/ font-size基准值，即可换算为rem

优：通过动态根font-size、viewpor、dpr来做适配，无兼容性问题，适配精准。
劣：需要根据设计稿进行基准值换算，在不使用sublime text编辑器插件开发时，单位计算复杂。

# 十三、总结
首先根据屏幕尺寸确定font-size基准值（如window.innerWidth / 10）,然后根据font-size基准值确定rem

当用作图片或者一些不能缩放的展示时，必须要使用固定的px值，因为缩放可能会导致图片压缩变形等。