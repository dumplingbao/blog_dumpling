---
title: Node之ORM框架
date: 2019-08-29 16:09:32
categories: 
 - node
tags:
 - node
 - orm
---

## 概述

写这篇blog的原因，想找个node的ORM框架用用，确很难找到一篇对比分析这些ORM框架的文章，唯一找到了一篇，居然是通过star数来论英雄，我觉着很难服众，于是就找几个看看。后来又不想分析，因为我发现node这种野蛮生长，滋生这些ORM轮子比比皆是，远比我想象的多；后来又觉着可以写，作为一个java出身业余研究node的就想通过java的ORM框架来洞悉node这群ORM框架的是非曲直，于是挑了几个框架小扯一篇。

<!--more-->

## ORM框架

ORM框架：Object Relational Mapping，对象-关系-映射，所以说ORM框架就是用面向对象的方式和目前的关系型数据库做匹配，java开发者目前主流的hibernate、mybatis很熟悉了，JDBC原始驱动的方式想必也不在成为主流了。下面介绍几款node的ORM框架，介绍之前先介绍ORM的两种模式：

Active Record 模式：活动记录模式，领域模型模式一个模型类对应关系型数据库中的一个表，模型类的一个实例对应表中的一行记录。这个不难理解，比较简单，但是不够灵活，再看另一种模式，比较一下

Data Mapper 模式：数据映射模式，领域模型对象和数据表是松耦合关系，只进行业务逻辑的处理，和数据层解耦。需要一个实体管理器来将模型和持久化层做对应，这样一来，灵活性就高，当然复杂性也增加了。

所以说，Data Mapper模式对业务代码干预少，Active Record模式直接在对象上CRUD，代码编写也更方便，这就像hibernate和mybatis两种框架，如果想深入研究，可以了解一下[贫血与充血领域对象的平衡](https://dzone.com/articles/anaemic-vs-rich-domain-objects-finding-the-balance)。

有这么一句话很认同，ActiveRecord更加适合快速开发成型的短期简单项目，而DataMapper更加适合长线开发，保持业务逻辑与数据存储独立的复杂项目。除此之外，技术选型还要考虑其他因素，比如项目历史背景等等。

## **TypeORM**

TypeORM 是一个 ORM 框架，详细介绍见 [TypeORM](https://typeorm.io) 官方介绍，TypeORM 也借鉴了hibernate，所以你会发现它特别熟悉，尤其是装饰类的方式。

闲话少说，直接用CLI 命令快速构建项目

```
npm install typeorm -g
```

创建项目

```
typeorm init --name MyProject --database mysql
```

 `name` 是项目的名称，`database` 是将使用的数据库，TypeORM 支持多种数据库。

生成文档结构

```
MyProject
├── src              // TypeScript 代码
│   ├── entity       // 存储实体（数据库模型）的位置
│   │   └── User.ts  // 示例 entity
│   ├── migration    // 存储迁移的目录
│   └── index.ts     // 程序执行主文件
├── .gitignore       // gitignore文件
├── ormconfig.json   // ORM和数据库连接配置
├── package.json     // node module 依赖
├── README.md        // 简单的 readme 文件
└── tsconfig.json    // TypeScript 编译选项
```

修改 `ormconfig.json` 数据库配置文件，直接运行就可以了

```
npm start
```

看一下实体model，user类

```
import {Entity, PrimaryGeneratedColumn, Column} from "typeorm";
@Entity()
export class User {
​    @PrimaryGeneratedColumn()
​    id: number;

​    @Column()
​    firstName: string;

​    @Column()
​    lastName: string;

​    @Column()
​    age: number;
}
```

CRUD操作：逻辑层

```
import "reflect-metadata";
import {createConnection} from "typeorm";
import {User} from "./entity/User";

createConnection().then(async connection => {
​    console.log("Inserting a new user into the database...");
​    const user = new User();
​    user.firstName = "Timber";
​    user.lastName = "Saw";
​    user.age = 25;
​    await connection.manager.save(user);
​    console.log("Saved a new user with id: " + user.id);
​    console.log("Loading users from the database...");
​    const users = await connection.manager.find(User);
​    console.log("Loaded users: ", users);
​    console.log("Here you can setup and run express/koa/any other framework.");
}).catch(error => console.log(error));
```

所以，TypeORM的方式很像hibernate的方式，虽然es6中就已经有装饰器类似java的注解的功能了，但是还是和装饰器有所区别，因为TypeORM采用的是TypeScript 的方式，TypeScript 是 JavaScript 的一个超集，TypeScript 采用类型注解方式，虽然支持es6的标准，但是有些语法还是需要了解，这也或多或少增加了一些选择难度。

## Sequelize

这个被star数最多了一个ORM框架，官方居然不给中文文档，找个CLI命令快速构建也没有，也没找到个合适轮子，只能自己搭了，也不是少了轮子就不能活了。不过Sequelize的官网文档看着很顺眼，不得不称赞一下，需要注意的一点Sequelize v5版本发生了比较大的变化，这里我以最新版本v5版本为主，老版本可以自己看看下官方文档。[Sequelize v5](https://sequelize.org/)

安装npm包

```
$ npm install --save sequelize
$ npm install --save mysql2
```

数据库的配置文件config.js

```
module.exports = {
​    database: {
​        dbName: 'TEST',
​        host: 'localhost',
​        port: 3306,
​        user: 'root',
​        password: '123456'
​    }
}
```

构建数据库访问公共文件db.js

```
const Sequelize = require('sequelize')
const {
​    dbName,
​    host,
​    port,
​    user,
​    password
} = require('../config').database

const sequelize = new Sequelize(dbName, user, password, {
​    dialect: 'mysql',
​    host,
​    port,
​    logging: true,
​    timezone: '+08:00',
​    define: {
​        // create_time && update_time
​        timestamps: true,
​        // delete_time
​        paranoid: true,
​        createdAt: 'created_at',
​        updatedAt: 'updated_at',
​        deletedAt: 'deleted_at',
​        // 把驼峰命名转换为下划线
​        underscored: true,
​        scopes: {
​            bh: {
​                attributes: {
​                    exclude: ['password', 'updated_at', 'deleted_at', 'created_at']
​                }
​            },
​            iv: {
​                attributes: {
​                    exclude: ['content', 'password', 'updated_at', 'deleted_at']
​                }
​            }
​        }
​    }
})
// 创建模型
sequelize.sync({
​    force: false
})
module.exports = {
​    sequelize
}
```

model

```
const {Sequelize, Model} = require('sequelize')
const {db} = require('../../db')

class User extends Model {}
User.init({
    // attributes
    firstName: {
        type: Sequelize.STRING,
        allowNull: false
    },
    lastName: {
        type: Sequelize.STRING
        // allowNull defaults to true
    }
}, {
    db,
    modelName: 'user'
    // options
});
```

还有一种写法，兼容老版本，不推荐

```
const User = db.define('user', {
    // attributes
    firstName: {
        type: Sequelize.STRING,
        allowNull: false
    },
    lastName: {
        type: Sequelize.STRING
        // allowNull defaults to true
    }
}, {
    // options
});
```

这种实际上是sequelize.define内部调用了model.init，但是老版本是没有第一种写法的。

此外需要知道的是，sequelize还默认为每个模型定义字段id（主键）、createdat和updatedat，也可以进行设置。

我们的db.js文件里面配置了，不自动创建模型，也就是自动创建数据表，关闭是有原因的，因为如果表存在会先drop然后再创建，这种操作本身就很可怕的

```
// 创建模型
sequelize.sync({
    force: false
})
```

单个模型也可以配置，切记这种操作很危险，尤其是生成环境

```
// Note: using `force: true` will drop the table if it already exists
User.sync({ force: true }).then(() => {
    // Now the `users` table in the database corresponds to the model definition
    return User.create({
        firstName: 'John',
        lastName: 'Hancock'
    });
});
```

CRUD操作：然后看一下逻辑层，就非常简单了，直接使用ES7 `async/await`即可

```
// Find all users
User.findAll().then(users => {
    console.log("All users:", JSON.stringify(users, null, 4));
});
// Create a new user
User.create({ firstName: "Jane", lastName: "Doe" }).then(jane => {
	console.log("Jane's auto-generated ID:", jane.id);
});
// Delete everyone named "Jane"
User.destroy({
    where: {
    	firstName: "Jane"
	}
}).then(() => {
	console.log("Done");
});
// Change everyone without a last name to "Doe"
User.update({ lastName: "Doe" }, {
    where: {
    	lastName: null
    }
}).then(() => {
	console.log("Done");
});
```

由此来看，没有typeorm装饰类的方式看着顺眼，但是整体构造也容易上手，操作简单，容易理解，看官网文档，功能覆盖强大，typeorm用户反馈使用问题比Sequelize要多，后期用到再做比较。

## ORM2

ORM2貌似没有正了八经的官网，所以看起来就特别麻烦，但是可以看一下github介绍[node-orm2](https://github.com/dresende/node-orm2)，只支持四种数据库MySQL、PostgreSQL、Amazon Redshift、SQLite，这个我没写demo，直接分析一下

安装

```
npm install orm
```

数据库连接

```
var orm = require("orm");
orm.connect("mysql://username:password@host/database", 
    function (err, db) {
    // ...里面一些参数不详细写了
});
```

model

```
var Person = db.define('person', {
	name: String,
	surname: String,
	age: String,
	male: boolean
}, {
	identityCache : true
});
```

 CRUD操作

```
Person.create([
	{
		name: "John",
		surname: "Doe",
		age: 25,
		male: true
	},
	{
		name: "Liza",
		surname: "Kollan",
		age: 19,
		male: false
	}
], function (err, items) {
	// err - description of the error or null
	// items - array of inserted items
});
```

```
Person.get(1, function (err, John) {
	John.name = "Joe";
	John.surname = "Doe";
	John.save(function (err) {
		console.log("saved!");
	});//保存
	Person.find({ surname: "Doe" }).remove(function (err) {
      // Does gone..
    });//删除
});
```

```
Person.find({
    name: "admin"})
    .limit(3)
    .offset(2)//跳过
    .only("name", "age")//返回字段
    .run(function(err, data) {
    
	});
```

所以，准确应该是node-orm2，写法和sequelize类似，但是文档确实不行，数据库支持也少，很难想象后续的可维护性。

## 其它

- bookshelf（这个用的也挺多）

- persistencejs

- waterline

- mongoose

- node-mysql

- knex

  。。。