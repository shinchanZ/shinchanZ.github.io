---
title: 基于VUE的H5页面制作
tag:
  - vue
  - 移动端
  - H5
category: 技术
abbrlink: 5d88249d
date: 2019-08-22 12:39:04
---
## 基于VUE的H5页面制作
#### 一、vuecli3的搭建
```
                vue ui

```


#### 二、slide整屏翻页的实现
swiper轮播插件--[swiper官网](https://www.swiper.com.cn/)

######  1.安装和配置

```
                npm install vue-awesome-swiper --save
```

```
                import VueAwesomeSwiper from 'vue-awesome-swiper'
                import 'swiper/dist/css/swiper.css'

                Vue.use(VueAwesomeSwiper, /* { default global options } */)
```
###### 2.使用
```
                //引入
                import {swiper, swiperSlide} from 'vue-awesome-swiper'

```
```
                //注册组件
                components: {
                          swiper,
                          swiperSlide,
                      },
```
```
                //HTML部分
                <swiper :options="swiperOption" ref="myswiper">
                <swiperSlide style="background-color: orange">第一页</swiperSlide>
                <swiperSlide style="background-color: blue">第二页</swiperSlide>
                <swiperSlide style="background-color: gray">第三页</swiperSlide>
                </swiper>
```
```
              //配置部分
              data(){
                return{
                swiper: null,
                swiperOption: {
                    // swiper configs 所有的配置同swiper官方api配置
                    speed: 600,
                    direction: 'vertical',
                    autoHeight: true,
                    slidesPerView: 1,
                    mousewheel: true,
                    height: window.innerHeight,
                    resistanceRatio: 0.85,
                    observeParents: true,//
                    effect: 'coverflow',
                    coverflowEffect: {
                        rotate: 10,
                        stretch: 0,
                        depth: 200,
                        modifier: 2,
                        slideShadows: true
                    },

                }
              }
              },


```

```
              //swiperAnimate动画的应用
              //引入
              npm install vue2-animate --save
              import 'vue2-animate/dist/vue2-animate.min.css';
              import * as swiperAni from '@/utils/swiper.animate.min.js'


              //html
              <p class="ani" swiper-animate-effect="fadeInLeft" swiper-animate-duration="0.6s"
                      swiper-animate-delay="0.6s">我是谁</p>
              <p class="ani" swiper-animate-effect="fadeInLeft" swiper-animate-duration="0.6s"
                      swiper-animate-delay="1.2s">我在哪</p>
              <p class="ani" swiper-animate-effect="fadeInLeft" swiper-animate-duration="0.6s"
                      swiper-animate-delay="1.8s">我要去哪</p>


              //载入动画配置
              on: {
                    init: function () {
                    swiperAni.swiperAnimateCache(this); //隐藏动画元素
                    swiperAni.swiperAnimate(this); //初始化完成开始动画
                          },
                    slideChangeTransitionEnd: function () {
                     swiperAni.swiperAnimate(this); //每个slide切换结束时也运行当前slide动画
                        }
                  }


```
  [swiperAnimatejs文件下载](https://www.swiper.com.cn/download/index.html)

  [animate动画效果](https://daneden.github.io/animate.css/)

  #### 三、移动端适配（px转rem）
  ###### 1.引入lib-flexible
  ```
              //安装
              npm i lib-flexible --save
              //引入
              import 'lib-flexible/flexible'
  ```
  ###### 2.设置meta标签
  通过meta标签，设置设备宽度以及缩放比例
  ```
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  ```
  ###### 3.pxtorem
  ```
              //安装
              npm install postcss-pxtorem -S
              //配置（根目录postcss.config.js）
              "postcss": {
                  "plugins": {
                    "autoprefixer": {},
                    "postcss-pxtorem": {
                      "rootValue": 75 ,// 设计稿宽度的1/10,
                      "propList":['*'],// 需要做转化处理的属性，如`hight`、`width`、`margin`等，`*`表示全部
                   }
                  }
                },
  ```
