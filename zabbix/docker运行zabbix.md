```docker
如果自己有数据，可以不使用第一行创建数据库，
docker run --name zabbixdb -t -e MYSQL_DATABASE="zabbix" -e MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbix_pwd" -e MYSQL_ROOT_PASSWORD="root_pwd" -p 3306:3306 -d mysql:5.7
docker run --name zabbixserver -t -e DB_SERVER_HOST="192.168.1.1" -e MYSQL_DATABASE="zabbix" -e MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbix" -e MYSQL_ROOT_PASSWORD="root" -e ZBX_JAVAGATEWAY="zabbix-java-gateway" -p 10051:10051 -p 10050:10050 -d zabbix/zabbix-server-mysql:latest
docker run --name zabbixweb -t -e DB_SERVER_HOST="192.168.1.1" -e MYSQL_DATABASE="zabbix" -e MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbix" -e MYSQL_ROOT_PASSWORD="root" -e PHP_TZ="Asia/Shanghai" -p 80:80 -d zabbix/zabbix-web-nginx-mysql:latest 
docker run --name zabbixagent -e ZBX_HOSTNAME="Zabbix server" -e ZBX_SERVER_HOST="zabbixserver" -d zabbix/zabbix-agent:alpine-trunk
zabbix/zabbix-java-gateway:latest
用于监控java程序
```