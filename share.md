## 什么是weex
weex 是阿里出品的一个类似RN的框架，可以使用前端技术来开发移动应用，
实现一份代码支持H5，IOS和Android。最新版本的weex已默认将vue.js作为
前端框架，而weex-hacknews则是weex官方出品的，首个使用 Weex 和 Vue
 开发的 Hacker News 原生应用，在项目中使用了 Vuex 和 vue-router等官
 方组件 。因此这个应用可以作为weex-vue开发的典范，分析该项目代码可以了解如何使用
 weex、vue技术栈进行开发，实现同一份代码在 iOS、Android、Web 下都能完整地工作。
    
## 如何使用weex
不得不提

## weex基本工作原理
![weex-process](./weex-process.png)
    
    工作流
    we\vue 文件 ————–前端(we\vue源码) 
    ↓ (转换) ——————前端(构建过程) 
    JS Bundle —————–前端(JS Bundle代码) 
    ↓ (部署) ——————服务器 
    在服务器上的JS bundle —-服务器 
    ↓ (编译) —————— 客户端(JS引擎) 
    虚拟 DOM 树 ————— 客户端(Weex JS Framework) 
    ↓ (渲染) —————— 客户端(渲染引擎) 
    Native视图 ————— 客户端(渲染引擎) 
    
## weex进阶

[代码地址](https://github.com/yinshuxun/weex-start-kit)