---
title: Splunk-cli命令
date: 2016-03-15 09:46:48
tags: 
- splunk
- 数据分析
categories: 技术
---

### 有用的帮助主题 ###
	splunk help 

#### CLI  管理命令 ####
	splunk help commands

#### 群集的 CLI  帮助 ####
	splunk help clustering

#### Splunk  控制的 CLI  帮助 ####
	splunk help controls

#### 数据管理的 CLI  帮助 ####
	splunk help datastore
	splunk help index

#### 分布式搜索部署的 CLI  帮助 ####
	splunk help distributed

#### 转发和接收的 CLI  帮助 ####
	splunk help forwarding

#### 搜索和实时搜索的 CLI  帮助 ####
	splunk help search
	splunk help rtsearch

	splunk help search-commands
	splunk help search-fields
	splunk help search-modifiers




### CLI  管理命令 ###
	splunk [command] [object] [-parameter <value>]...


### CLI 故障排除 ###
	splunk cmd <tool>


### 使用 CLI  来管理远程 Splunk  服务器 ###
#### 对任何 CLI 命令使用  uri 参数的一般语法为： ####

	splunk command object [-parameter <value>]... -uri <specified-server>

uri 值  specified-server 的格式为：

	[http|https]://name_of_server:management_port
此外， name_of_server 可以是远程 Splunk 服务器的完全解析域名或 IP 地址。


#### 查看远程服务器上已安装的应用 ####
	splunk display app -uri https://splunkserver:8089

#### 无法远程运行的 CLI  命令 ####
除了控制服务器的命令之外，您可以远程运行所有其他 CLI 命令。这些服务器控制命令包括：

	start, stop, restart
	status, version


### 在 Windows  上启动 Splunk Enterprise ###
在 Windows 上，您可以通过以下方法之一来启动和停止 Splunk：

1.通过 Windows 服务控制面板（从  Start -> Control Panel -> Administrative Tools -> Services 访问）来启动和停止Splunk Enterprise 进程
	

- 服务器守护程序和 Web 界面：  splunkd
	
- Web 界面（仅在旧模式中）： splunkweb 。在正常操作时，此服务启动，并在收到启动请求时立即退出。

2.从命令提示符下，使用  NET START <service> 或  NET STOP <service> 命令来启动和停止 Splunk Enterprise 服务：

- 服务器守护程序和 Web 界面：  splunkd
- 
- Web 界面（仅在旧模式中）： splunkweb 。在正常操作时，此服务启动，并在收到启动请求时立即退出。

3.前往  %SPLUNK_HOME%\bin 并键入以下命令，即可同时启动、停止或重新启动这两个进程：

	> splunk [start|stop|restart]

检查 Splunk  是否正在运行
	
	splunk status

### 使用 Splunk CLI ###
要通过 Splunk CLI 来更改端口设置，请使用 CLI 命令  set 。例如，该命令将 Splunk Web 端口设为 9000：

	splunk set web-port 9000
该命令将 splunkd 端口设为 9089：

	splunk set splunkd-port 9089

更改数据存储区位置
	
	splunk set datastore-dir /var/splunk/
	splunk restart

设置最小可用磁盘空间

	splunk set minfreemb 2000



### 在 Splunk Web 中查看和管理应用或加载项对象 ###
#### 在 CLI  中更新应用或加载项 ####
要使用 CLI 来更新您的 Splunk 实例上的现有应用：

	./splunk install app <app_package_filename> -update 1 -auth <username>:<password>
Splunk 会根据从安装软件包中找到的信息来更新应用或加载项。

#### 使用 CLI  禁用应用或加载项 ####
要通过 CLI 来禁用应用：
	
	./splunk disable app [app_name] -auth <username>:<password>
注意：如果您正在运行 Splunk Free，则无需提供用户名和密码。

####  ####卸载应用或加载项
要从 Splunk 安装中删除已安装的应用：

1. （可选）删除该应用或加载项的索引数据。通常，Splunk 不会从已删除的应用或加载项访问索引数据。不过，您可以使用 Splunk CLI 的 clean 命令来从应用中删除索引数据，然后再删除该应用。参阅“使用 CLI 命令来删除索引数
据”。

2.  删除应用及其目录。应位于  $SPLUNK_HOME/etc/apps/<appname> 中。您可在 CLI 中运行以下命令：

	./splunk remove app [appname] -auth <username>:<密码>
3.  您可能需要删除为您的应用或加载项创建的用户特定目录，做法是删除这里查找到的文件（如果有）：
	
	$SPLUNK_HOME/splunk/etc/users/*/<appname>

4.  重新启动 Splunk。


### 几个例子 ###
	
	sourcetype=access_* status=200 action=purchase | top categoryId

	sourcetype=access_* status=200 action=purchase clientip=87.194.216.51 | stats count, dc(productId) by clientip

#### 使用子搜索 ####
	sourcetype=access_* status=200 action=purchase [search sourcetype=access_* status=200 action=purchase | top limit=1

	clientip | table clientip] | stats count, dc(productId), values(productId) by clientip
此处，子搜索是用方括号 [] 括起的段。此搜索  search sourcetype=access_* status=200 action=purchase | top limit=1
clientip | table clientip 与示例 1 的步骤 1 相同，但最后的管道符命令除外，  | table clientip
由于  top 命令还会返回  count 和  percent 字段，因此  table 命令用于只保留  clientip 值。


	sourcetype=access_* status=200 action=purchase [search sourcetype=access_* status=200 action=purchase | top limit=1 clientip | table clientip] | stats count AS "Total Purchased", dc(productId) AS "Total Products",values(productName) AS "Product Names" by clientip | rename clientip AS "VIP Customer"


	sourcetype=access_* status=200 | chart count AS views count(eval(action="addtocart")) AS addtocart count(eval(action="purchase")) AS purchases by productName | rename productName AS "Product Name", views AS "Views",addtocart AS "Adds to Cart", purchases AS "Purchases"

	sourcetype=access_* status=200 | stats count AS views count(eval(action="addtocart")) AS addtocart count(eval(action="purchase")) AS purchases by productName | eval viewsToPurchase=(purchases/views)*100 | eval cartToPurchase=(purchases/addtocart)*100 | table productName views addtocart purchases viewsToPurchase cartToPurchase | rename productName AS "Product Name" views AS "Views", addtocart as "Adds To Cart", purchases AS "Purchases"