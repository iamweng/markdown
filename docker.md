# Docker

## UnionFS
- docker image 采用 UnionFS 文件系统，所有的 image 都有一个 base image， 当进行修改或添加新的内容时，就会在当前 iamge 上创建新的 image 层
- dockers image 都是只读的，当容器启动时，一个新的可写成被加载到镜像的顶层，通常被称为容器层，容器之下的都叫镜像层
- bootfs: 包含 bootloader 和 kernel, bootloader 主要是引导 kernel 加载
- roofs: 包含典型 Linux 系统中的标准目录和文件

## remove docker
```bash
# 如果存在旧版本的 docker 则需要卸载它们以及相关的依赖性
yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
# 如果需要删除主机上的镜像，容器，卷或自定义文件
rm -rf /var/lib/docker
```

## install docker
```bash
# 使用 repositories 安装需要先设置 docker repositories
yum install yum-utils device-mapper-persistent-data lvm2
# 获取稳定的 docker repositories
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 启动 nigthtly repository (可选)
yum-config-manager --enable docker-ce-nightly
# 启动 test repository (测试版)
yum-config-manager --enable docker-ce-test
# 查看 docker repositores 中可用的 docker 版本
yum list docker-ce --showduplicates | sort -r
# 安装最新版本的 docker
yum install docker-ce docker-ce-cli containerd.io
# 设置启动 docker
systemctl start docker
# 设置开机自启动 docker
systemctl enable docker
# 运行 hello-world 镜像验证是否正确安装了  docker engine-community
docker run hello-world
```

## rpm install docker
```bash
# 下载.rpm文件，如果需要 nightly 或 test 版本则修改 url 中的 stable
https://download.docker.com/linux/centos/7/x86_64/stable/Packages/
# 安装 docker pagkage
yum install //package.rpm
# 设置开机自启动 docker
systemctl enable docker
# 运行 hello-world 镜像验证是否正确安装了 docker engine-community
docker run hello-world
```

## offline install docker
```bash
# 下载 docker 离线安装包
https://download.docker.com/linux/static/stable/x86_64/
# 解压 docker 离线安装包
tar -xvf docker--ce.tgz
# 将解压的 docker 文件移动到 /usr/bin
mv docker/* /usr/bin/
# 注册 docker 为 service
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

# 添加 docker.service 文件执行权限
chmod +x /etc/systemd/system/docker.service
# 重载 util 配置文件
systemctl daemon-reload
# 设置启动 docker
systemctl start docker
# 设置开机自启动 docker
systemctl enable docker
```

## change mirrors
```bash
# 更换 docker 镜像源
vi /etc/docker/daemon.json

{
    "registry-mirrors": ["http://hub-mirror.c.163.com"]
}

# 重新启动 daemon
systemctl daemon-reload
# 重新启动 docker 服务
systemctl restart docker
```

## docker image
```bash
# 修改 docker 镜像名
docker tag <IMAGE_ID> <NEW_IMAGE:TAG>
# 导入本地 docker 镜像
docker load -i
# 导入远程 docker 镜像
docker pull <IMAGE:TAG>
# 查看 docker 镜像列表
docker images / docker image list
# 删除 docker 镜像
docker rmi <IMAGE_ID>
# 删除所有 docker 镜像
docker rmi -f $(docker images -qa)
# 提交镜像 -m 描述信息 -a 作者
docker commit <CONTAINER_ID> <IMAGE:TAG>
# 打包成为 tar 包
docker save
# 解压 tar 为 image
docker load
```

## docker container
```bash
# 创建 container -i 交互 -t 可以进入 -d 后台执行 -p : 端口映射 -v 存储映射 -c shell -e 参数传递 --net 指定网络 --link 指定连通的容器 --ip 指定ip
# 启动新的 container
docker run --name
e.g. | docker run --name mysql -v /home/wengy/mysql/conf.d:/etc/mysql/conf.d -v /home/wengy/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=000000 -d mysql
# 启动已有的 container
docker start
# 重新启动 container
docker restart
# 停止 container
dooker stop
# 强制停止 container
docker kill
# 挂起 container
docker pause
# 恢复 container
dokcer unpause
# 重命名 container
docker rename <OLD_CONTAINER_NAME> <NEW_CONTAINER_NAME>
# 使用新的终端进入 container
docker exec -it <CONTAINER_ID> /bin/sh
# 使用已经打开的终端进入 container
docker attach <CONTAINER_ID>
# 删除 container -f 强制删除
docker rm
# 查看 container -a 所有 -q 只显示 id
docker ps
# 查看 container 进程
docker top
# 删除所有 container
docker rm -f $(docker ps -aq)
docker ps -aq | xargs docker rm
# container 不停止退出
ctrl + p + q
# 查看 container的元数据
docker inspect <CONTAINER_ID>
# 拷贝 container 中的文件到宿主机
docker cp <CONTAINER_ID>:<CONTAINER_ID_DIR> <DIR>
# 查看 container占用的资源
docker stats <CONTAINER_ID>
```

## docker logs
```bash
# 查看 docker 容器的 logs -t 时间戳
docker logs <CONTAINER_ID>
```

## docker repository
```bash
# 创建本地 docker registry 服务器
docker run -d -v /opt/registry:/var/lib/registry --restart=always --name registry -p 5000:5000 registry
# 启动本地 docker registry 服务器
docker run
# 验证 docker registry 服务器是否成功启动
curl http://:5000/v2
# 配置 docker 服务器配置文件，将本地 docker registry 服务器添加到其中
vim /etc/docker/daemon.json

{ "insecure-registries" :[":5000"] }
# 重新启动 daemon
systemctl daemon-reload
# 重启 docker 服务
systemctl restart docker
# 查看配置是否成功
dockers info
# 修改镜像名称
docker tag :5000/
# 将本地镜像推送到本地 docker registry 服务器
docker push :5000/
# 从本地 docker registry 服务器拉取镜像
docker pull :5000/
# 查看本地 docker registry 服务器仓库
curl http://:5000/v2/_catalog
```

## docker network
```bash
- docker 默认网卡为 docker0，使用桥接模式，采用 veth-pair 技术
- veth-pair 虚拟设备接口，成对出现，一端连着协议，一端彼此相连，通常连接虚拟设备
- 每当启动一个 container 后，宿主机中都会创建一个新的网卡，以veth开头
- 如果不指定网卡则默认使用 bridge
- bridge: 桥接模式
- none: 不配置
- host: 主机模式
- container: 容器内联通

# 查看 dockers network 列表
docker network ls
# 创建 network
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
# 查看 network 的详细信息
docker network inspect <NETWORK_NAME>
# 设置两个 container 使用 container name 连通，只能当前创建的 container 连通 --link 指定的 container，反之不行
# --link 则是通过配置 /etc/hosts 实现
docker run -it --link <CONTAINER_NAME> <IMAGE:TAG>
dockers network connect <NETWORK_NAME> <CONTAINER_NAME
```

## docker container volumes
```bash
- 存储映射是双向绑定的，即使当 container 处于 stop 状态时依旧有效
- 当 container 删除是映射到宿主机的数据并不会丢失
- 具名挂载: 指定要挂载的 volume 名: container 中的路径
- 匿名挂载: 只声明容器内的路径，将随机生成 volume 名
- 指定路径挂载：指定要挂载的系统路径: container 中的路径
- 数据卷容器的生命周期持续到没有 container 使用为止
# 查看 volumes 列表
docker volume ls
# 查看 volume 详细信息
docker volume inspect <VOLUME_ID>
# 设置映射读取方式
docker run -d -it -v <DIR>:<DIR>:rw/ro <IMAGE:TAG>
# 设置数据卷容器, 当父容器删除时，其他 container 的数据并不受影响
docker run -it --volumes-from <IMAGE_ID> <IMAGE:TAG>
```

## dockerfile
```bash
- dockerfile 用来构建dokcer镜像的脚本文件
- 大部分 image 都是基于 scratch 构建
e.g.

FROM centos

VOLUME ["voume01", "volume02"]

CMD echo "---end---"
CMD /bin/bash
# 生成 image 文件
docker build -f <FILE_NAME> -t <IMAGE:TAG> <DIR>
# 常用指令
- FROM: 基础镜像
- MAINTAINER: 维护者的信息
- RUN: 运行命令
- ADD: 添加文件，自动解压
- WORKDIR: 设置当前工作目录
- VOLUME: 设置卷，挂载主机目录
- EXPOSE: 指定对外端口
- CMD: 指定容器启动时运行的命令，只有最后一个会生效
- ENTRYPOINT: 指定容器启动时运行的命令，可以追加命令
- ONBUILD: 当构建一个被继承的 docker file 时触发
- COPY: 拷贝文件到镜像中
- ENV: 构建时候设置环境变量

e.g.

FROM centos
MAINTAINER wengy<11111111@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum install -y vim
RUN yum install -y net-tools

EXPOSE 8000
CMD echo $PATH
CMD echo "---success---"
CMD /bin/bash

# 查看 image 的构建过程
docker history <IMAGE_ID>
# 构建 tomcat

FROM centos
MAINTAINER wengy<1111111111@qq.com>

ADD jdk-8u241-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.38.tar.gz /usr/local/

RUN yum install -y vim
ENV MYPATH /usr/local/
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk1.8.0_241
ENV CLASSPATH $JAVA_HOME/lib/di.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.38
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.38
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

EXPOSE 8080

CMD /usr/local/apache-tomcat-9.0.38/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.38/logs/catalina.out

```
## docker compose
```bash
# 安装 docker compose 组件 docker compose 二进制文件需要存放在/usr/local/bin/
curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 赋予 docker compose 执行的权力
chmod +x /usr/local/bin/docker-compose
# 验证 docker compose 是否正确安装
docker-compose -version
```

## dockers stack
```bash
```

## docker config
```bash
```

## docker secret
```bash
```

## docker swarm
```bash
# 基本配置:配置主机名 hosts 文件，关闭防火墙，安装 docker-ce 服务
# 在主服务和节点编辑 docker api 配置文件
vi /lib/systemd/system/docker.service

ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock
# 重新启动 daemon
systemctl daemon-reload
# 重新启动 docker 服务
systemctl restart docker
# 拉取 swarm 镜像
docker pull swarm
# 在主服务器初始化 docker 集群，ip 地址为本机 ip
docker swarm init --advertise-addr 192.168.30.10
# 将初始化后生成的命令在 node 上运行，使其加入集群，例如:
docker swarm join --token SWMTKN-1-03y3x54rd7b2563c92mj5t3xi2d2571kusr28m339u2fcbz6xd-darhu4rd54he17o09lsrz7v37 192.168.30.10:2377
# 查看 docker 集群列表
docker node ls
# 离开当前 docker 集群
docker swarm leave
# 解散当前 docker 集群
docker swarm leave --force
# 创建 portainer 卷来进行持久化管理
docker volume create portainer_data
# 在主服务器安装 portainer 管理界面服务
docker service create --name portainer --publish 9000:9000 --replicas=1 --constraint 'node.role==manager' --mount type=bind,src=//var/run/docker.sock,dst=/var/run/docker.sock --mount type=volume,src=portainer_data,dst=/data portainer/portainer -H unix:///var/run/docker.sock
# 使用浏览器访问主服务器的 9000 端口进入 portainer 管理界面
# 创建一个具有两个 nginx 容器名字为 web 的服务，假如两个节点则每个节点都会部署一个容器
docker service create --name web --replicas 2 nginx
# 创建一个具有两个 nginx 容器名字为 web 的服务，且带有端口映射
docker service create --name web --publish 8080:80 --replicas 2 nginx
# 查看 docker 服务列表
docker service ls
# 查看服务里所有的容器
docker service ps
# 扩容服务里的容器数量
docker service scale =
# 设置节点进入维护模式
docker node update --availability drain
# 设置 docker 服务端口映射
docker service update --publish-add 8080:80
# 删除 docker 服务
docker service rm
```

## harbor

### SSL
```bash
# 创建 ssl 目录并进入
mkdir -p /data/ssl;cd /data/ssl
# 创建安全认证
openssl req -newkey rsa:4096 -nodes -sha256 -keyout ca.key -x509 -days 2.235 -out ca.crt
# 签发 www.yidaoyun.com 证书
openssl req -newkey rsa:4096 -nodes -sha256 -keyout www.yidaoyun.com.key -out www.yidaoyun.com.csr
# 自签发
openssl x509 -req -days 2.235 -in www.yidaoyun.com.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out www.yidaoyun.com.crt
# 将证书复制到 /etc/pki/ca-trust/source/anchor
cp -rfv www.yidaoyun.com.crt /etc/pki/ca-trust/source/anchors/
# 设置系统加载证书
update-ca-trust enable update-ca-trust extract
```

### master
```bash
# 解压 harbor 离线安装包，通常解压在/opt目录
tar -zxvf harbor-offline-installer-v.tgz
# 配置 harbor.cfg 文件，设置IP地址和密码，开放 https 关闭 http ，管理员账号为: admin 1.5.x版本
vi /harbor/harbor.cfg
hostname = ui_url_protocol=https ssl_cert = /data/ssl/www.yidaoyun.com.crt ssl_cert_key = /data/ssl/www.yidaoyun.com.key harbor_admin_password:
# 配置 harbor.yml 文件，设置 IP 地址和密码，开放 https 关闭 http ，管理员账号为: admin 1.9.x版本
vi /harbor/harbor.yml
hostname = https: prot:443 certificate: /data/ssl/www.yidaoyun.com.crt private_key: /data/ssl/www.yidaoyun.com.key harbor_admin_password:
# 执行 prepare 文件，生成 harbor 配置文件
./prepare
# 执行 install.sh 加载镜像，开启安全扫描功能
./install.sh --with-notary --with-clair
# 编辑 docker 服务器配置文件，将harbor服务器地址添加到其中
vim /etc/docker/daemon.json

{ "insecure-registries" :[""] }

# 重新启动 daemon
systemctl daemon-reload
# 重启 docker 服务
systemctl restart docker
# 关闭 harbor 仓库
docker-compose down
# 启动 harbor 仓库
docker-compose up -d
# 登录 harbor 仓库 -u 用户
docker login https://
# 退出 harbor 仓库
docker logout
```

### client
```bash
# 将服务器上的安全证书下发到客户机
scp -r /data/ssl/www.yidaoyun.com.crt 192.168.30.30:/etc/pki/ca-trust/source/anchors
# 在客户机上设置系统加载证书
update-ca-trust enable update-ca-trust extract
# 重启客户机上的 docker 服务
systemctl restart docker
# 编辑客户机上的 docker 配置文件，将 harbor 服务器地址添加到其中
vi /etc/docker/daemon.json

{ "insecure-registries" :[""] }
# 重新启动 daemon
systemctl daemon-reload
# 重启 docker 服务
systemctl restart docker
# 切换目录至安全证书所在目录
cd /etc/pki/ca-trust/source/anchors
# 登录 harbor 仓库
docker login https://
# 修改镜像名称
docker tag ?
# 将本地镜像推送到本地 harbor 服务器
docker push ?
# 从本地 harbor 服务器拉取镜像
docker pull ?
```

### slaver
```bash
# 在从服务器安装本地 harbor 服务器仓库，具体步骤参考主服务器，创建安全证书时需将域名更改为 www2.yidaoyun.com
# 主服务器从从服务器上拷贝安全证书
scp 192.168.30.20:/data/ssl/www2.yidaoyun.com.crt /etc/pki/ca-trust/source/anchors/
# 设置系统加载证书
update-ca-trust enable update-ca-trust extract
# 重新启动 docker 服务
systemctl restart docker
# 主服务器关闭 harbor 服务，并重新安装 harbor 服务
docker-compose down;./prepare;./install.sh --with-notary --with-clair
#使用浏览器访问主服务器 harbor 页面，在仓库管理中新建目标进行测试连接
#测试成功，点击确认后在复制管理中新建规则，名称为 slave，项目为 library，触发模式为即刻
```


## portainer

```bash
# 安装 portaniner 可视化面板
docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```