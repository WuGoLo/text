---
title: mockjs+node.js实现搭建mock服务器
date: 2019-11-12 15:52:59
toc: true
comments: true #是否可评论
categories:  #分类
- Mock
tags:   #标签
- mock
- node
---



### 一、vue项目中引入mock基本用法

项目所需资源：

- mockjs：`npm i mockjs -S`    
- axios(或者ajax)

1. 在项目目录的src目录下新建一个mock文件夹(自定义)；
2. 在mock文件夹中创建一个index.js文件，用于调用mock相关API模拟数据；
3. 在mock文件夹中创建一个data.js文件，用于充当数据文件；
4. mock文件创建完毕后在main.js入口文件引入mock文件夹的index.js文件；
5. 通过axios进行请求即可

项目结构及具体代码如下：

![1573547064745](mockjs-node-js实现搭建mock服务器\1573547064745.png)
{% asset_img 1573547064745.png "" %}
mock/index.js

~~~js
import Mock from 'mockjs'
import util from './data'
# 注意此处路径书写之后，在request请求是的路径必须和此路径一致
Mock.mock('http://localhost:7721/swipes', 'get', util.list1)
// 可根据自己需求在data文件中模拟相关数据
Mock.mock('http://localhost:7721/names', 'get', util.list2)
export default Mock;
~~~

mock/data.js

~~~js
// 可以自定义多个数据对象
const list1 = {
  'ret': true,
  'data|1-10': [{
    'userid': '@id()',
    'username': '@cname()',
    'date': '@date()',
    'avatar': "@image('200x200','red','#fff','avatar')",
    'description': '@paragraph()',
    'ip': '@ip()',
    'email': '@email()'
  }]
}
const list2 = {
  'data|1-10': [{
    'id|+1': 1,
    'email': '@EMAIL'
  }]
}
export default {
  list1,
  list2
}
~~~

项目汇总应用

在main.js文件汇总引入

~~~js
import './mock/index'
~~~

应用页面如login.vue页面引入

~~~js
<script>
import axios from 'axios'
export default {
  name: 'Login',
  data() {
    return {
    }
  },
  created() {
    this.handleLogin()
  },
  methods: {
    handleLogin() {
      axios.get("http://localhost:7721/swipes").then(res => {
        console.log(res) //这里输出即使模拟的数据
      })
    }
~~~

如项目中第axios进行了封装，并且api分割到了响应的js文件中，也可进行使用

#### 遇到的问题

**此处注意**： mockjs是对浏览器的请求进行了请求拦截，所以network中不会看到请求过程；

**由此**：封装中的axios如果设置`baseURL: '127.0.0.1:3000' `,可能无法生效，会报404等多种错误；

**总结原因**：还是模拟路径和访问路径的不同导致，多尝试几次增删或者直接不设置baseURL也会成功；

**重要！重要！重要！ => 模拟数据接口必须和请求数据接口完全一致**

> 以上方法及mock代码结构不是绝对，也可根据自己的经验封装不同结构的mock代码逻辑；

### 二、通过mock+node搭建简易模拟数据服务器

如果觉得第一种方法太死板，，，，容易出错，，，，还看不到请求路径，，，，，

可以在自己本地搭建一个mock服务器

参考博客：

- [基于express和mock.js搭建自己的前后端分离Mock服务器](https://www.jianshu.com/p/e5074cb92e3f)
- [Mock.js 和Node.js详细讲解](http://www.manongjc.com/article/10503.html)

所需技术：

- node + mock + node express

具体步骤：

1. 安装node

2. 安装其他依赖包

   - `yarn`(替代npm的包管理工具): `npm install yarn`(可选)
   - `express` (express框架): `npm install express -g`
   - `express-generator` (express项目生成插件): `npm install express-generator -g`
   - `mockjs`(模拟数据生成库): `npm install mockjs`

3. 生成新的express项目并编写服务端

   3.1 在指定路径下打开命令行，输入`express mockserver`,即可生成名为`mockserver`的项目

   3.2 打开`app.js`文件,在 *var app = express()* 之后加入如下代码，屏蔽跨域:

```
            app.all('*', function(req, res, next) {
                res.header("Access-Control-Allow-Origin", "*");
                res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
                next();
            });
```

   3.3 仿照`users.js`文件的格式及其在`app.js`文件中的路由挂载方式（任何一个熟练地代码搬运工肯定看得懂），引入`mockjs`,生成需要的随机数据,当接收到前端发送的请求时,返回生成的数据:

```
//服务端响应代码片段/routes/operationboard.js:
//业务逻辑为查询系统告警信息列表
//node服务器启动后，请求地址为:127.0.0.1:3000/operationboard/systemwarn
//3000端口为express默认启动端口

var express = require('express');
var router = express.Router();
var Mock = require('mockjs');
var Random = Mock.Random;

router.get('/systemwarn', function (req, res, next) {
  var data =Mock.mock({
      'list|20':[{
            'id|+1':1,
            'serial_number|1-100':1,
            'warn_number|1-100':1,
            'warn_name|1':['流水线编排服务异常','磁盘占用超过阈值'],
            'warn_level|1':['紧急','重要'],
            'warn_detail':'环境IP:10.114.123.12,服务名称:XX',
            'create_time':Random.datetime(),
            'finish_time':Random.datetime(),
            'contact|4':'abc'
        }] 
      });

  res.send({
    meta : {
      message: 'success'
    },
    status:true,
    data: data.list
  })
})
module.exports = router;
```
4. 启动服务器即可

5. `$npm i nodemon -D`安装[nodemon](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fnodemon)，node自动启动工具，可根据自己需要进行安装

6. 打开项目目录下的`package.json`, 更改scripts:

   ```js
    改前 "start": "node ./bin/www"
    改后 "start": "nodemon ./bin/www"
   ```

#### 项目中应用：

像平时请求一样即可

#### 遇到问题：

报错`Failed to execute 'open' on 'XMLHttpRequest': Invalid URL`和404

原因：是配置的时候没有加http://和漏掉一个“/”

参考博客：[漏掉http](https://www.cnblogs.com/cap-rq/p/10315872.html)和[漏掉“/”](https://www.cnblogs.com/fxwoniu/p/11589287.html)