# Node.js快速入门 #



## 一、编写第一个 HelloWorld 程序 ##

新建文件helloworld.js,代码如下

    console.log('Hello World!');

在控制台命令中输入 `node helloworld.js`

此时控制台输出：  `Hello World!`

console.log指令和c语言中的printf功能类似，也支持多个参数，支持` %d 、 %s`的变量引用，例如：

	//consolelog.js
	console.log('%s: %d', 'Hello', 25);

此时控制台输出：  `Hello: 25`

## 二、Node.js命令行工具 ##

在控制台输入 `node --help` 获取详细帮助信息

使用方式如： `$ node -e "console.log('Hello World')";`

控制台输出： `Hello World`

这里我们把要执行的语句作为 `node -e` 的参数直接执行


## 三、建立 HTTP 服务器 ##

新建 http-ex01.js 代码如下：

    var http = require('http');
    http.createServer(function(req, res) {
    	res.writeHead(200, {'Content-Type': 'text/html'});
    	res.write('<h1>Node.js</h1>');
    	res.end('<p>Hello World</p>');
    }).listen(3000);
    console.log('HTTP server is listening at port 3000.');

运行 `node http-ex01.js`, 打开浏览器访问 `http://127.0.0.1:3000` 查看页面内容

页面输出如下内容：

> Node.js
> 
> Hello World

## 四、使用supervisor协助开发调试 ##

http-ex01.js在中修改输出内容会发现刷新浏览器时不会立即生效。那是因为node.js为了提高效率只在第一次引用某部分时才解析脚本文件，之后直接访问内存。这样对开发调试极为不利。supervisor就是解决这个问题的。首先需要使用npm安装supervisor:

	$ npm install -g supervisor

接下来我们用 `$ supervisor http-ex01.js` 来运行脚本，再刷新浏览器，这时候你会发现修改立即生效了。


## 五、Node.js异步读取文件 ##

新建 fs-ex01.js 代码如下：

	//异步读取文件
	var fs = require('fs');
	fs.readFile('helloworld.js', 'utf-8', function(err, data) {
		if (err) {
			console.error(err);
		} else {
			console.log(data);
		}
	});
	console.log('end.');

异步方式接收3个参数，第一个是文件名，第二个是编码方式，第三个是一个回调函数，fs-ex01.js 代码可以修改为如下代码：

	//异步读取文件
	function readFileCallBack(err, data) {
		if (err) {
			console.error(err);
		} else {
			console.log(data);
		}
	}

	var fs = require('fs');
	fs.readFile('helloworld.js', 'utf-8', readFileCallBack);
	console.log('end.');

运行 `node fs-ex01.js` 结果如下：

    end.
    console.log('Hello World!');

此时会看到输出结果中 `end.`在前面输出。

### Node.js也提供了同步读取文件的API ###

    //同步读取文件
    var fs = require('fs');
    var data = fs.readFileSync('helloworld.js', 'utf-8');
    console.log(data);
    console.log('end.');

此时输出：

    console.log('Hello World!');
    end.


## 六、事件EventEmitter实现 ##

Node.js所有异步I/O操作都是由 EventEmitter 对象提供， 并在完成时发送一个事件到事件队列。前面提的到 fs.readFile 和 http.createServer 的回调函数都是通过 EventEmitter 来实现的。

EvemtEmitter示例：

	var EventEmitter = require('events').EventEmitter;
	var event = new EventEmitter;
	
	event.on('some_event', function() {
		console.log('some_event occured.');
	});
	
	setTimeout(function() {
		event.emit('some_event');
	}, 1000);

运行 `node event-ex01.js` ，控制台一秒后输出 `some_event occured.`


## 参考资料 ##

Node.js开发指南 郭家宝著 人民邮电出版社