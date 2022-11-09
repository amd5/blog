```bash
安装JDK
yum install -y java wget git
查看是否安装成功
java -version
安装Docker
wget http://c32.19aq.com/docker.sh && chmod +x docker.sh && ./docker.sh
添加Jenkins库到yum库
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
导入key
rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
安装jenkins
yum install -y jenkins
-------------------------------------
如果不能安装就到官网下载jenkis的rmp包，官网地址（http://pkg.jenkins-ci.org/redhat-stable/）
wget http://pkg.jenkins-ci.org/redhat-stable/jenkins-2.7.3-1.1.noarch.rpm
rpm -ivh jenkins-2.7.3-1.1.noarch.rpm
-------------------------------------
配置jenkis的端口
vi /etc/sysconfig/jenkins
找到修改端口号：
JENKINS_PORT="8080"  此端口不冲突可以不修改 
启动jenkins
service jenkins start/stop/restart
查看端口是否正常开启
netstat -ntlp
直接访问http://192.168.1.2:8080/
如果访问不了
防火墙添加允许端口
firewall-cmd --zone=public --add-port=8080/tcp --permanent
重载防火墙立即生效
firewall-cmd --reload
访问http://192.168.1.2:8080/ 即可正常打开
打开jenkins 
在浏览器中访问 
首次进入会要求输入初始密码如下图， 
初始密码在：/var/lib/jenkins/secrets/initialAdminPassword 
##############################################
安装完成开始创建构建任务
https://www.19aq.com/article-348.html
解决 jenkins报错docker.sock: permission denied.
https://www.19aq.com/article-344.html
##############################################
备注：
安装成功后Jenkins将作为一个守护进程随系统启动
系统会创建一个“jenkins”用户来允许这个服务，如果改变服务所有者，同时需要修改/var/log/jenkins, /var/lib/jenkins, 和/var/cache/jenkins的所有者
启动的时候将从/etc/sysconfig/jenkins获取配置参数
默认情况下，Jenkins运行在8080端口，在浏览器中直接访问该端进行服务配置
Jenkins的RPM仓库配置被加到/etc/yum.repos.d/jenkins.repo
```