title: node-notes
date: 2016-11-11 19:52:23
tags: node
categories: 编程语言
---
** {{ title }}：** <Excerpt in index | 首页摘要>
    node学习重点，深入浅出nodejs学习笔记
<!-- more -->
<The rest of contents | 余下全文>
### node简介：
	1. 异步IO，适合io密集型的
	2. 单线程，通过childnode实现多线程
	3. 跨平台，通过electron编写跨平台客户端
	4. 上手简单,功能强大
### node模块
	1. node模块分为两类，一是node的内建模块（核心模块），二是用户编写的文件模块
	2. 核心模块在node编译时加载到内存，文件模块在运行时动态加载
	3. node的每一个文件模块都是一个对象
	4. 文件模块需要经过路径分析，文件定位，编译执行3个过程
	5. node对引入过的模块都进行缓存，require会优先使用缓存
### 异步IO
	1. node中都是异步的，实现同步的话要通过回调函数或者同步框架
	2. 异步的方案：事件发布/事件监听，Promise/Deferred模式,流程控制库
	3. 事件监听模式：Node自身的events模块提供了简单的实现，具有addListener/on(). once(). remove		Listener(). removeAllListeners()和emit()方法。
	```js
	emitter.on("event1", function (message) {
		console.log(message);
	});
	// 发布
	emitter.emit('event1', "I am message!");
	```
### 内存控制
	1. 在node中内存限制为64位1.4G（32位0.7G）
	2. 限制内存的原因：V8做垃圾回收如果以1.5G为例，做一次小的垃圾回收需要50ms,做一次非增量式内存回收耗时1s以上
	3. node在启动时可以更改内存大小，--max-old-space-size=或者--max-new-space-size=
	4. v8的内存回收机制：内存分代为新生代（生命周期短）和老生代（生命周期长），
	5. 堆外内存不受内存限制，如buffer对象的使用
### 理解buffer


### 网络编程
	1. tcp服务
		服务端：
```js
		var net = require('net');
		var server = net.createServer(function (socket) {
		socket.on('data', function (data) {
			socket.write("你好");
		});
		socket.on('end', function () {
			console.log('断开连接');
		});
		socket.write("欢迎光临：\n");
		});
		server.listen(8124, function () {
			console.log('server bound');
		});
```
		客户端：
```js
		var net = require('net');
		var client = net.connect({port: 8124}, function () { //'connect' listener
			console.log('client connected');
			client.write('world!\r\n');
		});
		client.on('data', function (data) {
			console.log(data.toString());
			client.end();
		});
		client.on('end', function () {
			console.log('client disconnected');
		});
```
	2. udp服务

```js
		// 服务端：
		var dgram = require("dgram");
		var server = dgram.createSocket("udp4");
		server.on("message", function (msg, rinfo) {
			console.log("server got: " + msg + " from " +
			rinfo.address + ":" + rinfo.port);
		});
		server.on("listening", function () {
		var address = server.address();
		console.log("server listening " +
			address.address + ":" + address.port);
		});
		server.bind(41234);
		// 客户端：
		var dgram = require('dgram');
		var message = new Buffer(”nodejs“);
		var client = dgram.createSocket("udp4");
		client.send(message, 0, message.length, 41234, "localhost", function(err, bytes) {
			client.close();
		});
```
	3. http服务

```js
		// 服务端：
		var http = require('http');
		http.createServer(function (req, res) {
		res.writeHead(200, {'Content-Type': 'text/plain'});
			res.end('Hello World\n');
		}).listen(1337, '127.0.0.1');
		console.log('Server running at http://127.0.0.1:1337/');
		// 客户端：
		var options = {
			hostname: '127.0.0.1',
			port: 1334,
			path: '/',
			method: 'GET'
		};
		var req = http.request(options, function(res) {
			console.log('STATUS: ' + res.statusCode);
			console.log('HEADERS: ' + JSON.stringify(res.headers));
			res.setEncoding('utf8');
			res.on('data', function (chunk) {
			console.log(chunk);
			});
		});
		req.end();
```
	4. websocket服务
```js
		// 客户端：
		var socket = new WebSocket('ws://127.0.0.1:12010/updates');
		socket.onopen = function () {
			setInterval(function() {
			if (socket.bufferedAmount == 0)
				socket.send(getUpdateData());
			}, 50);
		};
		socket.onmessage = function (event) {
			// TODO：event.data
		};
		// <!-- 模拟浏览器： -->

		var WebSocket = function (url) {
		// 代码?解析ws://127.0.0.1:12010/updates
			this.options = parseUrl(url);
			this.connect();
		};
		WebSocket.prototype.onopen = function () {
		// TODO
		};
		WebSocket.prototype.setSocket = function (socket) {
		this.socket = socket;
		};
		WebSocket.prototype.connect = function () {
			var this = that;
			var key = new Buffer(this.options.protocolVersion + '-' + Date.now()).toString('base64');
			var shasum = crypto.createHash('sha1');
			var expected = shasum.update(key + '258EAFA5-E914-47DA-95CA-C5AB0DC85B11').digest('base64');
			var options = {
				port: this.options.port, //12010
				host: this.options.hostname, // 127.0.0.1
			headers: {
				'Connection': 'Upgrade',
				'Upgrade': 'websocket',
				'Sec-WebSocket-Version': this.options.protocolVersion,
				'Sec-WebSocket-Key': key
			}
		};
		var req = http.request(options);
			req.end();
			req.on('upgrade', function(res, socket, upgradeHead) {
			// 连接成功
			that.setSocket(socket);
			// 触发open事件
			that.onopen();
		});
		};
```

```js
		// 服务端响应：
		var server = http.createServer(function (req, res) {
			res.writeHead(200, {'Content-Type': 'text/plain'});
			res.end('Hello World\n');
		});
		server.listen(12010);
		// 在收到upgrade请求之后，告知客户端允许切换协议
		server.on('upgrade', function (req, socket, upgradeHead) {
			var head = new Buffer(upgradeHead.length);
			upgradeHead.copy(head);
			var key = req.headers['sec-websocket-key'];
			var shasum = crypto.createHash('sha1');
			key = shasum.update(key + "258EAFA5-E914-47DA-95CA-C5AB0DC85B11").digest('base64');
			var headers = [
				'HTTP/1.1 101 Switching Protocols',
				'Upgrade: websocket',
				'Connection: Upgrade',
				'Sec-WebSocket-Accept: ' + key,
				'Sec-WebSocket-Protocol: ' + protocol
			];
			// 让数据立即发送
			socket.setNoDelay(true);
			socket.write(headers.concat('', '').join('\r\n'));
			// 建立服务器端WebSocket连接
			var websocket = new WebSocket();
			websocket.setSocket(socket);
		});
```
		5. TLS服务（安全方面）
```js
		// 服务端：
		var tls = require('tls');
		var fs = require('fs');
		var options = {
			key: fs.readFileSync('./keys/server.key'),
			cert: fs.readFileSync('./keys/server.crt'),
			requestCert: true,
			ca: [ fs.readFileSync('./keys/ca.crt') ]
		};
		var server = tls.createServer(options, function (stream) {
			console.log('server connected', stream.authorized ? 'authorized' : 'unauthorized');
			stream.write("welcome!\n");
			stream.setEncoding('utf8');
			stream.pipe(stream);
		});
		server.listen(8000, function() {
			console.log('server bound');
		});
```
			// 测试证书是否正常
			`$ openssl s_client -connect 127.0.0.1:8000`

			客户端：
			// 创建私钥
			`$ openssl genrsa -out client.key 1024`
			// 生成CSR
			`$ openssl req -new -key client.key -out client.csr`
			// 生成签名证书
			`$ openssl x509 -req -CA ca.crt -CAkey ca.key -CAcreateserial -in client.csr -out client.crt`
```js
			var tls = require('tls');
			var fs = require('fs');
			var options = {
				key: fs.readFileSync('./keys/client.key'),
				cert: fs.readFileSync('./keys/client.crt'),
				ca: [ fs.readFileSync('./keys/ca.crt') ]
			};
			var stream = tls.connect(8000, options, function () {
				console.log('client connected', stream.authorized ? 'authorized' : 'unauthorized');
				process.stdin.pipe(stream);
			});
			stream.setEncoding('utf8');
			stream.on('data', function(data) {
				console.log(data);
			});
			stream.on('end', function() {
				server.close();
			});
```
		6. HTTPS服务
			服务端：
```js
			var https = require('https');
			var fs = require('fs');
			var options = {
				key: fs.readFileSync('./keys/server.key'),
				cert: fs.readFileSync('./keys/server.crt')
			};
			https.createServer(options, function (req, res) {
			res.writeHead(200);
				res.end("hello world\n");
			}).listen(8000);
			```
			客户端：
```js
			var https = require('https');
			var fs = require('fs');
			var options = {
				hostname: 'localhost',
				port: 8000,
				path: '/',
				method: 'GET',
				key: fs.readFileSync('./keys/client.key'),
				cert: fs.readFileSync('./keys/client.crt'),
				ca: [fs.readFileSync('./keys/ca.crt')]
			};
			options.agent = new https.Agent(options);
			var req = https.request(options, function(res) {
				res.setEncoding('utf-8');
				res.on('data', function(d) {
					console.log(d);
				});
			});
			req.end();
			req.on('error', function(e) {
				console.log(e);
			});
```
### 玩转进程
	node提供了child_process.fork()实现进程的复制
```js
	var http = require('http');
	http.createServer(function (req, res) {
		res.writeHead(200, {'Content-Type': 'text/plain'});
		res.end('Hello World\n');
	}).listen(Math.round((1 + Math.random()) * 1000), '127.0.0.1');
```
	运行node worker.js，监听?1000到2000之间的端口。

	以下是master.js
```js
	var fork = require('child_process').fork;
	var cpus = require('os').cpus();
	for (var i = 0; i < cpus.length; i++) {
		fork('./worker.js');
	}
```
	这是著名的master-worker模式，主从模式

	//创建子进程
	child_process模块提供了四个方法创建子进程
	spawn()		执行命令
	exec()			执行命令		可设置时间
	execFile()		执行文件		可设置时间
	fork()			执行javascript
	后面3中方法都是spawn()的延伸
	//实例work.js
```js
	var cp = require('child_process');
	cp.spawn('node', ['worker.js']);
	cp.exec('node worker.js', function (err, stdout, stderr) {
	// some code
	});
	cp.execFile('worker.js', function (err, stdout, stderr) {
	// some code
	});
	cp.fork('./worker.js')
```
	//进程间通信
	在浏览器中，javascript主线程和UI渲染是一个线程，渲染UI和执行js是互相阻塞的
	html5提出来webworker API，创建工作线程在后台运行
```js
	var worker = new Worker('worker.js');
	worker.onmessage = function (event) {
		document.getElementById('result').textContent = event.data;
	};
```
	work.js代码如下
```js
	var n = 1;
	search: while (true) {
		n += 1;
		for (var i = 2; i <= Math.sqrt(n); i += 1)
			if (n  i == 0) %
				continue search;
		// found a prime
		postMessage(n);
	}
```
	主线程和工作线程通过onmessage()和postMessage()进行通信，子进程对象由send方法
	// parent.js
```js
	var cp = require('child_process');
	var n = cp.fork(__dirname + '/sub.js');
	n.on('message', function (m) {
		console.log('PARENT got message:', m);
	});
	n.send({hello: 'world'});
	// sub.js
	process.on('message', function (m) {
		console.log('CHILD got message:', m);
	});
	process.send({foo: 'bar'});
```
	//进程间通信原理
	实现进程间的技术有：管道，tcp，socket，共享内存，等。
	//句柄传递
	一个端口只能由一个工作进程监听，解决方案是有主进程监听一个80端口，然后
	分发到其他子线程去

```js
	// parent.js
	var child = require('child_process').fork('child.js');
	// Open up the server object and send the handle
	var server = require('net').createServer();
	server.on('connection', function (socket) {
		socket.end('handled by parent\n');
	});
	server.listen(1337, function () {
		child.send('server', server);
	});
	//child.js
	process.on('message', function (m, server) {
		if (m === 'server') {
			server.on('connection', function (socket) {
				socket.end('handled by child\n');
			});
		}
	});

	//将服务发送到多个子进程实例
	//parent.js
	var cp = require('child_process');
	var child1 = cp.fork('child.js');
	var child2 = cp.fork('child.js');
	// Open up the server object and send the handle
	var server = require('net').createServer();
	server.on('connection', function (socket) {
		socket.end('handled by parent\n');
	});
	server.listen(1337, function () {
		child1.send('server', server);
		child2.send('server', server);
	});
	//child.js
	process.on('message', function (m, server) {
	if (m === 'server') {
		server.on('connection', function (socket) {
			socket.end('handled by child, pid is ' + process.pid + '\n');
		});
	}
	})
	//最终版，请求全部由子进程处理，
	//parent,js
	var cp = require('child_process');
	var child1 = cp.fork('child.js');
	var child2 = cp.fork('child.js');
	// Open up the server object and send the handle
	var server = require('net').createServer();
	server.listen(1337, function () {
		child1.send('server', server);
		child2.send('server', server);
		server.close(); //关闭主线程的服务
	});
	//child.js
	var http = require('http');
	var server = http.createServer(function (req, res) {
		res.writeHead(200, {'Content-Type': 'text/plain'});
		res.end('handled by child, pid is ' + process.pid + '\n');
	});
	process.on('message', function (m, tcp) {
		if (m === 'server') {
			tcp.on('connection', function (socket) {
				server.emit('connection', socket);
			});
		}
	});
```
	<p>
	send发送的句柄类型：
		net.Socket。TCP
		net.Server。TCP服务

		net.Native。C++
		dgram.Socket。UDP
		dgram.Native。C++
	//进程事件
	error：
	exit：
	close：
	disconnect：
	</p>
```js
	//自动重启线程
	//master.js
	var fork = require('child_process').fork;
	var cpus = require('os').cpus();
	var server = require('net').createServer();
	server.listen(1337);
	var workers = {};
	var createWorker = function () {
	var worker = fork(__dirname + '/worker.js');
	//退出时重新启动新的线程
	worker.on('exit', function () {
		console.log('Worker ' + worker.pid + ' exited.');
		delete workers[worker.pid];
		createWorker();
	});
	// 句柄转发
	worker.send('server', server);
	workers[worker.pid] = worker;
	console.log('Create worker. pid: ' + worker.pid);
	};
	for (var i = 0; i < cpus.length; i++) {
	createWorker();
	}
	// 进程自己退出让所有工作进程退出
	process.on('exit', function () {
		for (var pid in workers) {
			workers[pid].kill();
		}
	});
	//work.js  考虑处理异常
	var http = require('http');
	var server = http.createServer(function (req, res) {
		res.writeHead(200, {'Content-Type': 'text/plain'});
		res.end('handled by child, pid is ' + process.pid + '\n');
	});
	var worker;
	process.on('message', function (m, tcp) {
		if (m === 'server') {
			worker = tcp;
			worker.on('connection', function (socket) {
				server.emit('connection', socket);
			});
		}
	});
	process.on('uncaughtException', function () {
		process.send({act: 'suicide'});
	// 停止接收新的连接
		worker.close(function () {
	// 连接断开后退出进程
			process.exit(1);
		});
	});
```












> 如果文章对你有帮助,请去我的博客留个言吧! [我的博客][1]

[1]: http://geeksblog.cc
