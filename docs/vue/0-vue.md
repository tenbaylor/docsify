## 开发环境


- 语言：Node.js（14.15.4）
- IDE ：Visual Studio Code
- ​

## 前端技术架构

- 基础框架：Vue：2.6.11
- 运行环境：Node.js：14.15.4
- 包管理工具：yarn：1.22.4 
- UI框架：Ant-Design-Vue：1.7.2
- 路由：Vue-Router：3.5.1
- 路由守卫：nprogress：0.2.0
- 状态管理与缓存：Vuex：3.6.2
- 本地缓存：Vue-ls：3.2.2
- http请求：Axios：0.21.0
- 打包工具：Webpack：7.1.2
- 代码格式规范：eslint：6.7.2



## 前端代码结构
```
#
fte_manage_main
├── node_modules （依赖包）
├── public （公共资源、主文件目录）
├── src （源码）
│   ├── api （接口管理）
│   ├── assets（静态资源）
│   ├── components （公共组件）
│   ├── config （路由配置 ）
│   ├── i18n （语言配置）
│   ├── layouts （页面容器组件）
│   ├── mixins （公共混入文件）
│   ├── store （状态、缓存管理）
│   ├── utils （js工具包）
│   ├── views （页面组件）
│   ├── App.vue （主渲染组件）
│   ├── main.js （入口文件）
│   └──  permission.js （路由守卫控制文件）
├── .gitignore （git忽略文件）
├── .dockerignore （docker忽略文件）
├── .gitignore （git忽略文件）
├── .editorconfig （编辑器配置）
├── babel.config.js （高级语法转低级语法）
├── package.json （项目管理、node模块包管理）
├── package-lock.json （锁版本包 ）
├── README.md （项目说明 ）
├── yarn.lock （锁版本包 ）
└── vue.config.js （项目自定义配置文件 ）
```


