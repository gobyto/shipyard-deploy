# shipyard-deploy
shipyard-deploy中文版

wget https://raw.githubusercontent.com/gobyto/shipyard-deploy/master/shipyard-deploy
chmod 755 shipyard-deploy
./shipyard-deploy

浏览器输入:http://主机IP:8080
默认账号:admin
默认密码:shipyard


shipyard中文版安装教程（附安装脚本）
三、shipyard中文版安装(CentOS)
注：本文安装操作均在root用户下，安装前需先安装Docker (传送门)
下载所需docker镜像

docker pull rethinkdb
docker pull microbox/etcd
docker pull shipyard/docker-proxy
docker pull swarm
docker pull dockerclub/shipyard

修改原安装脚本为中文版安装脚本

#下载官方脚本
wget https://shipyard-project.com/deploy
若下载失败请使用
wget https://raw.githubusercontent.com/shipyard/shipyard-project.com/master/site/themes/shipyard/static/deploy

#替换官方脚本
grep -n shipyard:latest deploy
sed -i 's/shipyard\/shipyard:latest/dockerclub\/shipyard:latest/g' deploy

设置web访问端口(根据需要修改)

#检查8080端口是否被占用，若占用需修改端口
yum install -y net-tools    //安装net-tools工具包，若已安装可跳过此步骤
netstat -tlnp | grep 8080   //查看宿主机8080端口是否被占用
#配置修改
grep -n 'PORT:-8080' deploy
SHIPYARD_PORT=${PORT:-8080}
修改为
SHIPYARD_PORT=${PORT:-指定端口}

安装与删除

sh deploy                                //安装
cat deploy | ACTION=remove bash          //删除

使用shipyard

浏览器输入:http://主机IP:8080
默认账号:admin
默认密码:shipyard

安装过程中错误,常用的解决办法
容器冲突：

#出现错误一般都是提示容器冲突,如果刚搭建,可以直接把容器全部停止并删除
docker stop $(docker ps -a -q)        //停止所有服务
docker rm $(docker ps -a -q)          //删除所有服务

#也可以根据提示来找到容器的ID进行停止删除
docker ps -a
docker stop ID
docker rm ID

四、 如何使用
如何增加一个节点

curl https://shipyard-project.com/deploy | ACTION=node DISCOVERY=etcd://主服务器IP:4001 bash 
若下载失败请使用
curl -sSL  https://raw.githubusercontent.com/shipyard/shipyard-project.com/master/site/themes/shipyard/static/deploy | ACTION=node DISCOVERY=etcd://主节点IP:4001 bash -s

五、安装脚本下载
文件说明

install.sh      //一键安装脚本
deploy          //官方安装脚本修改版，若已下载前文所需镜像可直接运行此脚本安装
1
2
脚本下载：https://wwwfcwys.oss-cn-shenzhen.aliyuncs.com/typecho/2017/12/27/shipyard.tar.gz

注：作者由于没有时间与精力继续维护下去，在去年八月份就开始询问是否有人感兴趣接手该项目，可惜过了几个月依旧没有人出现，只能无奈的决定停止这个项目，官网也被关掉了。若下载安装失败请尝试替换下载链接为https://raw.githubusercontent.com/shipyard/shipyard-project.com/master/site/themes/shipyard/static/deploy
参考文章：Centos-Docker-shipyard中文版安装
