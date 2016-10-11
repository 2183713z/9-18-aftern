# Mongodb 数据库操作
!['我自己'](https://github.com/2183713z/express-mongoose/raw/master/doc/images/small.jpg)
### 启动Mongodb
```
mkdir -p data/db
mongod --dbpath=./data/db
```

### 开启Mongo Shell
```js
$ mongo

```
### 创建一个数据库
```
$ use digicity-express-api
```
数据库是mongodb中的顶级存储单位，之下一级就 **集合**
### 创建一个集合
```
$ db.createCollection('posts')
```
### 插入数据记录
一个集合(例如：posts),里面可以插入多个数据记录
```
$ db.posts.insert({title:'myTitle',content:'myContent'})
```
### 查看集合中的所有记录
```
 $ db.posts.find()
```
#### Hello Ada
![](https://raw.githubusercontent.com/2183713z/express-mongoose/0339755a94e46e1e88165f7492de6e760dd779eb/doc/images/001-ada.png)
### 修改一条记录(了解内容)
```
db.posts.update({_id: ObjectId('xxx')}, {$set: {title: 'mongodb'}})
```
### 删除一条记录
```
db.posts.remove({_id:Object('xxx')})
```
### 删除posts集合中的所有记录
```
db.posts.remove({})
```
### 删除数据库
假设我们的数据库叫做 digicity-express-api
```
use digicity-express-api
db.dropDatabase()
```
### 为什么记录电子版笔记？
```
- 第一个原因，使用markdown 格式美观
- 第二个原因，便于更新
- 第三个原因，有git/github 控制，永远不会丢失
- 第四个原因，便于搜索
```
### 用js代码操作 mongodb
我们主要基于一个js库的帮助，
[Mongoose](http://mongoosejs.com/),他可以是一个npm的包来安装
解释一下，一个 **js库**，就是一组** JS 接口**的集合。库，英文对应library
![](https://github.com/happypeter/digicity-express-api/blob/master/doc/img/002-mongoose.png?raw=true)
### 下面我们来动手做一个express+mongoose的小 demo
先写一个最简单的express程序
其中index.js代码如下：
```
var express = require('express');
var app = express();

app.post('/posts', function(req, res){  console.log('hello');
});

app.listen(3000, function(){  console.log('running on port 3000.....');
});
```
相应的 curl 测试命令是
```
curl --request POST localhost:3000/posts
```
如果可以在运行 `node index.js` 的位置看到 `hello` 表示我们这一步胜利完成。

  ![](https://github.com/2183713z/express-mongoose/raw/master/doc/images/003-curl.png)
### 安装mongoose
作为一个npm包进行安装[link](https://www.npmjs.com/package/mongoose)
```
npm install --save mongoose
```
### 在js代码中导入mongoose
```
var mongoose=require('mongoose');
```
### 进行数据库的连接
```
mongoose.connect('mongodb://localhost:27017/digicity-express-api');
```
mongoose.connect接口用来连接我们系统上的安装的mongodb数据库
## 如何定位数据库所在位置？
- 一种逻辑上可行的方案，就是用数据存储的文件夹的位置(比如前面我们采用的data/db 文件夹)，但是实际上Mongodb有其他方案
- mongodb的软件，运行起来类似一个网站，用链接来访问。(mongodb://localhost:27017)

但是，链接之后，要跟上具体数据库的名字。我们每次链接，都是链接到一个数据库。比如我们这里，就是digicity-express-api(一般与项目名同名)
## 如何验证链接成功了呢？用下面的代码：
```
var db=mongoose.connection;
db.on('error',console.log);
db.once('open',function(){
  console.log('success');
})
```
看到  `success` 字样表示链接成功
### 在js代码中导入mongoose
### 进行数据库的连接
### 定义数据的概要Schema
数据天然的具有一定的结构，比如，人员名单中，自然的会涉及姓名，年龄，籍贯等信息，在mongodb这里，一个人员的信息会被作为一条记录来保存。所有信息的类型会对应成字段名，由于是跟计算机打交道，每个字段还要涉及他的数据类型(num,string...)。
那么Schema就是用来规定一个记录的各个字段的，字段名+数据类型的。
```
var Schema=mongoose.Schema;
var PostSchema=new Schema({
  title:String,
  content:String
})
```
上面代码，规定出了我们的记录能够保存哪些数据

### 创建数据模型model
数据库的结构就是，一个数据库，里面会包含多个集合，一个集合会包含多条数据记录。
那么现在，我们的数据要往哪个数据库中存储？这个问题以及通过前面的`mongoose.connect(xxx)`的语句指定了。
但是数据库要保存到哪个集合还没有指定。所有我们的model创建语句如下：
```
var Post=mongoose.model('Post',PostSchema)
```
上面 `.model()`的第一个参数, `Post`就为我们指定了集合的名字，会对应数据库中的posts这个集合，第二个参数就是数据schema，就是前面我们定义的。
到这里，所有数据存储的基础设施全部就绪。
### 实例化model得到数据对象
现在我们要把实际存储的数据，放到一个model的实例（对象）之中了。
```
 var post = new Post({title:"myTitle", content: "myConent"})
```
### 对象之上呼叫save()

这样可以把保存到数据库中
```
post.save(function(){
   console.log('saved!')
  })
```
