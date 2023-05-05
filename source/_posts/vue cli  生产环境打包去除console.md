---
title: vue cli  生产环境打包去除console
tags:
  - vue
  - console
category: 技术
abbrlink: a78f843
date: 2020-08-20 18:33:20
---


# vue cli  生产环境打包去除console



## 1. vue cli 2.x

> /build/webpack.prod.conf.js



```js
   new UglifyJsPlugin({
      uglifyOptions: {
        compress: {
          warnings: false,
          drop_debugger: true,
          drop_console: true
        }
      }
    })
```



## 2.vue cli 3+

### 2.1 安装

```bash
npm install terser-webpack-plugin --save-dev /cnpm .../  yarn add  terser-webpack-plugin
```



### 2.2 配置

>/vue.config.js

```javascript
module.export = {
  configureWebpack: (config)=>{
    if(process.env.NODE_ENV === 'production'){
      config.optimization.minimizer[0].options.terserOptions.compress.drop_console = true
    }
  }
}
```

