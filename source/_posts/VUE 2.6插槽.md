---
title: VUE 2.6 插槽
tag:
  - vue
category: 技术
abbrlink: 83bae515
---

## VUE 2.6 插槽

插槽（Slot）是Vue提出来的一个概念，正如名字一样，插槽用于决定将所携带的内容，插入到指定的某个位置，从而使模板分块，具有模块化的特质和更大的重用性。插槽显不显示、怎样显示是由父组件来控制的，而插槽在哪里显示就由子组件来进行控制

##### 1 匿名插槽（默认插槽）
 匿名的方式，就是指把在引用组件的时候，里面传的内容，全部一起传送到组件页面中 <slot></slot> 所在的位置。

```html
    //childComponent
    <div>
        <h2>我是子组件</h2>
        <slot ></slot>
        <p>我是子组件的内容</p>
    </div>
```

```html  
    //parentComponent
    <child-component >
    <h1>我是匿名插槽</h1>
    <p>我会在子组件指定位置显示</p>
    </child-component>
```
注：
    -  只要组件中有 <slot></slot> ，并且不管有多少个，都会全部渲染为传过来的内容。
    -  <slot></slot>可以设置内容，这个内容是保证引入组件的时候，有个默认值。当然<slot></slot>里面不设置内容也可以，这样只是没有默认值，是不会报错的
    -  如果传递的内容，没有slot 来接收，那么，传递的内容就会被抛弃掉，不会起作用


##### 2 具名插槽

当我们组件需要用到多个插槽的时候, 需要用到具名插槽: 通过v-slot指令来命名
v-slot:header 或缩写的形式#header，header不能加引号

```html 
    //childComponent
    <header>
        <slot name="header"></slot>
    </header>
    <main>
        <slot></slot>
    </main>
    <footer>
        <slot name="footer"></slot>
    </footer>
```

```html
    //parentComponent
    <child-component >
    <template v-slot：header>
        <h1>这里是header的插槽内容</h1>
    </template>
    
    <p>这里是默认插槽的插槽内容</p>
    <p>And another one.</p>
    
    <template #footer>
        <p>这里是footer的插槽内容</p>
    </template>
    </child-component>
```

##### 3 作用域插槽

有时候需要让插槽内容能够访问到子组件中才有的数据, 需要用到作用域插槽:

```html
    //childComponent
    //默认插槽
    <div>
        <slot  :user="user"></slot>
    </div>
    //具名插槽
    <div>
        <slot name="userInfo" :user="user"></slot>
    </div>
```

```html
    //parentComponent
   
    <child-component >
     //默认插槽
        <template v-slot="slotProps">
        {{ slotProps.user.age }}
        </template>
    //具名插槽
        <template v-slot:userInfo="slotProps">
        {{ slotProps.user.age }}
        </template>
    </child-component>
```

##### 4 Vue2.6之前和2.6之后插槽的区别

 区别主要体现在具名插槽和作用域插槽的写法：
 - 具名插槽 

    （1） slot=' xxx ' 改成了 v-slot : xxx 并且冒号后面这个名称不能带引号

    （2） 子组件页面中slot的内容没有变化

    （3） 2.6 之后具名插槽 v-slot:header 可以缩写为 #header , #后面有参数才能这样写， # = "xxx " 是错误的 #default = ‘xxx’ 这样才可以


```html
    <child-component >
        //2.6之前的写法
        <template  slot='header'>
            <p>这里是footer的插槽内容</p>
        </template>
    
    
        //2.6之后的写法
        <template v-slot:header>
            <p>这里是footer的插槽内容</p>
        </template>
    </child-component>
      
```

* 作用域插槽

    （1） 两个属性合并成了一个 v-slot : 插槽名称 = ' 传过来的值 ' 
    
    （2） 组件页面中slot的内容没有变化 
    
    （3） v-slot 不能用在 html 标签上 
    
    （4） 如果是默认插槽 可以写成 v-slot='xxx'
    

```html
    <child-component >
        //2.6之前的写法
        //默认插槽
         <div  slot-scope='slotProps'>
                {{ slotProps.user.age }}
        </div>
        //具名插槽
        <div slot='listbox' slot-scope='slotProps'>
                {{ slotProps.user.age }}
        </div>
        <template slot='listbox' slot-scope='slotProps'>
                {{ slotProps.user.age }}
        </template>
        
        //2.6之后的写法
        //默认插槽
        <template v-slot="slotProps">
                {{ slotProps.user.age }}
        </template>
        //具名插槽
        <template v-slot:userInfo="slotProps">
                {{ slotProps.user.age }}
        </template>
    </child-component>
```
