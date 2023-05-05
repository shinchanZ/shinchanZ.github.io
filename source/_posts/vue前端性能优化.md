---
title: VUE前端性能优化
tag:
  - vue
  - 前端性能优化
category: 技术
abbrlink: 837eac20
date: 2019-08-22 12:39:04
---

## VUE前端性能优化
### 1.字体资源压缩
##### Fontmin（第一个纯JavaScript字体子集化方案） 
[Fontmin](https://github.com/ecomfe/fontmin/)

- node.js
```

    //安装
    npm install --save fontmin

    //使用
    var Fontmin = require('fontmin');

    var srcPath = 'src/font/*.ttf'; // 字体源文件
    var destPath = 'asset/font';    // 输出路径
    var text = 'Fontmin（第一个纯JavaScript字体子集化方案）';

    // 初始化
    var fontmin = new Fontmin()
        .src(srcPath)               // 输入配置
        .use(Fontmin.glyph({        // 字型提取插件
            text: text              // 所需文字
        }))
        .use(Fontmin.ttf2eot())     // eot 转换插件
        .use(Fontmin.ttf2woff())    // woff 转换插件
        .use(Fontmin.ttf2svg())     // svg 转换插件
        .use(Fontmin.css())         // css 生成插件
        .dest(destPath);            // 输出配置

    // 执行
    fontmin.run(function (err, files, stream) {

        if (err) {                  // 异常捕捉
            console.error(err);
        }

        console.log('done');        // 成功
    });
```
- 客户端

    [下载链接](https://github.com/ecomfe/fontmin-app/releases)

- [常用3500汉字及特殊字符](https://github.com/zdd2018/-3500-)
- [可商用免费中文字体](http://www.fonts.net.cn/commercial-free-32767/fonts-zh-4.html)
### 2.路由懒加载
##### 打包构建应用时，JavaScript 包会变得非常大，影响页面加载。路由懒加载就是把不同路由对应的组件分割成不同的代码块，当路由被访问的时候才加载对应组件。
- 传统非路由懒加载模式
```
...
import login from '@/views/login/login.vue'
...
{
    path:'/login',
    name:'login',
    component:login
}
...
```
- require.esure
```
    ...
    component: require.ensure([], () => r(require('@/views/login/login.vue')), 'index')
    ...
```
- import方式
```
    ...
    component: () => import('@/views/login/Login.vue')
    ...
```
### 3.CDN加速

##### 使用CDN资源,减小服务器带宽压力
- 在index.html中引入cdn资源

```   
    <script src="https://cdn.staticfile.org/echarts/4.1.0/echarts.min.js"></script>
```

- 配置externals(vue.config.js)
```
    configureWebpack: {
            externals: {
                'echarts': 'echarts',
            },
        }
```
### 4.gzip压缩
##### 使用nginx开启gzip减小网络传输的流量大小
- vue 配置(vue.config.js)
```
    //安装compression-webpack-plugin插件
    npm install compression-webpack-plugin -S
    //cnpm ...
```
```
    //引入
    const CompressionPlugin = require("compression-webpack-plugin")
```

```
    //配置
    configureWebpack: {
        plugins: [
            //gzip压缩配置
            new CompressionPlugin({
                filename: '[path].gz[query]',
                algorithm: 'gzip',
                test: /\.js$|\.html$|\.json$|\.css$|/,
                threshold: 0, // 只有大小大于该值的资源会被处理
                minRatio: 0.8, // 只有压缩率小于这个值的资源才会被处理
                deleteOriginalAssets: false // 删除原文件
            })

        ],
       
    }

```
- nginx 服务器配置(nginx.conf)

- 未开启状态
```
    #Gzip on
```
- 开启配置
```
    gzip on; //开启gzip

    gzip_buffers 4 16k;//设置压缩所需要的缓冲区数量及大小

    gzip_proxied       any;//Nginx作为反向代理的时候启用(expired ,no-cache ,no-store,private ,no_last_modified,no_etag,auth,any)

    gzip_comp_level 5;//压缩级别，1-9，数字越大压缩的越好，也越占用CPU时间

    gzip_vary on;//是否在http header中添加Vary:Accept-Encoding，建议开启

    gzip_http_version 1.1;//设置gzip压缩针对的HTTP协议版本

    gzip_types   text/plain application/javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png ;

    //更改配置需重启nginx方能生效
```
- 测试是否成功开启
```
    curl -I -H "Accept-Encoding: gzip, deflate" "http://192.168.1.101:6102"
```
```
    HTTP/1.1 200 OK

    Server: nginx/1.8.0

    Date: Thu, 15 Aug 2019 01:26:35 GMT

    Content-Type: text/html

    Last-Modified: Wed, 14 Aug 2019 05:57:18 GMT

    Connection: keep-alive

    Vary: Accept-Encoding

    ETag: W/"5d53a2be-152d"

    Content-Encoding: gzip
```

