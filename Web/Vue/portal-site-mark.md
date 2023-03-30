## 创建项目
  ```shell
  npm init vite@latest portal-site -- --template vue 
  #选择y,再按提示运行
  cd portal-site
  npm install
  npm run dev

  # 配置路由
  npm install vue-router --save
  #安装axios
  npm install axios --save
  ```

## 安装element依赖
   ```shell
   npm install element-plus --save
   ```
   在main.js中调整element相关配置，同时调整style.css

## 创建联系表单
  - src/view/ContactForm.vue
  
## 创建主页组件
  - src/view/Home.vue

## 增加路由
  - src/router/index.js

## 安装配置 Axios
  - npm i axios -- save

## 配置 vite.config.js (后端地址)