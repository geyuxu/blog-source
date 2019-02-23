title: "nodejs调用dnspodApi"
date: 2015-05-23 22:30:20
tags: [nodejs,dnspod,api] 
---
dnspod是国内的一款DNS产品，关于它的介绍请自行搜索，本文（系列）讲的是如何使用nodejs调用dnspod提供的api实现ddns功能。

<!--more-->		
	var https = require('https');
	var querystring = require('querystring');

	var post_data = querystring.stringify({
		login_token:'token',
		format:'json',// 返回的数据格式，可选，默认为xml，建议用json
		lang:'cn'//返回的错误语言，可选，默认为en，建议用cn
		error_on_empty:'yes' //{yes,no} 没有数据时是否返回错误，可选，默认为yes，建议用no
	});

	var options = {
	  hostname: 'dnsapi.cn',
	  port: 443,
	  path: '/Info.Version',
	  method: 'POST',
	  headers: {"Content-type": "application/x-www-form-urlencoded", "Accept": "text/json", "User-Agent": "geyuxu-nodejs/0.0.1(register@geyuxu.com)"}
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

login_token Token,如果由此配置，则不需要配置用户账号和用户密码  
login_email 用户帐号，必选  
login_password 用户密码，必选  
format {json,xml} 返回的数据格式，可选，默认为xml，建议用json  
lang {en,cn} 返回的错误语言，可选，默认为en，建议用cn  
error_on_empty {yes,no} 没有数据时是否返回错误，可选，默认为yes，建议用no  
user_id 用户的ID，可选，仅代理接口需要， 用户接口不需要提交此参数


官方文档要求必须写上User-Agent，User-Agent原本是浏览器的标识，如`Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0)`,dnspod要求的格式为`程序英文名称/版本(联系邮箱)`。

必须为POST请求，GET请求会提示错误。

-----------------------------
参考：
>[nodejs官方文档](https://support.dnspod.cn/Support/api)

>[dnspod官方api文档](https://support.dnspod.cn/Support/api)

>[dnspod官方python调用demo](https://github.com/DNSPod/dnspod-python)