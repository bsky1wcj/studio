# Node.js模块和包 #

模块（Module）和包（Package）是Node.js最重要的支柱。

**模块（Module） :** 是Node.js基本组成部分，文件和模块是一一对应的。即一个Node.js文件就是一个模块，这个文件可以是JavaScript、JSON、C/C++扩展。

## 创建模块 ##

Node.js提供了 `exports` 和  `require` 两个对象，  exports是模块公开的接口， require 用于获取一个模块的接口，即获取模块 exprots对象。

示例：

创建 module.js 代码如下：

	var name;
	
	exports.setName = function(thyName) {
		name = thyName;
	};
	
	exports.sayHello = function() {
		console.log('Hello ' + name);
	};

同目录下创建 getmodule.js 代码如下：

	var myModule = require('./module');
	
	myModule.setName('wcj');
	myModule.sayHello();

运行 `node getmodule.js` ,控制台输出： `Hello wcj`

以上示例中 module.js中通过 exports对象把 setName 和 syaHello 作为模块的访问接口，在getmodule.js 中 通过 `require('./module')` 加载这个模块。

## 单次加载 ##

新建 loadmodule.js 代码如下：

	var hello1 = require('./module');
	hello1.setName('wcj');
	
	var hello2 = require('./module');
	hello2.setName('zdd');
	
	hello1.sayHello();

运行 `node loadmodule.js` 输出 `Hello zdd` 
hello1和hello2指向了同一个实例，hello1.setName 的结果被 hello2.setName 覆盖， 输出结果由后者决定。


## 覆盖 exports ##

有时我们想把一个对象封装到模块中， 如下：

	//singleobject.js
	function Hello() {
		var name;
	
		this.setName = function(thyName) {
			name = thyName;
		};
	
		this.sayHello = function() {
			console.log('Hello ' + name);
		};
	};
	
	exports.Hello = Hello;

此时我们要通过 `require('.singleobject').Hello` 来获取 Hello 对象。 这样略显冗余，可以简化为：

	//singleobject-ex02.js
	function Hello() {
		var name;
	
		this.setName = function(thyName) {
			name = thyName;
		};
	
		this.sayHello = function() {
			console.log('Hello ' + name);
		};
	};
	
	//exports.Hello = Hello;
	module.exports = Hello;

获取对象：

	//getsingleobject-ex02.js
	var Hello = require('./singleobject-ex02');
	hello1 = new Hello();
	hello1.setName('wcj');
	hello1.sayHello();

这里唯一的区别就是用  `module.exports = Hello;`  代替了 `exports.Hello = Hello;`

运行 `node getsingleobject-ex02.js` 输出 `Hello wcj` 

## 创建包 ##

Node.js的包就是一个目录，其中包含一个JSON格式的包说明文件 package.json 。 严格符合 CommonJS 规范的包应该具备以下特征：

- package.json必须在包的顶层目录下
- 二进制文件应该在bin 目录下
- JavaScript代码应该在lib目录下
- 文档应该在doc目录下
- 单元测试应该在test目录下

Node.js 对包的要求并没有这么严格，只要求顶层目录下有 package.json， 并符合一些规范即可。

### 作为文件夹的模块 ###

模块与文件一一对应的。文件不仅是可以是JavaScript代码 或 二进制代码，还可以是一个文件夹。最简单的包就是一个作为文件夹的模块。

示例：

	// package-ex01/index.js
	exports.hello = function() {
		console.log('hello');
	};

在 package-ex01 文件夹之外 建立 getpackage.js 代码如下：

	var somePackage = require('./package-ex01');
	somePackage.hello();

运行 `node getpackage.js` 输出 `hello` 

我们通过该方式可以把文件夹封装为一个模块，即所谓的包。包通常是一些模块的集合，在模块的基础上提供了更高层的抽象，相当于提供了一些固定函数库。通过定制 package.json,我们可以创建更复杂更完善的符合规范的包用于发布。

### package.json ###
在前面的例子中的 package-ex01 文件夹下，新建 package.json的文件。或通过运行 `npm init`
引导程序生成 package.json。
下面是通过 `npm init` 生成的 package.json 文件

	{
	  "name": "packaget1",
	  "version": "0.0.1",
	  "description": "一个学习测试包",
	  "main": "index.js",
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1"
	  },
	  "repository": "",
	  "author": "bsky1wcj",
	  "license": "GPLv2"
	}

包参数说明：

- main: 包的接口模块， 如果package.json或main 字段不存在，系统会尝试寻找 index.js 或 index.node 作为包的接口。
- name: 包的名称，必须是唯一的，由小写英文字母、数字和下划线组成，不包括空格。
- description: 包简要说明。
- version: 版本字符串。
- keywords: 关键字数组，通常用于搜索。
- maintainers: 维护者数组，每个元素包括 name 、 email（可选） 、web（可选） 字段
- contributors: 贡献者数组，格式和 maintainers 相同。包的作者应该是贡献者数组的第一个元素。
- bugs: 提交bug的地址，可以是网址或电子邮件地址
- licenses: 许可证数组，每个元素包括 type(许可证名称） 和 url(链接到许可证文本的地址)字段。
- repository: 仓库托管地址数组，每个元素包括 type (仓库的类型，如git)、url (仓库的地址） 和 path(相对于仓库的路径，可选）字段。
- dependencies: 包的依赖， 一个关联数组，由包名称和版本号组成。

## Node.js包管理器 ##

### 获取一个包 ###

使用npm安装包的命令格式为：` npm [install/i] [package_name]`
例如安装 express 可以在命令行运行：` $ npm install express` 或者 ` $ npm i express`

安装成功后会在当前目录下的  node_modules子目录下找到。

### 本地模式和全局模式 ###

**全局模式使用方法** ` npm [install/i] -g [package_name]`

### 创建全局链接 ###

例如： `$ npm link express`  
npm link命令不支持Windows

### 包的发布 ###

在控制台的 package-ex01 文件夹下 用之前创建好的 package-ex01 包做为发布的实例。

在发布前,使用 `npm adduser` 根据提示输入用户名、密码、邮箱，并等待账号的创建完成。 使用 `npm whoami` 检验是否已经取得了账号。

接下来，在package.json 所在的目录下运行 `npm publish`, 稍等片刻就完成发布了。 打开浏览器，访问 `http://search.npmjs.org` 就可以通过搜索包名称找到自己发布的包了，或者直接访问 `https://npmjs.org/package/packaget1` ,地址中最后一个路径名即为包名。
在包的介绍页描述了包的安装方法及相关信息，我们可以通过刚才创建的用户名及密码登录维护自己的包。

包的安装,在控制台输入 `$ npm install packaget1` 安装之前发布的包。 如果你觉得这个包没用了， 可以通过输入 `npm unpublish` 命令来取消发布。


## 调试 ##

在控制台输入 `node debug fs-ex01.js` 开启调试。

## 参考资料 ##

Node.js开发指南 郭家宝著 人民邮电出版社



