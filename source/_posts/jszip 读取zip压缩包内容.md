---
title: jszip 读取zip压缩包内容
tags:
  - javascript
  - jszip
category: 技术
abbrlink: a5a1446b
date: 2020-08-15 20:50:40
---

# jszip 读取zip压缩包内容


### 1.要求

> - 后端文件以流的形式返回
> - 只能是zip包

### 2. 设置接收类型

```javascript
  	url: 'api/v1/***',
    method: 'GET',
    params: {},
    responseType: "arraybuffer"//主要是请求时加上这一行
```



### 3.使用jszip解压读取

#### 1）安装jszip

```bash
npm install jszip
```

#### 2)引入并使用

```javascript
import JSZip from 'jszip' 
...

// 使用axios请求获取
apiGetZip(query).then(response => {
          let files = new Blob([response.data], {type: 'application/zip'});
          // let files = new window.File([response.data], '截图', {type: 'zip'})
          // 读取zip压缩文件里面的文件内容
          let zip = new JSZip()
          zip.loadAsync(files).then((zip) => {
            console.log(zip)
            for (let key in zip.files) {
              if (!zip.files[key].dir) {
                if (/\.(png|jpg|jpeg|gif)$/.test(zip.files[key].name)) { // 判断是否是图片格式
                  let base = zip.file(zip.files[key].name).async(
                    'base64') // 将图片转化为base64格式
                  base.then(res => {
                    this.imgList.push({
                      fileName: zip.files[key].name,
                      type: 'img',
                      content: `data:image/png;base64,${res}`
                    })
                  })
                }}
            }
          })
})
```





