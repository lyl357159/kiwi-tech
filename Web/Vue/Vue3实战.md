- [Vue3实战](#vue3实战)
  - [1、环境准备](#1环境准备)
    - [1.1安装nvm](#11安装nvm)
    - [1.2 安装Node.js](#12-安装nodejs)
  - [2.搭建脚手架项目](#2搭建脚手架项目)
    - [2.1创建脚手架项目](#21创建脚手架项目)
    - [2.2 安装依赖](#22-安装依赖)
    - [2.3 启动](#23-启动)
  - [Vue项目理解](#vue项目理解)
  - [附录](#附录)
    - [1.nvm常用命令](#1nvm常用命令)
  - [参考资料](#参考资料)

---
# Vue3实战

## 1、环境准备
  - 电脑： MacBook Pro Apple M1 Max
  
### 1.1安装nvm
  - 1.移除可能已经存在的Node版本
  ```shell
   #Remove existing Node Versions
   brew uninstall --ignore-dependencies node 
   brew uninstall --force node 
  ```
  - 2.安装NVM到Mac OS
    - 使用brew安装mvm
      ```shell
       #brew update 
       brew install nvm 
      ```
    - 配置NVM环境变量
      ```shell
       vim ~/.bash_profile 
      ```
       添加如下内容
      ```shell
       export NVM_DIR=~/.nvm
       source $(brew --prefix nvm)/nvm.sh
      ```
      刷新环境变量
      ```shell
      # source ~/.bash_profile 
      # 新版本的macOS Catalina开始，新用户的默认shell改为了zsh对于zsh，使用.zshrc, 已手动在.zshrc中source ~/.bash_profile
      source ~/.zshrc
      ```
   - 3.检查nvm是否已安装成功
     ```shell
      nvm -v
      > 0.39.3
     ```
     **Note**: [附录：nvm常用命令](#1nvm常用命令)

 ### 1.2 安装Node.js
   ```shell
    #查看所有网上可安装的node版本
    nvm ls-remote 
    # 安装了指定版本Node
    # 2022-7-12的最新LTS:16.16.0
    # 2022-2-23的最新LTS:v18.14.2 
    nvm install 16.17.0
    #看安装的所有node.js的版本
    nvm ls
     #显示当前版本
    nvm current
    > v16.17.0
    # 切换使用指定的版本node
    nvm use <version>
    #永久node版本切换
    nvm alias default v16.17.0
    nvm use v16.17.0

    # 查看npm版本(npm是随node绑定安装的)
    npm -v 
    > 8.15.0
   ```
   ---

## 2.搭建脚手架项目
### 2.1创建脚手架项目
   ```shell
   # vite 也是一个构建 vue 项目的脚手架工具,和 Vue cli 的效果一样, 参考：https://cn.vitejs.dev/guide/

   # npm 6.x
   npm init vite@latest vue3-kiwi --template vue

   # npm 7+ & npm 8+
   npm init vite@latest vue3-kiwi -- --template vue 
   # 会提醒需要安装, 直接y
   > Need to install the following packages:
   > create-vite@4.1.0
   > Ok to proceed? (y) y
   ```
### 2.2 安装依赖
   ```shell
    cd vue3-kiwi
    npm install
   ```
### 2.3 启动
   ```shell
    npm run dev
   ```
   ![image](../Resources/../../Resources/Web/Vue/vue-vite.png)


## Vue项目理解
  - index.html
    > 启动页，声明了id为app的元素，用于挂载，并引用了/src/main.js
   ```html
    <body>
     <div id="app"></div>
     <script type="module" src="/src/main.js"></script>
   </body>
   ```
   - src/main.js
   > 属于TypeScript脚本，引入了vue.d.ts, 并关联了./App.vue
   > Vue3 中的应用是通过使用 createApp 函数来创建的,传递给 createApp 的选项用于配置根组件。在使用 mount() 挂载应用。createApp 的参数是根组件（App），在挂载应用时，该组件是渲染的起点。#app就是index.html中的`<div id="app"></div>`
   ```js
   import { createApp } from 'vue'
   import App from './App.vue'

   const app = createApp(App)
   app.mount('#app')
   ```
   - src/App.vue
   >  router-view组件作为vue最核心的路由管理组件，在项目中作为路由管理经常被使用到。vue项目最核心的App.vue文件中即是通过router-view进行路由管理。需要`npm install vue-router --save`来提供
   ```
   <template>
      <div>
        <router-view></router-view>
      </div>
   </template>
   ```
   - 安装element依赖
   ```shell
   npm install element-plus --save
   ```
   - .配置路由
   > 提供了router-view组件
   ```
   npm install vue-router --save 
   ```
   - 在 src 文件夹下新建 router 文件夹，然后新建 index.js
   ```js
   import { createRouter, createWebHashHistory } from "vue-router"
   const routes = [ ]
   const router = createRouter({
    history:createWebHashHistory(),
    routes
    })
    
    //导出路由
    export default router;
   ```
   - 在 main.js 中配置路由
     ```js
     import router from './router/index.js'
     app.use(router).use(ElementPlus, {locale})
     ```
  

---
## 附录
### 1.nvm常用命令
   ```shell
    #查看所有网上可安装的node版本
    nvm ls-remote 
    #安装了指定版本(2022-7-12的最新LTS:16.16.0)
    nvm install 16.16.0
    #看安装的所有node.js的版本
    nvm ls
    #是查找本电脑上所有的node版本
    nvm list 
    #查看已经安装的版本
    nvm list installed 
    #查看网络可以安装的版本 
    nvm list available 
    #安装最新版本nvm
    nvm install 
    # 切换使用指定的版本node
    nvm use <version> 
    #列出所有版本
    nvm ls 
    #显示当前版本
    nvm current
    #给不同的版本号添加别名
    nvm alias <name> <version> 
    # 删除已定义的别名
    nvm unalias <name> 
    #在当前版本node环境下，重新全局安装指定版本号的npm包
    #nvm reinstall-packages <version>
    #打开nodejs控制
    nvm on 
    #关闭nodejs控制
    nvm off 
    #查看设置与代理
    nvm proxy 
    #设置或者查看setting.txt中的node_mirror，如果不设置的默认是 https://nodejs.org/dist/
    nvm node_mirror [url] 
    #设置或者查看setting.txt中的npm_mirror,如果不设置的话默认的是： https://github.com/npm/npm/archive/.
    nvm npm_mirror [url] 
    #卸载制定的版本
    nvm uninstall <version> 
    #切换制定的node版本和位数
    nvm use [version] [arch] 
    #设置和查看root路径
    nvm root [path] 
    #永久node版本切换
    nvm alias default v9.4.0
    nvm use v9.4.0
   ```

--- 
 - vue缩写
   - v-bind 缩写
     Vue.js 为两个最为常用的指令提供了特别的缩写：
   ```js
    <!-- 完整语法 -->
    <a v-bind:href="url"></a>
    <!-- 缩写 -->
    <a :href="url"></a>
    v-on 缩写
    <!-- 完整语法 -->
    <a v-on:click="doSomething"></a>
    <!-- 缩写 -->
    <a @click="doSomething"></a>
   ```



 
 ## 参考资料
 - [element-plus](https://element-plus.gitee.io/zh-CN/) 基于 Vue 3.0 的桌面端组件库
  - [超详细！10分钟开发一个Vue3的后台管理系统](https://juejin.cn/post/7175760401701797947)
  - [Vue中文文档](https://cn.vuejs.org/guide/introduction.html)
  - [Vue3 目录结构](https://www.runoob.com/vue3/vue3-directory-structure.html)