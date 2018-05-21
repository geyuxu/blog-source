title: "varnish 安装"
date: 2016-11-11 13:09:00
tags: [varnish] 
---

1. 安装

	rpm -ivh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
	rpm --nosignature -i https://repo.varnish-cache.org/redhat/varnish-4.1.el6.rpm
	yum install varnish

	**EPEL 是yum的一个软件源,里面包含了许多基本源里没有的软件。**
  **官方的安装文档：http://varnish-cache.org/releases/index.html**
<!--more-->
2. 配置文件

	1) varnish 配置文件： /etc/sysconfig/varnish

		* VARNISH_STORAGE_SIZE 为缓存文件大小
		
		VARNISH_STORAGE_SIZE=100T
		
		* VARNISH_STORAGE 为缓存方式，配置为 file 形式
		
		VARNISH_STORAGE_FILE=/root/varnish.cache
		
		VARNISH_STORAGE="file,${VARNISH_STORAGE_FILE},${VARNISH_STORAGE_SIZE}"

	2) vcl 配置文件： /etc/varnish/*.vcl

3. 启动 varnish
	service varnish start
	service varnishlog start

4. 加载 vcl

1） 进入 varnishadm 

	varnishadm -S /etc/varnish/secret -T 127.0.0.1:6082

2） 查看现有配置

	varnish> vcl.list

	available  auto/cold          0 boot
	available  auto/cold          0 ceph1
	available  auto/warm          0 ceph2
	available  auto/warm          0 ceph3
	available  auto/cold          0 ceph4
	available  auto/cold          0 ceph5
	available  auto/cold          0 ceph6
	available  auto/cold          0 ceph7
	active     auto/warm          0 ceph8

	* active 为现在活动的配置
	* boot/ceph1-8 为加载过的配置

3） 加载一个 vcl 配置

	varnish> vcl.load ceph9 ceph.vcl
	
	* ceph9 为自定义名称，与上面 list 中的名称不能重复
	* ceph.vcl 默认 path 为 /etc/varnish/
	* 此时 ceph9 的状态为 available

	available  auto/warm          0 ceph9

4. 启用配置

	varnish> vcl.use ceph9
