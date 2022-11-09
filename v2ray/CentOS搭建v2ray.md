```bash
https://github.com/v2fly/fhs-install-v2ray
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
cat > /usr/local/etc/v2ray/config.json << EOF
{
    "inbounds": [
        {
            "port": 12086, 
            "protocol": "vmess",
            "settings": {
                "clients": [
                    {
                        "id": "b831381d-6324-4d53-ad4f-8cda48b30811"
                    }
                ]
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom"
        }
    ]
}
EOF
systemctl start nginx.service
systemctl enable nginx.service
netstat -ntlp
Windows      .NET 4.6  Core version
https://github.com/2dust/v2rayN
带http pac版本
https://github.com/2dust/v2rayN/releases/download/3.29/v2rayN-Core.zip
OSX
https://github.com/Cenmrev/V2RayX  未测试
Android
https://github.com/2dust/v2rayNG
其他客户端下载地址
https://www.hgblog.net/1056.html
```