# Docker

## remove docker
```bash
# 如果存在旧版本的docker则需要卸载它们以及相关的依赖性
yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
# 如果需要删除主机上的镜像，容器，卷或自定义文件
rm -rf /var/lib/docker
```

## install docker
```bash
# 使用repositories安装需要先设置docker repositories
yum install yum-utils device-mapper-persistent-data lvm2
# 获取稳定的docker repositories
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 启动nigthtly repository(可选)
yum-config-manager --enable docker-ce-nightly
# 启动test repository(测试版)
yum-config-manager --enable docker-ce-test
# 查看docker repositores中可用的docker版本
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo yum list docker-ce --showduplicates | sort -r
# 安装最新版本的docker
yum install docker-ce docker-ce-cli containerd.io
# 设置启动docker
systemctl start docker
# 设置开机自启动docker
systemctl enable docker
# 运行hello-world镜像验证是否正确安装了docker engine-community
docker run hello-world
```

## rpm install docker
```bash
# 下载.rpm文件，如果需要nightly或test版本则修改url中的stable
https://download.docker.com/linux/centos/7/x86_64/stable/Packages/
# 安装docker pagkage
yum install //package.rpm
# 设置开机自启动docker
systemctl enable docker
# 运行hello-world镜像验证是否正确安装了docker engine-community
docker run hello-world
```

## offline install docker
```bash
# 下载docker离线安装包
https://download.docker.com/linux/static/stable/x86_64/
# 解压docker离线安装包
tar -xvf docker--ce.tgz
# 将解压的docker文件移动到/usr/bin
mv docker/* /usr/bin/
# 注册docker为service
vim /etc/systemd/system/docker.service

[Unit] Description=Docker Application Container Engine Documentation=https://docs.docker.com After=network-online.target firewalld.service Wants=network-online.target [Service] Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd ExecReload=/bin/kill -s HUP $MAINPID
# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity LimitNPROC=infinity LimitCORE=infinity
# Uncomment TasksMax if your systemd version supports it.
# Only systemd 226 and above support this version.
# TasksMax=infinity
TimeoutStartSec=0
# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
# kill only the docker process,not all processes in the cgroup
KillMode=process
# restart the docker process if it exits prematurely
Restart=on-failure StartLimitBurst=3 StartLimitInterval=60s
[Install] WantedBy=multi-user.target

# 添加docker.service文件执行权限
chmod +x /etc/systemd/system/docker.service
# 重载util配置文件
systemctl daemon-reload
# 设置启动docker
systemctl start docker
# 设置开机自启动docker
systemctl enable docker
```

## change mirrors
```bash
# 更换docker镜像源
vi /etc/docker/daemon.json

{ "registry-mirrors" : ["https://docker.mirrors.ustc.edu.cn"], }

# 重新启动daemon
systemctl daemon-reload
# 重新启动docker服务
systemctl restart docker
```

## docker image
```bash
# 修改docker镜像名
docker tag
# 导入本地docker镜像
docker load -i
# 导入远程docker镜像
docker pull
# 查看docker镜像列表
docker images / docker image list
# 删除docker镜像
docker rmi
# 提交镜像
docker commit
```

## docker container
```bash
# 创建docker容器 -i 交互 -t 可以进入 -d 后台执行 -p : 端口映射 -v 存储映射
# 启动docker容器
docker run --name
# 关闭docker容器
dooker stop
# 进入docker容器
docker exec -it /bin/sh
# 删除docker容器 -f 强制删除
docker rm
# 查看docker容器 -a 所有
docker ps
# 查看docker容器运行状态
docker top
# 删除所有docker容器
docker rm -f docker ps -aq
```

## docker repository
```bash
# 创建本地docker registry服务器
docker run -d -v /opt/registry:/var/lib/registry --restart=always --name restart -p 5000:5000 registry
# 启动本地docker registry服务器
docker run
# 验证docker registry服务器是否成功启动
curl http://:5000/v2
# 配置docker服务器配置文件，将本地docker registry服务器添加到其中
vim /etc/docker/daemon.json

{ "insecure-registries" :[":5000"] }
# 重新启动daemon
systemctl daemon-reload
# 重启docker服务
systemctl restart docker
# 查看配置是否成功
dockers info
# 修改镜像名称
docker tag :5000/
# 将本地镜像推送到本地docker registry服务器
docker push :5000/
# 从本地docker registry服务器拉取镜像
docker pull :5000/
# 查看本地docker registry服务器仓库
curl http://:5000/v2/_catalog
```

## docker build
```bash
```

## docker file
```bash
```

## docker compose
```bash
# 安装docker compose组件 docker compose二进制文件需要存放在/usr/local/bin/
curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 赋予docker compose执行的权力
chmod +x /usr/local/bin/docker-compose
# 验证docker compose是否正确安装
docker-compose -version
```

## docker machine
```bash
```

## harbor

### SSL
```bash
# 创建ssl目录并进入
mkdir -p /data/ssl;cd /data/ssl
# 创建安全认证
openssl req -newkey rsa:4096 -nodes -sha256 -keyout ca.key -x509 -days 2.235 -out ca.crt
# 签发www.yidaoyun.com证书
openssl req -newkey rsa:4096 -nodes -sha256 -keyout www.yidaoyun.com.key -out www.yidaoyun.com.csr
# 自签发
openssl x509 -req -days 2.235 -in www.yidaoyun.com.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out www.yidaoyun.com.crt
# 将证书复制到/etc/pki/ca-trust/source/anchor
cp -rfv www.yidaoyun.com.crt /etc/pki/ca-trust/source/anchors/
# 设置系统加载证书
update-ca-trust enable update-ca-trust extract
```

### master
```bash
# 解压harbor离线安装包，通常解压在/opt目录
tar -zxvf harbor-offline-installer-v.tgz
# 配置harbor.cfg文件，设置IP地址和密码，开放https关闭http，管理员账号为:admin 1.5.x版本
vi /harbor/harbor.cfg
hostname = ui_url_protocol=https ssl_cert = /data/ssl/www.yidaoyun.com.crt ssl_cert_key = /data/ssl/www.yidaoyun.com.key harbor_admin_password:
# 配置harbor.yml文件，设置IP地址和密码，开放https关闭http，管理员账号为:admin 1.9.x版本
vi /harbor/harbor.yml
hostname = https: prot:443 certificate: /data/ssl/www.yidaoyun.com.crt private_key: /data/ssl/www.yidaoyun.com.key harbor_admin_password:
# 执行prepare文件，生成harbor配置文件
./prepare
# 执行install.sh加载镜像，开启安全扫描功能
./install.sh --with-notary --with-clair
# 编辑docker服务器配置文件，将harbor服务器地址添加到其中
vim /etc/docker/daemon.json

{ "insecure-registries" :[""] }

# 重新启动daemon
systemctl daemon-reload
# 重启docker服务
systemctl restart docker
# 关闭harbor仓库
docker-compose down
# 启动harbor仓库
docker-compose up -d
# 登录harbor仓库
docker login https://
```

### client
```bash
# 将服务器上的安全证书下发到客户机
scp -r /data/ssl/www.yidaoyun.com.crt 192.168.30.30:/etc/pki/ca-trust/source/anchors
# 在客户机上设置系统加载证书
update-ca-trust enable update-ca-trust extract
# 重启客户机上的docker服务
systemctl restart docker
# 编辑客户机上的docker配置文件，将harbor服务器地址添加到其中
vi /etc/docker/daemon.json

{ "insecure-registries" :[""] }
# 重新启动daemon
systemctl daemon-reload
# 重启docker服务
systemctl restart docker
# 切换目录至安全证书所在目录
cd /etc/pki/ca-trust/source/anchors
# 登录harbor仓库
docker login https://
# 修改镜像名称
docker tag ?
# 将本地镜像推送到本地harbor服务器
docker push ?
# 从本地harbor服务器拉取镜像
docker pull ?
```

### slaver
```bash
# 在从服务器安装本地harbor服务器仓库，具体步骤参考主服务器，创建安全证书时需将域名更改为www2.yidaoyun.com
# 主服务器从从服务器上拷贝安全证书
scp 192.168.30.20:/data/ssl/www2.yidaoyun.com.crt /etc/pki/ca-trust/source/anchors/
# 设置系统加载证书
update-ca-trust enable update-ca-trust extract
# 重新启动docker服务
systemctl restart docker
# 主服务器关闭harbor服务，并重新安装harbor服务
docker-compose down;./prepare;./install.sh --with-notary --with-clair
#使用浏览器访问主服务器harbor页面，在仓库管理中新建目标进行测试连接
#测试成功，点击确认后在复制管理中新建规则，名称为slave，项目为library，触发模式为即刻
```

## docker swarm
```bash
# 基本配置:配置主机名，hosts文件，关闭防火墙，安装docker-ce服务
# 在主服务和节点编辑docker api配置文件
vi /lib/systemd/system/docker.service

ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock
# 重新启动daemon
systemctl daemon-reload
# 重新启动docker服务
systemctl restart docker
# 拉取swarm镜像
docker pull swarm
# 在主服务器初始化docker集群，ip地址为本机ip
docker swarm init --advertise-addr 192.168.30.10
# 将初始化后生成的命令在node上运行，使其加入集群，例如:
docker swarm join --token SWMTKN-1-03y3x54rd7b2563c92mj5t3xi2d2571kusr28m339u2fcbz6xd-darhu4rd54he17o09lsrz7v37 192.168.30.10:2377
# 查看docker集群列表
docker node ls
# 离开当前docker集群
docker swarm leave
# 解散当前docker集群
docker swarm leave --force
# 创建portainer卷来进行持久化管理
docker volume create portainer_data
# 在主服务器安装portainer管理界面服务
docker service create --name portainer --publish 9000:9000 --replicas=1 --constraint 'node.role==manager' --mount type=bind,src=//var/run/docker.sock,dst=/var/run/docker.sock --mount type=volume,src=portainer_data,dst=/data portainer/portainer -H unix:///var/run/docker.sock
# 使用浏览器访问主服务器的9000端口进入portainer管理界面
# 创建一个具有两个nginx容器名字为web的服务，假如两个节点则每个节点都会部署一个容器
docker service create --name web --replicas 2 nginx
# 创建一个具有两个nginx容器名字为web的服务，且带有端口映射
docker service create --name web --publish 8080:80 --replicas 2 nginx
# 查看docker服务列表
docker service ls
# 查看服务里所有的容器
docker service ps
# 扩容服务里的容器数量
docker service scale =
# 设置节点进入维护模式
docker node update --availability drain
# 设置docker服务端口映射
docker service update --publish-add 8080:80
# 删除docker服务
docker service rm
```
