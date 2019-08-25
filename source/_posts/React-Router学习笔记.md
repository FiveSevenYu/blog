---
title: React Router学习笔记
date: 2018-01-18 00:43:19
tags:
---

引入
React Router分成三个包：
react-router	
react-router-dom	//浏览器
react-router-native  //native

路由器
对于网页项目，有<BrowserRouter>与<HashRouter>两种组件。
<BrowserRouter> //当存在服务区来管理动态请求时
<HashRouter> //用于静态网站
<!-- more -->
<Route>
它最基本的职责是当页面的访问地址与Route上的path匹配时，就渲染出对应的UI页面。
<Route>自带三个render method和三个 props.
render method分别是：
+ Route component
+ Route render
+ Route children
每种 render method 都有不同的应用场景，同一个<Route>应该只能使用一种render method，大部分情况下你将使用component。
props分别是：
+ match
+ location
+ history
所有的render method 五一例外都将传入这些props.

component
component
只有当访问地址和路由匹配时，一个 React component 才会被渲染，此时此组件接受 route props (match, location, history)。
当使用 component 时，router 将使用 React.createElement 根据给定的 component 创建一个新的 React 元素。这意味着如果你使用内联函数（inline function）传值给 component 将会产生不必要的重复装载。对于内联渲染（inline rendering）, 建议使用 render prop。
