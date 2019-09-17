---
title: react-router踩坑
date: 2018-07-31
tags: 
    - 前端
    - react
categories: 
    - 全栈之路
---

# 前言

`react-router`经过很多版本更新，目前是v4版本，坑源自于不同版本的api文档不规范，本文记录`react-router-dom`V4在项目中的使用。

[React Router文档](https://react-guide.github.io/react-router-cn/docs/guides/basics/RouteConfiguration.html)，[V2/V3到V4的迁移指南](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/guides/migrating.md)

> Router.jsx

```jsx
import React,{Component} from "react";
import App from "./App"
import Home from "./pages/home"
import Login from "./pages/login";
import Registe from "./pages/registe";
import {BrowserRouter,Switch,Route,Redirect} from 'react-router-dom';

export default class extends Component {
    render() {
        return <BrowserRouter>
                <App>
                    <Switch>
                        <Route exact path='/' component={Home}/>
                        <Route path='/registe' component={Registe}/>
                        <Route path='/login' component={Login}/>
                    </Switch>
                </App>
            </BrowserRouter>
    }
}
```

> App.js

```jsx
import React, { Component } from 'react';
import './App.css';
class App extends Component {
  render() {
    return (
      <div>
          {/* 路由外的部分 */}  
          <header>
              公共头部
          </header>
          {/* 路由嵌套 */}
          {this.props.children}
      </div>
    );
  }
}

export default App;
```

> 脚本跳转

```javascript
this.props.history.push(PATH)
```

> 