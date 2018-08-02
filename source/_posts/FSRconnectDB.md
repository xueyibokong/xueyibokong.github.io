---
title: 全栈之路-koa + sequelize=>ORM
date: 2018-07-31
tags: 
    - 服务端
    - 数据库
categories: 
    - 全栈之路
---

# koa链接mysql数据库

## 选型

```bash
$ yarn add sequelize mysql2
```

`sequelize`是一个ORM(Object-Relational Mapping)库，其依赖`mysql2`，所以需要安装`mysql2`。



> 目录

```
.
├── controllers
│   ├── index.js
│   ├── login.js
│   ├── register.js
│   └── test.js
├── middlewares
│   └── response.js
├── public
│   └── imgs
│       ├── upload_00f9bdb7aa75dfd455794547b138aebc.png
│       └── upload_ec0497887318dc78a69503de737570da.png
├── routes
│   └── index.js
├── service
├── tablemodules----------------------------------sequelize维护的modules，是与表对应的。
│   ├── index.js
│   ├── user_info.js
│   └── users.js
├── app.js
├── config.js
├── sequelize.js
├── ws.js
├── nodemon.json
├── package.json
└── yarn.lock
```

> config.js

```javascript
module.exports = {
    port: '5757',
    rootPathname: '',

    /**
     * MySQL 配置，用来存储 session 和用户信息
     * 若使用了腾讯云微信小游戏解决方案
     * 开发环境下，MySQL 的初始密码为您的微信小游戏 appid
     */
    mysql: {
        host: '140.143.230.121',
        port: 3306, //mysql默认端口
        database: 'test',
        username: 'root',
        password: 'zhangjun..'
    }
}

```

> suquelize链接数据库
>
> 借鉴[廖雪峰系列](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001471955049232be7492e76f514d45a2180e2c224eb7a6000)，[sequelize中文文档](https://github.com/demopark/sequelize-docs-Zh-CN)

```javascript
//初始化链接数据库，并创建ORM(Object-Relational Mapping)实例
const Sequelize = require('sequelize');
const config = require('./config').mysql;

let sequelize = new Sequelize(config.database, config.username, config.password, {
    host: config.host,
    dialect: 'mysql',
    pool: {
        max: 5,
        min: 0,
        idle: 30000
    }
})

module.exports = sequelize;
```

>表模型

```javascript
const Sequelize = require('sequelize')
const sequelize = require('../sequelize');
module.exports = sequelize.define('users',{
    id : {
        type: Sequelize.STRING(50),
        primaryKey: true        //如果是主键，需要此项描述。
    },
    'user_id' : Sequelize.STRING,
    'user_name' : Sequelize.STRING,
    'sex' : Sequelize.STRING
},{
    timestamps: false //{ timestamps: false }是为了关闭Sequelize的自动添加timestamp的功能。
})
```

### 未完待续...

