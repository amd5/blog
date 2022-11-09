```bash
内网frp穿透
首先到git下载最新的稳定版本
https://github.com/fatedier/frp/releases
frp_$version_linux_amd64.tar.gz
下载到服务器后，解压文件。
----------------------------------------
配置公网服务器：
首先删掉frpc、frpc.ini
vi frps.ini
增加
[common]
bind_port = 7000           #与客户端绑定的进行通信的端口
vhost_http_port = 6081     #访问客户端web服务自定义的端口号
保存然后启动服务./frps -c ./frps.ini
----------------------------------------
配置客户端（内网服务器）
删掉frps、frps.ini
vi ./frpc.ini
增加
[common]
server_addr = 111.222.333.444   #公网服务器ip
server_port = 7000            #与服务端bind_port一致
#公网通过ssh访问内部服务器
[ssh]
type = tcp              #连接协议
local_ip = 127.0.0.1    #内网服务器ip
local_port = 22         #ssh默认端口号
remote_port = 6000      #自定义的访问内部ssh端口号
#公网访问内部web服务器以http方式
[web]
type = http         #访问协议
local_port = 8081   #内网web服务的端口号
custom_domains = repo.iwi.com   #所绑定的公网服务器域名，一级、二级域名都可
保存然后执行启动
./frpc -c ./frpc.ini
Supervisor 执行命令为：
/home/frp/frpc -c /home/frp/frpc.ini
多站点只需要配置多tcp即可
备用
------------------------------
使用systemctl来控制启动
vi /lib/systemd/system/frps.service 
[Unit]
Description=fraps service
After=network.target syslog.target
Wants=network.target
[Service]
Type=simple
#启动服务的命令（此处写你的frps的实际安装目录）
ExecStart=/your/path/frps -c /your/path/frps.ini
[Install]
WantedBy=multi-user.target
------------------------------
然后就启动frps 
sudo systemctl start frps 
再打开自启动 
sudo systemctl enable frps
```