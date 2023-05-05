---
title: js操作vue文件里的变量和方法
tag:
  - javascript
  - vue
category: 技术
abbrlink: 34e0fdd2
date: 2020-04-05 11:50:35
---

# js获取vue文件里的this对象（用于访问vue文件里的变量和方法）

## 1.js文件里写入sendThis方法

``` javascript
//model.js文件
//status.vue里的this对象
let vm = null
const sendThis = (_this) => {
    vm = _this
}
export default {
    sendThis，
    changeStatus() {
        vm.status = true
        vm.select()
    }
}
```

## 2.vue文件里调用sendThis方法

``` javascript
//status.vue
...
import model from '@/utils/model'
export default {
    ...
    data() {
        return {
            status: false
        }
    },
    mounted() {
        model.sendThis()
    },
    methods: {
        select() {},
        change() {
            model.changeStatus()
        }
    }
}
...
```

## 3. 然后js文件就可以直接通过vm来操作vue文件的方法和变量了

