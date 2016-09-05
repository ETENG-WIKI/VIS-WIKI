安装docker
---	
	curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -
	
	service docker restart//启动docker 

	docker --version或docker info //查看docker版本
安装希云
---
* 安装Controller
	
		curl -SsL -o /tmp/csphere-install.sh https://csphere.cn/static/csphere-install-v2.sh
		sudo env ROLE=controller CSPHERE_VERSION=1.0.1 /bin/sh /tmp/csphere-install.sh

	**注意：** 希云安装，如果是多台服务器，只需要在一台上面装controller
* 安装agent

	打开浏览器，访问controller A主机的1016端口，第一次访问填入管理员邮箱密码注册，即可看到控制台的界面。 点击左侧的“主机”菜单，进入主机列表页面，点击“添加主机”并复制脚本，在Agent主机安装Agent程序，即可开始希云cSphere旅途。

添加docker命令
---
	yum install wget
	wget -P ~ https://github.com/yeasy/docker_practice/raw/master/_local/.bashrc_docker;
	echo "[ -f ~/.bashrc_docker ] && . ~/.bashrc_docker" >> ~/.bashrc; source ~/.bashrc
	
	可执行：
	docker-pid 可以获取某个容器的 PID：docker-pid myjenkins
	docker-enter 可以进入容器或直接在容器内执行命令：docker-enter myjenkins
获取jenkins
---
	docker pull yitengshidai/jenkins:0.1
运行jenkins
---
	docker run -d --name myjenkins -p 8080:8080 -v /var/jenkins_home yitengshidai/jenkins:0.1
获取tomcat
---
	docker pull yitengshidai/tomcat:0.1

运行tomcat
---
	docker run -d --name tomcat-vis -p 8080:8080  -e JAVA_OPTS='-Xms1024m -Xmx1024m' yitengshidai/tomcat:0.1
centos7端口号开放
---
 * 开启端口
 
 		firewall-cmd --zone=public --add-port=81/tcp --permanent
		命令含义：
		--zone #作用域
		--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
		--permanent   #永久生效，没有此参数重启后失效
 * 重启防火墙
 
	 	firewall-cmd --reload 
 * 查看已开启的端口号
 
	 	firewall-cmd --list-all 

nginx安装依赖
---
	yum install -y readline-devel pcre-devel openssl-devel gcc

nginx官网
---

[http://openresty.org/](http://openresty.org/ "nginx官网")

获取nginx
---
	cd /opt/tools
	wget https://openresty.org/download/openresty-1.9.7.5.tar.gz

解压nginx
---
	tar -xzvf openresty-1.9.7.5.tar.gz
	
编译安装
---
	cd openresty-1.9.7.5
	 ./configure --prefix=/opt/local/openresty
	make && make install
redis官网
---
[http://redis.io/](http://redis.io/ "redis官网")
安装redis(最新版3.2.1)
---
	cd /opt/tools
	wget http://download.redis.io/releases/redis-3.2.1.tar.gz
	tar xzf redis-3.2.1.tar.gz
	cd redis-3.2.1
	make && make install

redis配置调整
---

	mkdir /etc/redis/
	cp redis.conf /etc/redis/6379.conf
	vi /etc/redis/6379.conf
	daemonize yes #默认值no，该参数用于定制redis服务是否以守护模式运行。
	protected-mode no
	bind 127.0.0.1 #前加注释，默认本机访问
	save ""

redis添加密码(可忽略)
---
	redis 127.0.0.1:6379> AUTH PASSWORD
	(error) ERR Client sent AUTH, but no password is set
	redis 127.0.0.1:6379> CONFIG SET requirepass "mypass"
	OK
	redis 127.0.0.1:6379> AUTH mypass
	Ok
	
测试nginx配置mds服务
---
	http://*****/etmds/mds/test?name=zhang
	出现 zhang:testGEThello 表示成功


获取elasticsearch镜像
---

	docker run -d -p 9200:9200 -p 9300:9300 --name=elasticsearch -e "ES_JAVA_OPTS=-Xms1g -Xmx1g" elasticsearch:2.3.4

	docker-enter elasticsearch

安装分词插件
---

	cd /usr/share/elasticsearch
	./bin/plugin install http://maven.nlpcn.org/org/ansj/elasticsearch-analysis-ansj/2.3.4/elasticsearch-analysis-ansj-2.3.4-release.zip

安装vim编辑器
---
	apt-get install -y vim
设置ansj作为默认分词器:
---
	cd config 
	vi elasticsearch.yml 添加下面信息 保存
	 #默认分词器,索引
  	index.analysis.analyzer.default.type: index_ansj
 	#默认分词器,查询
 	index.analysis.analyzer.default_search.type: query_ansj
添加索引库
---
* 建立索引，并配置type（相当于数据表）
		
		/**
		* curl生成配置
		* -d 后面是配置json
		* 配置json DEMO:
		* (wtd_worktrend为type，
		*  index配置为not_analyzed时，不进行分词)
		*  {
		*    "mappings": {
		*      "wtd_worktrend": {
		*        "properties": {
		*          "customer_name": {
		*            "type": "string"
		*          },
		*          "is_pub": {
		*            "type": "long"
		*          },
		*          "region_area": {
		*            "type": "string"
		*          },
		*          "region_name": {
		*            "type": "string"
		*          },
		*          "spl_flag": {
		*            "type": "long"
		*          },
		*          "wtd_address": {
		*            "type": "string"
		*          },
		*          "wtd_content": {
		*            "type": "string"
		*          },
		*          "wtd_name": {
		*            "type": "string"
		*          },
		*          "wtd_time": {
		*            "type": "date",
		*            "format": "yyyy-MM-dd HH:mm:ss"
		*          },
		*          "from_code": {
		*            "type": "string",
		*            "index": "not_analyzed"
		*          },
		*          "code_name": {
		*            "type": "string"
		*          },
		*          "corp_id": {
		*            "type": "string",
		*            "index": "not_analyzed"
		*          },
		*          "user_id": {
		*            "type": "string",
		*            "index": "not_analyzed"
		*          },
		*          "cus_id": {
		*            "type": "string",
		*            "index": "not_analyzed"
		*          }
		*        }
		*      }
		*    }
		*  }
		*
		* http://localhost:9200/villagesale villagesale为索引名称
		*/
		curl -H 'Content-Type: application/json' -X PUT -d '{"mappings":{"wtd_worktrend":{"properties":{"customer_name":{"type":"string"},"is_pub":{"type":"long"},"region_area":{"type":"string"},"region_name":{"type":"string"},"spl_flag":{"type":"long"},"wtd_address":{"type":"string"},"wtd_content":{"type":"string"},"wtd_name":{"type":"string"},"wtd_time":{"type":"date","format": "yyyy-MM-dd HH:mm:ss"},"from_code":{"type":"string","index":"not_analyzed"},"code_name": {"type": "string"},"corp_id":{"type":"string","index":"not_analyzed"},"user_id":{"type":"string","index":"not_analyzed"},"cus_id":{"type":"string","index":"not_analyzed"}}}}}' http://localhost:9200/villagesale
	
		出现如下信息表示成功
		{"acknowledged":true}
* 导入数据

以上出错的解决办法：

  * 删除索引库，然后重新走步骤一和二：

		curl -X DELETE 'http://localhost:9200/villagesale'

  * 返回数据为下面的结果时，则删除成功。 

		{"acknowledged":true}

添加查询插件
---
	cd ..
	./bin/plugin install mobz/elasticsearch-head

检查天气
---
	http://api.map.baidu.com/location/ip?ak=YnTqNl3twGaEHa4Zifl7mxZ9&ip=*.*.*.*