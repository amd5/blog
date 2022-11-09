## 安装`SSH plugin`插件

![c32's blog](http://qiniu.19aq.com/3977a34f428bdeb4365c5a9719f47bb2.png "c32's blog")

## 点击全局凭据

![c32's blog](http://qiniu.19aq.com/3d364c3369361f5c7470c16108be5f85.png "c32's blog")

## 点击添加凭据

![c32's blog](http://qiniu.19aq.com/85b8eb4f7f25444c45aa314851ef0b92.png "c32's blog")

## 选择类型账户密码

输入ssh账户，密码，以及该账户的描述，然后点击确定保存。

然后添加一个项目

大致可以参考


>[jenkins添加容器构建任务](https://www.19aq.com/article-348.html "jenkins添加容器构建任务")

>注：需要用到代码仓库就用，否则不需要勾选。

## 设置远程触发webhook

![c32's blog](http://qiniu.19aq.com/338e4e7c0d0598aafc0fc547fcf3f8c1.png "c32's blog")
## 勾选，使用远程SSH执行脚本
```docker
docker service update \
--image registry.cn-zhangjiakou.aliyuncs.com/ccc/php70:latest \
--with-registry-auth \
nginx_php70
```
![c32's blog](http://qiniu.19aq.com/56756f3c88be3b23b07580021980bf0a.png "c32's blog")

选择ssh主机

预编译脚本（填写构建之前执行的）

post编译脚本（填写编译成功后的）

外部webhook钩子访问地址

http://127.0.0.1:8081/job/php/build?token=aA123456

如果是带参数的钩子的话（或者webhook访问403） 参数传递构建自行百度，或参考后续文章
>系统管理-全局安全配置-项目矩阵授权策略，匿名用户权限参考

![c32's blog](http://qiniu.19aq.com/f79f8a7efd454a31127c2a27028b0c30.png "项目矩阵授权策略")

### 远程更新Swarm堆栈
```
cd /root/Swarm     #cd到yaml文件的目录下面
git checkout -f && git pull    #确保yaml文件是最新的
docker stack deploy -c nginx.yaml nginx --with-registry-auth    #更新集群
```
