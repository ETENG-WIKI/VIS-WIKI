#[CentOS6安装ftp服务器](http://jingyan.baidu.com/article/95c9d20d4e1222ec4e7561fc.html?qq-pf-to=pcqq.group)
##安装vsftp
* 查看是否已安装vsftpd
	
		rpm -qa |grep vsftpd
* 安装vsftpd

		yum install vsftpd -y 

##配置虚拟用户

所谓虚拟用户就是没有使用真实的帐户，只是通过映射到真实帐户和设置权限的目的。虚拟用户不能登录CentOS系统。

修改配置文件：

	#设定不允许匿名访问
	anonymous_enable=NO
	local_enable=YES
	local_umask=022
	dirmessage_enable=YES
	connect_from_port_20=YES
	#设定支持ASCII模式的上传和下载功能
	ascii_upload_enable=YES
	ascii_download_enable=YES
	#chroot_local_user=YES
	#使用户不能离开主目录
	chroot_list_enable=YES
	chroot_list_file=/etc/vsftpd/vuser_passwd.txt
	#PAM认证文件名，PAM将根据/etc/pam.d/vsftpd进行认证
	pam_service_name=vsftpd
	user_list_enable=YES
	tcp_wrapper=YES
	guest_enable=YES
	guest_username=ftp
	user_congig_dir=/etc/vsftpd/vuser_conf

##使用Berkeley DB进行认证

* 安装工具
  
   		yum install db4 db4-utils

* 创建密码文件

		vi /etc/vsftpd/vuser_passwd.txt
		ftpuser
		ftppasswd

    **注意： 奇行是用户名，偶行是密码**

* 生成虚拟用户认证
	* 编辑认证文件/etc/pam.d/vsftpd 全部注释，添加下面内容
	
			vi  /etc/pam.d/vsftpd
			auth required pam_userdb.so db=/etc/vsftpd/vuser_passwd
			account required pam_userdb.so db=/etc/vsftpd/vuser_passwd
	* 生成虚拟用户认证的db文件
			
			db_load -T -t hash -f /etc/vsftpd/vuser_passwd.txt /etc/vsftpd/vuser_passwd.db

##创建虚拟用户配置文件
	
* 创建配置文件目录

		cd /etc/vsftpd
		mkdir vuser_conf

* 创建配置文件

		#文件名是vuser_passwd.txt文件中提供的用户名，否则无效		
		vi /etc/vsftpd/vuser_conf/ftpuser

		#虚拟用户根目录，根据实际情况修改
		local_root=/opt/logs
		write_enable=YES
		anon_umask=022
		anon_world_readable_only=NO
		anon_upload_enable=YES
		anon_mkdir_write_enable=YES
		anon_other_write_enable=YES

##定义ftp访问目录

	mkdir -p /opt/logs
	
	
##开启ftp端口

	vi /etc/sysconfig/iptables
	#开放21端口
	-A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT
	#重启防火墙
	service iptables restart


##启动ftp服务

	#启动ftp
	service vsftpd start
	#关闭ftp
	service vsftpd stop
	#重启ftp
	service vsftpd restart

	
		