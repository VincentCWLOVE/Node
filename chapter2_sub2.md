# 非阻塞I/O 和 事件环

在一块，我们要理解的是`NodeJS`如何在单线程中实现高并发的。

### 非阻塞I/O

```javascript

console.log("hello");

setTimeout(function(){
	console.log("world")
},2000)

console.log("vincent");


```

对于以上代码，有过JS编程经验的童鞋都知道输出结果是：

```
hello
vincent
world

```
为什么是这样的结果，我们都明白，是因为我们使用了 `setTimeout` 注册了一个2s后执行的事件，`setTimeout` 并不会阻塞程序的继续执行。而且2s我们的事件也会如期执行。

和`setTimeout`一样，Node采用的就是先注册事件，随后不停的询问`cpu`这些事件是否分发了。一旦事件分发了，那么对应的函数就会执行。

```javascript

http.createServer(function(req,res){
	console.log("hello");
	// 拉取数据库信息
	database.getInfomation(function(data){
		console.log(data);
		res.writeHead(200),
		res.end("data");
	})
	console.log("world");
})

```
在上面的代码中，我们发现当客户端发送一个http请求后，Node调用了一个`database.getInfomation`事件，这个事件里面有一个回调函数，这个函数就是一个NodeJS注册事件，当数据库查询完了，这个事件就会派发，然后执行这个函数。

我们会发现 `拉取数据库信息` 这个功能并不会阻塞 `console.log("world");` 执行。而 `拉取数据库信息`就是一个典型的I/O的例子


### 事件环

我们通俗来讲，就是我们客户端（浏览器）请求建立连接，提交数据等行为，会触发Node相应的事件，在这一个时刻，只能执行一个事件回调函数，但是在执行一个事件回调函数的中途，是可以转而处理其他事件（比如，又有新客户端连接了），然后返回继续执行原事件的回调函数，这种处理机制，称为`事件环`机制。

不管是新用户的请求，还是老用户的I/O完成，都将以事件方式加入事件环，等待调度。

### 总结

> 单线程 、 非阻塞I/O 、 事件环，让`NodeJS`实现了并发操作




