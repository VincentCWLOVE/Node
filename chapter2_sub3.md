# 错误处理

NodeJS应用是运行在一个进程中的单线程应用，一旦因为某个回调函数发生了错误，整个程序就崩溃了，事实上我们的服务器也就会崩溃了。

如果我们要写一个健壮的NodeJS程序，那么捕获错误是必不可少的技能点。

```javascript

process.on("uncaughtException",function(error){
	console.error(error);
	process.exit(1); //手动退出程序
})

```
我们在开发NodeJS应用的时候，我们可以在每个回调函数中，加入以上代码的调用，来查找错误原因。

当然，我们NodeJS提供了一些模块，像 `http`、`net`、`fs` 等，都提供了 `error` 分发事件

```javascript

var fs = require("fs");

fs.readFile('xxx.txt',function(err,data){
	if(err){
		console.log(err)
		return;
	}

	console.log(data);
})

```