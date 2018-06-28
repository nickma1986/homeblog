---
layout: post
title: React搭建需求列表
publised: false
categroy: [jekyll, test]
---

## React搭建需求管理列表
Node.js + React + MongoDB

>express -e feed
>npm install
>npm install react react-dom webpack
>npm start
//重新打包
>webpack -w

#React路由
文档 https://react-guide.github.io/react-router-cn/index.html
两种方式
Link
history.push
withRouter(MyComponent)组件，包裹自定义组件，自动获得router属性

#React页面间传值
1. props.params
2. query
3. state
4. onclick

#nodejs配置log4js
http://blog.fens.me/nodejs-log4js/
>npm install log4js --save //安装到工程目录

>npm i antd -S
>npm i babel-plugin-import -D
>.roadhogrc
>{
  "extraBabelPlugins": [
    ["import", {
      "libraryName": "antd",
      "libraryDirectory": "lib",
      "style": "css"
    }]
  ]
}
>npm install less
>npm install less-loader
>npm install style-loader
>npm install css-loader

#css相关知识
>height: 8vp; //视口被分为100份，即100vp
