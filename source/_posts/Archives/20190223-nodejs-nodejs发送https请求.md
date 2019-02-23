title: "nodejs发送https请求"
date: 2015-05-23 18:03:00
tags: [nodejs,https] 
---
	var https = require('https');
	
	https.request({
	  hostname: 'nodejs.org',
	  port: 443,
	  path: '/api/https.html',
	  method: 'GET'
	}, function(res) {
	  console.log("statusCode: ", res.statusCode);
	  console.log("headers: ", res.headers);
	
	  res.on('data', function(d) {
	    process.stdout.write(d);
	  });
	}).on('error', function(e) {
	  console.error(e);
	}).end();

<!--more-->
hostname: hostname

port: 端口号，默认是443

method: 请求方式，值为POST/GET/PUT...，默认是GET

path: 路径。默认是 '/'. 栗子： '/index.html?page=2'

####发送POST请求：

	var https = require('https');
	var querystring = require('querystring');
	
	
	var post_data = querystring.stringify({
		//post数据，这里传入json对象
		aaa:'AAA',
		bbb:'BBB'
	});
	
	var options = {
	  hostname: 'hostname',
	  port: 443,
	  path: '/',
	  method: 'POST',
	  headers: {"Content-type": "application/x-www-form-urlencoded", "Accept": "text/json","User-Agent": "geyuxu-nodejs/1.0.0"}
	};
	
	var req = https.request(options, function(res) {
	  console.log("statusCode: ", res.statusCode);
	  console.log("headers: ", res.headers);
	
	  res.on('data', function(d) {
	    process.stdout.write(d);
	  });
	}).on('error', function(e) {
	  console.error(e);
	});
	
	req.write(post_data);
	req.end();

post请求数据需要用querystring包的stringify方法，将json对象转换为请求字符串`aaa=AAA&bbb=BBB`

req.write(post_data);将post数据传到服务器

------------------
参考：

>[官方文档](https://nodejs.org/api/https.html)


