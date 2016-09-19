#Jenkins安装阿里云OSS服务插件

##插件地址
	
上传文件至阿里云插件下载地址：

[http://repository-proxy.fit2cloud.com:8080/content/repositories/releases/org/jenkins-ci/plugins/aliyun-oss/0.6/aliyun-oss-0.6.hpi ](http://repository-proxy.fit2cloud.com:8080/content/repositories/releases/org/jenkins-ci/plugins/aliyun-oss/0.6/aliyun-oss-0.6.hpi  "上传文件至阿里云插件") 
##插件安装

系统管理---> 管理插件 --->高级---->上传插件
![找到高级](http://i.imgur.com/98hglhX.png)

![找到上传插件](http://i.imgur.com/oOKf7Fu.png)
##配置插件
系统管理---> 系统设置 --->阿里云OSS帐号设置
![配置阿里云帐号信息](http://i.imgur.com/4vcgZ3h.png)

阿里云EndPoint后缀:

	如果jenkins在阿里云服务器安装建议配置"-internal.aliyuncs.com";
	在其他服务器上建议配置".aliyuncs.com".
##项目配置
找到对应的项目--->配置--->增加构建后操作步骤--->上传Artifacts到阿里云OSS

![配置上传信息](http://i.imgur.com/8Gislrc.png)

说明：

	Bucket名称: artifact要存放的bucket
	要上传的artifacts: 文件之间用;隔开。支持通配符描述，比如 *.apk
	Object前缀设置：可以设置object key的前缀 比如：VISAPPTEST