# Vue

## 名词概念
   - **vite** 是一种快速、轻量级的前端开发构建工具，它的目标是提供一种快速启动和快速重载的开发体验。Vite最初由Evan You（Vue.js的创始人）创建，但它不仅仅适用于Vue.js，也可以用于React、Angular等其他前端框架。和Vue cli 的效果一样，vite 也是一个构建 vue 项目的脚手架工具。比起 Vue cli，vite 的热更新速度更快，打包构建速度更快。因为它默认安装的插件很少，所以需要自己额外配置。
   - npm
   - yarn
   - pnpm


## pnpm命令
  ```shell
  # 安装pnpm
  npm i pnpm -g
   
  #查看版本信息：
  pnpm -v

  # 升级版本
  pnpm add -g pnpm to update 

  # 设置源
  pnpm config get registry
  #切换淘宝源
  pnpm config set registry https://registry.npmmirror.com 

  #安装项目依赖 
  pnpm install

  #运行项目
  pnpm run dev

  ```

## Vue组件
  - [element-plus](https://element-plus.gitee.io/zh-CN/) 基于 Vue 3.0 的桌面端组件库
  - [vant](https://vant-contrib.gitee.io/vant/v3/#/zh-CN) vant3.0版本，有赞前端团队开源移动端组件库
  - [ant-design-vue](https://2x.antdv.com/components/overview/) Ant Design Vue 2.0版本，社区根据蚂蚁 ant design 开发

---
## 参考资料
  - [超详细！10分钟开发一个Vue3的后台管理系统](https://juejin.cn/post/7175760401701797947)

---

- [返回首页](../../README.md)