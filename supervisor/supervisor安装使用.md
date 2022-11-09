```bash
yum update
python -V
py2执行
pip install supervisor
py3执行
pip2 install supervisor
生成配置文件
echo_supervisord_conf > /etc/supervisord.conf
启动
supervisord -c /etc/supervisord.conf
查看 supervisord 是否在运行：
ps aux | grep supervisord
查看状态
systemctl status supervisor.service
重启服务
systemctl restart supervisor.service
开启WEB 9001 
[inet_http_server]
port=0.0.0.0:9001
username=c32
password=c32
上面4个取消注释，IP修改为0.0.0.0 打开防火墙，然后保存后进行reload
[include]
files = /etc/supervisor/conf.d/*.conf
最后2行取消注释，建立对应目录 继续reload
*.conf 进程启动配置文件 在本文最下面有参考
/etc/supervisord.conf   配置在本文最下面
输入命令 supervisorctl 进入 shell 交互界面，就可以在下面输入命令了：
supervisord -c /etc/supervisord.conf   启动supervisor
supervisorctl reload :修改完配置文件后重新启动supervisor
supervisorctl status :查看supervisor监管的进程状态
supervisorctl start 进程名 ：启动XXX进程
supervisorctl stop 进程名 ：停止XXX进程
supervisord开机启动
vi /lib/systemd/system/supervisor.service
创建supervisor.service文件
写入以下内容：
==============================================
[Unit]
Description=supervisor
After=network.target
[Service]
Type=forking
ExecStart=/usr/bin/supervisord -c /etc/supervisord.conf
ExecStop=/usr/bin/supervisorctl $OPTIONS shutdown
ExecReload=/usr/bin/supervisorctl $OPTIONS reload
KillMode=process
Restart=on-failure
RestartSec=42s
[Install]
WantedBy=multi-user.target
==============================================
设置开机启动
systemctl enable supervisor.service
systemctl daemon-reload
修改文件权限为766
cd /lib/systemd/system
chmod 766 supervisor.service
配置开机启动
chkconfig supervisor on
==========================================
/etc/supervisord.conf
以下取消注释 配置参考：
chmod=0700
[inet_http_server]
port=0.0.0.0:9001
username=c32
password=c32
[include]
files = /etc/supervisor/conf.d/*.conf
======================================
进程启动配置文件  nat2222.conf
[program:nat2222]
process_name=%(program_name)s ; process_name expr (default %(program_name)s)
command=/www/1.sh run         ; the program (relative uses PATH, can take args)
stdout_logfile=/tmp/supervisor_nat2222.log        ; stdout log path, NONE for none; default AUTO
stdout_events_enabled=false   ; emit events on stdout writes (default false)
autostart=true                ; start at supervisord start (default: true)
autorestart=true              ; when to restart if exited after running (def: unexpected)
startsecs=1                   ; # of secs prog must stay up to be running (def. 1)
priority=999                  ; the relative start priority (default 999)
stopasgroup=true              ; send stop signal to the UNIX process group (default false)
killasgroup=true              ; SIGKILL the UNIX process group (def false)
numprocs=1                    ; number of processes copies to start (def 1)
stopsignal=QUIT               ; signal used to kill process (default TERM)
stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
```