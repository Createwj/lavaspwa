# pwa-project

> 这是一个 Lavas PWA 项目

## npm 相关命令

``` bash
# 安装工程依赖
npm install

# 在本地启动调试 server
npm run dev

# 构建线上生产环境产物
npm run build

# 启动编译后的代码，注意，需要在 dist 目录中启动，仅 SSR 模式下有效
npm run start

# 检查代码是否符合规范
npm run lint
```

Lavas 工程模版基于 [vue-template-vue](https://github.com/lavas-project/lavas-template-vue/) 模版的 `release-*` 分支创建。
如果想要了解具体如何玩转整个 Lavas 创建的 PWA 工程, 请查看[Lavas 官网教程](https://lavas.baidu.com/guide)。

## Changelog

详细的 Changelog 请看 [Release Notes](https://github.com/lavas-project/lavas-template-vue/releases)


# 初始化基本目录简介

```
lavas-project
├── assets/
├── components/
├── core/
├── middlewares/
├── node_modules/
├── pages/
├── static/
├── store/
├── lavas.config.js
├── server.dev.js, server.prod.js
├── .babelrc, .editorconfig, .fecsignore, .fecsrc, .gitignore
└── LINCENSE, package.json, README.md

```

#### assets  static

- /assets 里的内容会被 webpack 构建到生成目录的文件中，不再会单独以文件形式存在。因此 iconfont 放置在 /assets 中

- /static 里的内容会被原样复制到生成目录中，会以独立的文件形式存在。因此 PWA 用到的 manifest.json 和一系列图标等都放置在 /static 中。


#### components
 
存放其他页面复用的组件


UpdateToast.vue

在core/app.vue 中被使用 提醒用户 server work更新的时候提醒用户

ProgressBar.vue

页面加载的时候 顶部的进度条



#### core

存放一些散落但必须的文件


#### midddlewares
https://lavas.baidu.com/guide/v2/advanced/middleware

用户可以在这个目录编写自定义中间件，用来获取或者修改每次到达服务器的网络请求


#### pages

```/pages/``` 目录存放每个页面的vue文件 开发与vue-cli组件一模一样

pages的所有文件 都会自动生成一条路由 无需用户进行配置

开发者也可以自定义路由https://lavas.baidu.com/guide/v2/advanced/router


#### store
 
vuex模块化加载

在core/app.js 中批量加载
一个标准的vuex模块包括
```
export const state = {}

export const mutations = {}

export const action = {}


```

#### lavas.config.js 配置项目基本选项

https://lavas.baidu.com/guide/v2/basic/init#lavasconfigjs


#### 其他选项 

- LINCENSE, package.json, README.md 

这些文件也不属于 Lavas，开发者可以根据自己情况自行修改。


- .babelrc, .editorconfig, .fecsignore, .fecsrc, .gitignore

这些文件实际上并不属于 Lavas，而是项目通用文件，分别处理 babel 对 es6/7 的转码规则，IDE 的配置，fecs 代码风格检查的配置和 git 的忽略文件，开发者一般不需要修改。


- server.dev.js, server.prod.js

这两个文件作用于用户输入命令 lavas dev 和 lavas start 时配合 Lavas 使用的，用户一般不需要修改。Lavas 集成的命令简介将在 Lavas 命令中介绍。



#### .lavas

项目打包一次 根目录会产生一个 .lavas目录

这个目录是 Lavas 在运行之前事先生成的，如之前提到过的自动生成的路由规则，
以及处理热加载 (hotreload) 等都放置在这里，开发者不应该修改这个目录的内容，
并且将目录留在 /.gitignore 中，防止协作开发时互相影响。



### lavas 自动生成路由

- 普通情况
```
/pages/Appshell.vue => /appshell
/pages/Error.vue => /error
```

- index.vue
```
/pages/Index.vue => /
```

- 动态参数
我们可以在 /pages/detail/ 目录中建立一个 vue 文件并命名为 _id.vue (下划线开头表示动态参数)。
在这个 vue 文件中，我们可以通过 this.$route.params.id( script 中 )或者 {{$route.params.id}}( template 中 ) 获取这个参数。


- 嵌套路由
https://lavas.baidu.com/guide/v2/basic/init#%E5%B5%8C%E5%A5%97%E8%B7%AF%E7%94%B1
Lavas 同样支持 vue-router 的嵌套路由。开发者需要在 /pages 目录中新建两个同名的目录和 Vue 文件(但首字母大小写不同，目录小写，文件大写)即可实现。
```
lavas-project
└── pages/
    ├── Detail.vue
    └── detail/
        ├── Product.vue
        ├── Service.vue
        └── Mixed.vue
```
Detail.vue

```
<template>
    <header>xxx</header>
    <router-view></router-view>
</template>
```
使用vue-router来描述的话 如下
```
let routes = [{
    path: '/detail',
    name: 'detail',
    component: xxx,
    children: [{
        path: 'product',
        name: 'detailProduct',
        component: xxx
    },{
        path: 'service',
        name: 'detailService',
        component: xxx
    },{
        path: 'mixed',
        name: 'detailMixed',
        component: xxx
    }]
}]
```


# lavas 基本命令

##### lavas init

![](https://gss0.bdstatic.com/9rkZbzqaKgQUohGko9WTAnF6hhy/assets/pwa/projects/1515680651550/lavas-init.png)


##### lavas build

开始构建项目
使用 Lavas 对项目进行构建，内部会调用 babel, webpack 等，最终生成在 /dist/ 目录中 (可以通过 /lavas.config.js 进行修改)
![](https://gss0.bdstatic.com/9rkZbzqaKgQUohGko9WTAnF6hhy/assets/pwa/projects/1515680651531/lavas-build.png)


指定配置文件进行打包
```
lavas build config/lavas.another.config.js
```


##### lavas dev

lavas dev config/lavas.another.config.js

lavas dev --port 8000


##### lavas start

打包之后进入dist目录 运行项目 可以提前看到线上的效果



##### lavas static
启动服务器访问静态资源



#### core 目录详解
https://lavas.baidu.com/guide/v2/advanced/core




# 注意

asyncData 中访问不到this对象


























