# Express框架 #

[Express](http://expressjs.com) 除了为 http 模块提供了更高层的接口外，还实现了许多的功能，包括：

- 路由控制
- 模版解析支持
- 动态视图
- 用户会话
- CSRF 保护
- 静态文件服务
- 错误控制器
- 访问日志
- 缓存
- 插件支持

它是一个轻量级的 Web 框架，多数功能只是针对 HTTP 协议中常用的操作的封装，更多的功能需要插件和其他模块来完成。
## 安装Express ##

用全局模式安装： `$ npm install -g express`

安装后用 `express -V` 查看版本， 控制台输出： 3.4.0

通过 `express --help` 查看帮助信息：

  Usage: `express [options] [dir]`

  Options:

    -h, --help          output usage information
    -V, --version       output the version number
    -s, --sessions      add session support
    -e, --ejs           add ejs engine support (defaults to jade)
    -J, --jshtml        add jshtml engine support (defaults to jade)
    -H, --hogan         add hogan.js engine support
    -c, --css <engine>  add stylesheet <engine> support (less|stylus) (defaults to plain css)
    -f, --force         force on non-empty directory


## 建立工程 ##

Express 在初始化项目时需要指定模版引擎，它支持 jade 和 ejs。 jade为默认引擎，由于它颠覆了传统模版引擎，指定了一套完整的语法用来生成html的每个标签结构，功能强大但不易学习。 为了降低学习门槛， 这里推荐使用 ejs ，同时暂时不支持添加css 引擎和会话支持。

通过命令 `express -t --ejs myblog` 建立网站基本结构。（虽然建立了，但感觉有点不对。）

这里尝试了以下方式建立工程都可以成功：

jade:  `express myblog`

ejs: `express --ejs myblog`  官网安装方式： `express --sessions --css stylus --ejs myapp`

工程建立后需要安装 dependencies (依赖)： `cd myblog && npm install`

命令检查 myblog 目录下的 package.json 文件， 开始安装依赖 ejs 和 express。 安装成功后会在 myblog 目录下出现 node_modules 目录。

## 启动服务器 ##

进入myblog 目录， 运行 `node app.js`, 在浏览器里输入 ： `http://localhost:3000` ，就可以看到简单的 Welcome to Express页面。


## 工程结构 ##

### app.js ###

app.js 是工程的入口文件。

	/**
	 * Module dependencies.
	 */
	
	var express = require('express');
	var routes = require('./routes');
	var user = require('./routes/user');
	var http = require('http');
	var path = require('path');
	
	var app = express();
	
	// all environments
	app.set('port', process.env.PORT || 3000); //设置监听3000端口
	app.set('views', __dirname + '/views');
	app.set('view engine', 'ejs');
	app.use(express.favicon());
	app.use(express.logger('dev'));
	app.use(express.bodyParser());
	app.use(express.methodOverride());
	app.use(express.cookieParser('your secret here'));
	app.use(express.session());
	app.use(app.router);
	app.use(require('stylus').middleware(__dirname + '/public'));
	app.use(express.static(path.join(__dirname, 'public'))); //配置静态文件服务器，即样式图片等文件所在目录
	
	// development only
	if ('development' == app.get('env')) {
	  app.use(express.errorHandler());
	}
	
	app.get('/', routes.index); //是一个路由控制器，用户访问“/”路径，则由 routes.index 来控制
	app.get('/users', user.list);
	
	//创建应该实例
	http.createServer(app).listen(app.get('port'), function(){
	  console.log('Express server listening on port ' + app.get('port'));
	});


**app.set 的参数设置工具**

> - **env** Environment mode, defaults to process.env.NODE_ENV or "development"
> - **trust proxy** Enables reverse proxy support, disabled by default
> - **jsonp callback name** Changes the default callback name of ?callback=  （开启透明的JSONP支持）
> - **json replacer** JSON replacer callback, null by default
> - **json spaces** JSON response spaces for formatting, defaults to 2 in development, 0 in production
> - **case sensitive routing** Enable case sensitivity, disabled by default, treating "/Foo" and "/foo" as the same (路径区分大小写)
> - **strict routing** Enable strict routing, by default "/foo" and "/foo/" are treated the same by the router （严格路径，启用后不会忽略路径末尾的 “/”）
> - **view cache** Enables view template compilation caching, enabled in production by default （启用视图缓存）
> - **view engine** The default engine extension to use when omitted （视图模板引擎）
> - **views** The view directory path, defaulting to "./views"（视图文件的目录，存放模板文件）
> - **basepath** 基础地址， 通常用于 res.redirect() 跳转
> - **view options** 全局视图参数对象


### routes/index.js ###

index.js 是路由文件，相当于控制器，用于组织展示的内容：

	/*
	 * GET home page.
	 */
	
	exports.index = function(req, res){
	  res.render('index', { title: 'Express', t1: 't1122', t2:{t21:'t21', t22: 't22'} });
	};

app.js中通过  `app.get('/', routes.index);` 将“/” 路径映射到 `exports.index` 函数下（这里和文件名无关，会直接找到exports下的index）。 在 exports.index 中有一句 `res.render('index', { title: 'Express', t1: 't1122' });`, 功能是调用模板解析引擎，翻译 views目录下名为 index 的模版，并传入一个对象作为参数。这些参数供 index.ejs 模板中使用。

### views/index.ejs ###

index.ejs 是模板文件，内容如下：

	<!DOCTYPE html>
	<html>
	  <head>
	    <title><%= title %></title>
	    <link rel='stylesheet' href='/stylesheets/style.css' />
	  </head>
	  <body>
	    <h1><%= title %></h1>
	    <p>Welcome to <%= title %></p>
	    <p>Welcome to <%= t1 %></p>
	    <p>Welcome to <%= t2.t22 %></p>
	  </body>
	</html>


它的基础是 HTML 语言， 其中 <%= title %> 的标签的功能是引用 exports.index 中的变量。


### views/layout.ejs  （在目前的版本中好像已经停用）### 
默认情况下所有的模版都继承自 layout.ejs ， 即通过 `<%- body %>` 部分才是独特的内容，其他部分是共有的，可以看做是网页的框架。

	<!DOCTYPE html>
	<html>
	  <head>
	    <title>test-<%= title %></title>
	    <link rel='stylesheet' href='/stylesheets/style.css' />
	  </head>
	  <body>
	    <%- body %>
	  </body>
	</html>


**对于模版问题** 
express-partials
Express 3.x Layout & Partial support.

从 Express 2.x 之后的版本可以用 express-partials 中间键提供支持

安装命令： `$ npm install express-partials`

使用方法： 在 app.js 文件中添加包依赖 `var partials = require('express-partials');`    应用 `app.use(partials());`

**注意：** `app.use(partials());` 一定要在 `app.use(app.router);` 前面

这下上面的方法就可以用了。 更多资料查看： [https://github.com/publicclass/express-partials](https://github.com/publicclass/express-partials)