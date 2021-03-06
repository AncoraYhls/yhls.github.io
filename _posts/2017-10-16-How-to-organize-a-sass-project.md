---
title: 如何组织一个sass项目
description: 
categories: technology
tags: SASS
---

> SASS文档教程看了很多，也写了一些demo，但如何在项目中使用仍然是个问题。于是找了很多资料，看了一些源码，总结出了一些组织SASS项目的经验，分享给大家。

> 使用SASS的原因，除了各种变量，宏，函数等一些预处理操作，我觉得最主要其实是SASS文件可拆分，可以像JS模块化一样进行管理。组织一个好的SASS项目，对样式的编写，管理和维护都非常便利。

> 本次介绍主要针对单项目组织文件，核心思想是将样式文件分为page,compents,layout,modules等部分，用文件夹分割开来，最后在一个文件中统一引入进行编译。

以下是我平时项目中常用的组织样式

样例
```
sass/ 
| 
| modules/ 
| | _reset.scss
| | _normailze.scss
| ... # Etc 
| 
| components/ 
| | _buttons.scss 
| | _banner.scss 
| | _dropdown.scss 
| ... # Etc 
| 
| helpers/ 
| | _variables.scss  
| | _functions.scss 
| | _mixins.scss 
| | _helpers.scss 
| ... # Etc 
| 
| layout/ 
| | _grid.scss  
| | _header.scss  
| | _footer.scss 
| | _sidebar.scss  
| | _forms.scss 
| ... # Etc 
| 
| pages/ 
| | _home.scss  
| | _contact.scss 
| ... # Etc 
| 
| vendors/ 
| | _bootstrap.scss
| ... # Etc 
| 
| 
` main.scss 
```
.scss文件前面有一个下划线(_)是用来告诉Sass,这些.scss文件只是局部,不通过@import是不应该被编译出.css文件。事实上,它们是导入和合并文件的基本文件而以.

modules里存放的是reset,normalize以及一些可能会用到的IE hack写法。

compents里存放的是一些通用的UI组件样式，比如button,banner,dropdown等我都会把它们分类放到一个文件，结合mixin，管理和复用非常方便。

helpers里面存放的是变量，宏，函数等，整个项目的通用变量都可以放到variable.scss里面，值得一提的是图片的路径也可以把它作为变量，开发阶段用本地相对路径，测试部署环节再把它替换为cdn路径，只用一次编译而无需查找替换。
```scss
// _variables
//$baseimgurl: '../images';
$baseimgurl: 'cache.xxx.com/project';

// _home
.a {
    background-image: url(#{$baseimgurl}/b.png);
}
```

layout存放的是一些布局样式，pages存放页面样式，vendors存放第三方样式。

main.scss(注意前面没有_)，作为SASS项目入口，不放任何样式文件，只引入其它文件。
```scss
// Modules 
@import "mudules/reset"; 

// pages 
@import "pages/home";
// ... 

```

最后，编译就好了，加上sourceMap，调试管理更加方便了。

> 其它

如果觉得新建这些文件夹太累的话，推荐gulp的mkdirp插件，谁用谁知道。。。

参考:

[Nested selectors: the inception rule](http://thesassway.com/beginner/the-inception-rule)

[CLEAN OUT YOUR SASS JUNK-DRAWER](http://gist.io/4436524)

[如何组织一个Sass项目](http://www.w3cplus.com/preprocessor/beginner/how-to-structure-a-sass-project.html)

[管理Sass项目文件结构](http://www.w3cplus.com/preprocessor/architecture-sass-project.html)