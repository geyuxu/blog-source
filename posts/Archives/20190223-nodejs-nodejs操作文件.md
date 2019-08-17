---
title: "nodejs操作文件"
date: 2016-03-18
tags: [nodejs] 
---
向文件中写数据：

	var txt = '保存的数据';
	fs.writeFile('message.txt', 	
		 txt, (err) => {
			if (err) throw err;
			console.log('It\'s saved!');
		}
	);
	
fs.writeFile的同步方法是

	fs.writeFileSync(file, data[, options])

从文件中读取数据：
	
	fs.readFile('/etc/passwd', (err, data) => {
		if (err) throw err;
		console.log(data);
	});

-----------------
[node官方文档](https://nodejs.org/dist/latest-v5.x/docs/api/fs.html#fs_fs_writefile_file_data_options_callback) 