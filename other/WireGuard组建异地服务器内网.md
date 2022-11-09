```bash
WireGuard 组建异地服务器内网
uname -r  内核大于3.10
安装方式一:(推荐)
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
curl -o /etc/yum.repos.d/jdoss-wireguard-epel-7.repo https://copr.fedorainfracloud.org/coprs/jdoss/wireguard/repo/epel-7/jdoss-wireguard-epel-7.repo
yum install wireguard-dkms wireguard-tools
安装方式二:
yum install epel-release elrepo-release
yum install yum-plugin-elrepo
yum install kmod-wireguard wireguard-tools
# 创建目录
mkdir -p /etc/wireguard
cd /etc/wireguard
# 创建server公私钥
wg genkey | tee privatekey-server | wg pubkey > publickey-server
# 创建client端的公私钥:
wg genkey | tee privatekey-client_name | wg pubkey > publickey-client_name
# 创建配置文件
cat > /etc/wireguard/wg0.conf << EOF
[Interface]
Address = 192.168.19.1/24
DNS = 223.5.5.5,1.2.4.8,114.114.114.114,8.8.8.8
ListenPort = 51820
#服务端私钥
PrivateKey = eOEsoekLwkoLTVBAUDLQO0FdRQsDneUi2tzSxgSr0E0=
# 声明一个节点,节点公钥和节点ip
[Peer]
#客户端公钥
PublicKey = bBfh16iyea0aWK0mZHQVkzM8rE9uhMmLWDuJ2o0kbnc=
AllowedIPs = 192.168.19.2/32
EOF
# 启动服务
systemctl restart wg-quick@wg0.service;
wg-quick down wg0 && wg-quick up wg0
# 重载配置
systemctl reload wg-quick@wg0.service;
systemctl status wg-quick@wg0.service;
# 重载网卡
wg syncconf wg0 <(wg-quick strip wg0)
# 重启网卡
wg-quick down wg0 && wg-quick up wg0
# 停止网卡
wg-quick down wg0
# 查看客户端状态
wg
#开启内核转发功能,系统其它设置打开转发
echo 1 > /proc/sys/net/ipv4/ip_forward
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
echo "net.ipv4.conf.all.proxy_arp = 1" >> /etc/sysctl.conf
sysctl -p
windows客户端
https://download.wireguard.com/windows-client/wireguard-installer.exe
#客户端配置
[Interface]
# 客户端私钥
PrivateKey = aMtNXl1Ok4gjtSp/SUdpWjpxo2rSAELBxu7BpTTbEkc=
Address = 192.168.19.2/24
DNS = 223.5.5.5,1.2.4.8,114.114.114.114,8.8.8.8
[Peer]
# 服务端公钥
PublicKey = barzgwUjBeyjGpKrgU6JaxG2gItrNMZppZQGZsglM0A=
AllowedIPs = 0.0.0.0/0
Endpoint = 47.245.30.72:51820
PersistentKeepalive = 25
# 设置IP地址伪装 ----不确定
# 允许防火墙伪装IP
firewall-cmd --add-masquerade
# 检查是否允许伪装IP
firewall-cmd --query-masquerade
# 禁止防火墙伪装IP
firewall-cmd --remove-masquerade
```