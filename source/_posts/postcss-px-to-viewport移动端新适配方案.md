---
title: postcss-px-to-viewport移动端新适配方案
tags:
  - 移动端
  - H5
category: 技术
abbrlink: 2099ebba
date: 2019-12-31 15:18:40
---

## postcss-px-to-viewport移动端新适配方案 

之前移动端项目一直用的是pxtorem来实现适配的，但是发现在某些情况下，rem布局会错乱，然后在vant（一款移动端ui框架）给的demo里面发现了这个宝藏插件。

* tip: [vant github地址](https://github.com/youzan/vant-demo) (里面也有rem适配demo、typescript demo)

### 1. 简介

postcss-px-to-viewport的原理其实就是把你设置的以px为单位的css属性转换为相应的视口单位。

而视口就是你的浏览器实际显示内容的区域——换句话说就是你的不包括工具栏和按钮的网页浏览器。这些单位分别是 `vw` 、 `vh` 、 `vmin` 和 `vmax` 。它们都代表了浏览器（视口）尺寸的比例和窗口大小调整产生的规模改变。

* vw：viewpoint width，视窗宽度，1vw=视窗宽度的1%。
* vh：viewpoint height，视窗高度，1vh=视窗高度的1%。
* vmin：vw和vh中较小的那个。
* vmax：vw和vh中较大的那个。

所以页面宽高也可以用height：100vh，width：100vw来表示，等同于height：100%，width：100%。

### 2. 安装

目前开发主要基于vue，所以以vuecli为例

``` bash
npm install postcss-px-to-viewport --save-dev
# cnpm install postcss-px-to-viewport --save-dev
```

### 3. 配置和使用

vuecli3以上在新建项目的时候会在根目录生成一个和vue.config.js同级postcss.config.js的文件

``` javascript
const autoprefixer = require('autoprefixer')
const pxtoviewport = require('postcss-px-to-viewport')

module.exports = {
    plugins: [
        autoprefixer(),
        pxtoviewport({
            viewportWidth: 750, // 设计稿宽度
            viewportHeight: 1334, // 设计稿高度，可不配置，默认使用vw为单位
            unitPrecision: 3, // 指定 `px` 转换为视窗单位值的小数位数
            viewportUnit: "vw", //指定需要转换成的视窗单位，建议使用vw，不加此选项默认使用vw
            selectorBlackList: ['.ignore'], // 指定不转换为视窗单位的class类样式，可以自定义，可以无限添加,建议定义一至两个通用的类名
            minPixelValue: 1, // 小于或等于 `1px` 不转换为视窗单位，你也可以设置为你想要的值
        })
    ]
}
```

如果没有生成postcss.config.js文件，也可在vue.config.js里面配置

``` javascript
const autoprefixer = require('autoprefixer');
const pxtoviewport = require('postcss-px-to-viewport');

module.exports = {
    css: {
        loaderOptions: {
            postcss: {
                plugins: [
                    autoprefixer(),
                    pxtoviewport({
                        viewportWidth: 750, // 设计稿宽度
                        viewportHeight: 1334, // 设计稿高度，可不配置，默认使用vw为单位
                        unitPrecision: 3, // 指定 `px` 转换为视窗单位值的小数位数
                        viewportUnit: "vw", //指定需要转换成的视窗单位，建议使用vw，不加此选项默认使用vw
                        selectorBlackList: ['.ignore'], // 指定不转换为视窗单位的class类样式，可以自定义，可以无限添加,建议定义一至两个通用的类名
                        minPixelValue: 1, // 小于或等于 `1px` 不转换为视窗单位，你也可以设置为你想要的值
                    })
                ]
            }
        }
    }
};
```

配置好就完工啦，可以通过浏览器查看元素确认一下有没有配置成功, 然后就可以根据设计稿进行开发啦

![demo](data:image/png;base64,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)

    

