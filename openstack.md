#  OpenStack

* @Author:yuxin_weng
* @Date:2020/03/05

## 基本工具

```bash
# 安装基本工具
yum install lezsz wget vim unzip net-tools
```

## Ntpdate

```bash
# 安装ntpdate服务
yum install ntpdate
# 配置
ntpdate time1.aliyun.com
ntpdate time2.aliyun.com
```

## OpenStack

```bash
# 安装openstack软件包
yum install centos-release-openstack-rocky
# 安装openstack客户端
yum install python-openstackclient
# 安装openstack-selinux包
yum install openStack-selinux
```
## MariaDB

```bash
# 安装数据库软件包
yum install mariadb mariadb-server python2-PyMySQL
# 编辑数据库配置文件
vi /etc/my.cnf.d/openstack.cnf

[mysqld]
bind-address= <IP_ADDR>
default-storage-engine=innodb
innodb_file_per_table
max_connections=4096
collation-server=utf8_general_ci
character-set-server=utf8

# 设置开机启动mariadb服务
systemctl enable mariadb.service
# 启动mariadb服务
systemctl start mariadb.service
# 运行mysql_secure_installation安全脚本
mysql_secure_installation
```

## RabbitMQ
```bash
# 安装rabbitMQ依赖包
yum install gcc glibc-devel make ncurses-devel openssl-devel xmlto
# 使用wget下载rabbitMQ软件包
wget https://github.com/rabbitmq/rabbitmq-server/release/download/ \
v3.7.2/rabbitmq-server-3.7.2-1.el7.noarch.rpm
# 使用wget下载erlang源码包
wget http://erlang.org/download/otp_src_19.3.tar.gz
# 解压erlang源码包
tar -zxvf otp_src_19.3.tar.gz
# 配置erlang安装信息
./configure --prefix=/usr/local/erlang --without-javac
# 编译并安装erlang
make && make install
# 编辑环境变量配置文件,向其添加erlang环境变量
vim /etc/profile

ERL_HOME=/usr/local/erlang
PATH=$ERL_HOME/bin:$PATH
exportERL_HOMEPATH

# 更新环境变量配置文件
source /etc/profile
# 安装rabbitMQ
rpm -ivh --nodeps rabbitmq-server-3.7.2-1.el7.noarch.rpm
# 安装错误则设置rabbitMQ文件权限
chown rabbitmq:rabbitmq /var/lib/rabbitmq/.erlang.cookie
chmod 400 /var/lib/rabbitmq/.erlang.cookie
# 安装错误则设置主机名字
vi /etc/hosts

127.0.0.1 <HOSTNAME>

# 重启rabbitMQ服务
rabbitmq-server restart
# 查看rabbitMQ进程
ps -ef | grep rabbitmq
# 查看rabbitMQ服务所有插件
rabbitmq-plugins list
# 添加rabbitMQ管理控制台插件
rabbitmq-plugins enable rabbitmq_management
# 登录rabbitMQ服务器后台
http://<IP_ADDR>:15672
# 创建rabbitMQ用户并授权远程访问
rabbitmqctl add_user openstack OpenStack
rabbitmqctl set_user_tags openstack administrator
rabbitmqctl set_permissions -p / openstack '.*' '.*' '.*'
# 设置开机启动rabbitMQ服务
systemctl enable rabbitmq-server.service
# 启动rabbitMQ服务
systemctl start rabbitmq-server.service
# 编辑rabbitmq-server配置文件在rabbitmq-server的第85行添加erlang的环境变量
vim /usr/lib/rabbitmq/bin/rabbitmq-server

ERLANG_HOME=/usr/local/erlang
export PATH=$PATH:$ERLANG_HOME/bin

# 设置开机启动rabbitMQ服务
systemctl enable rabbitmq-server.service
# 重新启动rabbitMQ服务
systemctl restart rabbitmq-server.service
```

## Memacached

```bash
# 安装memacached
yum install memcached python-memcached
# 编辑memcached配置文件
vi /etc/sysconfig/memcached

OPTIONS="-l127.0.0.1,::1,<CONTROLLER>"

# 开机启动memcached服务
systemctl enable memcached.service
# 启动memcached服务
systemctl start memcached.service
# 查看端口是否被监听
netstat -ntlp
```

## Keystone

* 用户与认证：用户权限与用户行为跟踪；
* 服务目录：提供一个服务目录，包括所有服务项与相关API的端点；
* Project：各个服务中的一些可以访问的资源集合；
* Role：用户在项目里面的权限；
* User：使用一个数字来代表Openstack云服务的一个人/系统/服务；
* Service：如Nova服务/Glance服务/Swift服务；
* Token：令牌，登录认证使用；
* Endponit：端点，一个服务暴露出来的服务点(public/private/admin)；

### 安装和配置

```bash
# 安装keystone
yum install openstack-keystone httpd mod_wsgi
# 连接数据库
mysql -u root -p
# 创建keystone数据库
create database keystone;
# 对keystone数据库授予权限
grant all on keystone.* to 'keystone'@'localhost' identified by '<KEYSTONE_DBPASS>';
grant all on keystone.* to 'keystone'@'%' identified by '<KEYSTONE_DBPASS>';
# 编辑keystone配置文件设置数据库访问
vim /etc/keystone/keystone.conf

[dataase]
# ...
connection = mysql+pymysql://keystone:<KEYSTONE_DBPASS>@<CONTROLLER>/keystone

[token]
# ...
provider = fernet

# 验证配置是否成功
grep '^[a-z]' /etc/keystone/keystone.conf
# 初始化身份认证服务数据库，即填充身份认证服务数据库
su -s /bin/sh -c "keystone-manage db_sync" keystone
# 验证数据库中的表是否创建成功
mysql-h <CONTROLLER> -ukeystone -p<KEYSTONE_DBPASS> -e "use keystone;show tables;"
# 初始化FernetKey
keystone-manage fernet_setup --keystone-user keystone --keystone-group keystone
keystone-manage credential_setup --keystone-user keystone --keystone-group keystone
# Bootstrap the Identity service 初始化admin用户密码
keystone-manage bootstrap --bootstrap-password <ADMIN_PASS> \
--bootstrap-admin-url http://<CONTROLLER>:5000/v3/ \
--bootstrap-internal-url http://<CONTROLLER>:5000/v3 \
--bootstrap-public-url http://<CONTROLLER>:5000/v3 \
--bootstrap-region-id RegionOne
# 编辑apache http配置文件
vim /etc/httpd/conf/httpd.conf

ServerName <CONTROLLER>:80

# 创建一个软连接映射
ln -s /usr/share/keystone/wsgi-keystone.conf /etc/httpd/conf.d/
# 查看apache http监听端口
cat /etc/httpd/conf.d/wsgi-keystone.conf
# 设置开机启动apache http服务
systemctl enable httpd.service
# 启动apache http服务
systemctl start httpd.service
# 配置admin账户
export OS_USERNAME=admin
export OS_PASSWORD=<ADMIN_PASS>
export OS_PROJECT_NAME=admin
export OS_USER_DOMAIN_NAME=Default
export OS_PROJECT_DOMAIN_NAME=Default
export OS_AUTH_URL=http://<CONTROLLER>:5000/v3
export OS_IDENTITY_API_VERSION=3
# 查看Openstack用户列表
openstack user list
# 查看Openstack项目列表
openstack peoject list
# 查看openstack权限列表
oopenstack role list
# 查看openstack服务列表
openstack service list
# 查看openstack端点列表
openstack endpoint list
# 删除openstack端点
openstack endpoint delete <ENDPONIT_ID>
```

### 创建域、项目、用户、角色

```bash
# 创建新域
openstack domain create --description "<DESCRIPTION_INFO>" <DOMAIN_NAME>
# 使用默认域创建服务项目
openstack project create --domain default --description "<DESCRIPTION_INFO>" <SERVICE_NAME>
# 创建项目，用作常规任务应使用非特权项目和用户
openstack project create --domain default --description "<DESCRIPTION_INFO>" <PROJECT_NAME>
# 创建用户
openstack user create --domain default --password-prompt <USER_NAME>
# 创建角色
openstack role create <ROLE_NAME>
# 使用角色添加用户到项目中
openstack role add -project <PROJECT_NAME> --user <USER_NAME> <ROLE_NAME>
# 撤销临时环境变量OS_AUTH_URL和OS_PASSWORD
unset OS_AUTH_URL OS_PASSWORD
# 使用admin用户请求身份验证令牌
openstack --os-auth-url http://<CONTROLLER>:5000/v3 \
--os-project-domain-name Default --os-user-domain-name Default \
--os-project-name admin --os-username admin token issue
# 使用用户请求身份验证令牌
openstack --os-auth-url http://<CONTROLLER>:5000/v3 \
--os-project-domain-name Default --os-user-domain-name Default \
--os-project-name <PROJECT_NAME> --os-username <USER_NAME> token issue
# 创建管理员环境变量脚本
vim admin-openstack.sh

export OS_PROJECT_NAME=Default
export OS_PROJECT_DOMAIN_NAME=Default
export OS_USER_DOMAIN_NAME=Default
export OS_PROJECT_NAME=admin
export OS_USERNAME=admin
export OS_PASSWORD=<USER_PASSWORD>
export OS_AUTH_URL=http://<CONTROLLER>:5000/v3
export OS_IDENTITY_API_VERSION=3
export OS_IMAGE_API_VERSION=2

# 创建用户环境变量脚本
vim <USER_NAME>-openstack.sh

export OS_PROJECT_NAME=Default
export OS_PROJECT_DOMAIN_NAME=Default
export OS_USER_DOMAIN_NAME=Default
export OS_PROJECT_NAME=<PROJECT_NAME>
export OS_USERNAME=<USER_NAME>
export OS_PASSWORD=<USER_PASSWORD>
export OS_AUTH_URL=http://<CONTROLLER>:5000/v3
export OS_IDENTITY_API_VERSION=3
export OS_IMAGE_API_VERSION=2

# 使用脚本
source <USER_NAME>-openstack.sh
openstack token issue
```

## Glance

* Glance-api(9292):接受云系统镜像的创建、删除、获取请求
* Glance-Registry(9191):云系统的镜像注册服务

```bash
# 连接数据库
mysql -u root -p
# 创建glance数据库
create database glance;
# 对glance数据库授予权限
grant all on glance.* to 'glance'@'localhost' identified by '<GLANCE_DBPASS>';
grant all on glance.* to 'glance'@'%' identified by '<GLANCE_DBPASS>';
# 创建glance用户
openstack user create --domain default --password-prompt glance
# 使用admin角色添加Glance用户到service项目中
openstack role add --project service --user glance admin
# 创建glance服务实体，一般命名为image
openstack service create --name glance --description "<DESCRIPTION_INFO>" <SERVICE_NAME>
# 创建镜像服务的API端点
openstack endpoint create --region RegionOne <SERVICE_NAME> public http://<CONTROLLER>:9292
openstack endpoint create --region RegionOne <SERVICE_NAME> internal http://<CONTROLLER>:9292
openstack endpoint create --region RegionOne <SERVICE_NAME> admin http://<CONTROLLER>:9292
# 安装glance
yum install openstack-glance
# 编辑glance-api配置文件设置数据库访问
vim /etc/glance/glance-api.conf

[database]
# ...
connection = mysql+pymysql://glance:<GLANCE_DBPASS>@<CONTROLLER>/glance

[keystone_authtoken]
# ...
www_authenticate_uri=http://<CONTROLLER>:5000
auth_url=http://<CONTROLLER>:5000
memcached_servers=<CONTROLLER>:11211
auth_type=password
project_domain_name=Default
user_domain_name=Default
project_name=service
username=glance
password=<USER_PASSWORD>

[paste_deploy]
# ...
flavor=keystone

[glance_store]
# ...
stores=file,http
default_store=file
filesystem_store_datadir=<IMAGE_DIR>

# 验证配置是否成功
grep '^[a-z]' /etc/glance/glance-api.conf
# 编辑glance-registry配置文件设置数据库访问
vim /etc/glance/glance-registry.conf

[database]
# ...
connection = mysql+pymysql://glance:<GLANCE_DBPASS>@<CONTROLLER>/glance

[keystone_authtoken]
# ...
www_authenticate_uri=http://<CONTROLLER>:5000
auth_url=http://<CONTROLLER>:5000
memcached_servers=<CONTROLLER>:11211
auth_type=password
project_domain_name=Default
user_domain_name=Default
project_name=service
username=glance
password=<USER_PASSWORD>

[paste_deploy]
# ...
flavor=keystone

# 验证配置是否成功
grep '^[a-z]' /etc/glance/glance-registry.conf
# 填充glance服务数据库
su -s /bin/sh -c "glance-manage db_sync" glance
# 验证glance服务数据库
mysql -uglance -p<GLANCE_DBPASS> -e "use glance;show tables;"
# 设置开机启动glance服务
systemctl enable openstack-glance-api.service openstack-glance-registry.service
# 启动glance服务
systemctl start openstack-glance-api.service openstack-glance-registry.service
# 获取cirrOS镜像
wget http://download.cirros-cloud.net/0.4.0/cirros-0.4.0-x86_64-disk.img
# 验证glance服务
openstack image create "cirros" --file <FILE_DIR> --disk-format qcow2 --container-format bare --public
# 查看openstack镜像列表
openstack image list
```

## Nova

* nova-api：为Nova组件对外的唯一窗口，接收和响应用户的API请求；
* nova-api-metadata：主要接收虚拟机的元数据请求，通常在带有nova-network的多主机模式下才会使用；
* nova-compute：它是一个工作进程(worker deamon)，通过hypervisor APIs来创建或者关闭虚拟机实例；
* nova-placement-api：主要追踪每个提供者的库存和使用量情况；
* nova-scheduler：处理队列中的请求，然后会根据计算节点当时的资源使用情况选择一个最合适的计算节点来运行虚拟机；
* nova-conductor：nova-compute需要获取和更新数据库中instance的信息，但nova-compute并不会直接访问数据库，而是通过nova-conductor实现数据的访问；
* nova-cert：一个Nova的证书服务，提供 x509 证书支持，用于兼容AWS；
* nova-consoleauth：负责对访问虚机控制台请亲提供Token认证；
* nova-novncproxy：提供基于Web浏览器的VNC访问；
* nova-spicehtml5proxy： 提供基于HTML5浏览器的SPICE访问；
* nova-xvpvncproxy：提供基于Java客户端的VNC访问；
* The queue：Nova通过Message Queue作为子服务的信息中转站；
* SQL database：Nova会存放一些云平台的状态数据在数据库；

### Controller

```bash
# 安装nova
yum install openstack-nova-api openstack-nova-conductor \
openstack-nova-console openstack-nova-novncproxy \
openstack-nova-scheduler openstack-nova-placement-api
# 连接数据库
mysql -u root -p
# 创建nova数据库
create database nova;
create database nova_api;
create database nova_cell0;
create database placement;
# 对nova数据库授予权限
grant all on nova.* to 'nova'@'localhost' identified by '<NOVA_DBPASS>';
grant all on nova.* to 'nova'@'%' identified by '<NOVA_DBPASS>';
# 对nova_api数据库授予权限
grant all on nova_api.* to 'nova'@'localhost' identified by '<NOVA_DBPASS>';
grant all on nova_api.* to 'nova'@'%' identified by '<NOVA_DBPASS>';
# 对nova_cell0数据库授予权限
grant all on nova_cell0.* to 'nova'@'localhost' identified by '<NOVA_DBPASS>';
grant all on nova_cell0.* to 'nova'@'%' identified by '<NOVA_DBPASS>';
# 对placement数据库授予权限
grant all on placement.* to 'placement'@'localhost' identified by '<PLACEMENT_DBPASS>';
grant all on placement.* to 'placement'@'%' identified by '<PLACEMENT_DBPASS>';
# 查看数据库验证是否创建成功
show databases;
# 创建nova用户
openstack user create --domain default --password-prompt <NOVA_PASSWORD>
# 使用admin角色添加nova用户到service服务项目中
openstack role add --project service --user nova admin
#创建placement用户
openstack user create --domain default --password-prompt <PLACEMENT_PASSWORD>
# 使用admin角色添加Placement用户到service服务项目中
openstack role add --project service --user placement admin
# 编辑nova.conf配置文件
vim /etc/nova/nova.conf

[DEFAULT]
# ...
enabled-apis=osapi_compute,metadata
transport_url=rabbit://openstack:<OPENSTACK_PASSWORD>@<CONTROLLER>
use_neutron=true
firewall_driver=nova.virt.firewall.NoopFirewallDriver

[api_database]
# ...
connection=mysql+pymysql://nova:<NOVA_DBPASS>@<CONTROLLER>/nova_api

[database]
# ...
connection=mysql+pymysql://nova:<NOVA_DBPASS>@<CONTROLLER>/nova

[placement_database]
# ...
connection=mysql+pymysql://placement:<PLACEMENT_DBPASS>@<CONTROLLER>/placement

[api]
# ...
auth_strategy=keystone

[keystone_authtoken]
# ...
auth_url=http://<CONTROLLER>:5000/v3
memcached_servers=<CONTROLLER>:11211
auth_type=password
project_domain_name=Default
user_domain_name=Default
project_name=service
username=nova
password=<NOVA_PASSWORD>

[vnc]
enabled=true
#...
server_listen=0.0.0.0
server_proxyclient_address=<CONTROLLER>

[glance]
#...
api_servers=http://CONTROLLER>:9292

[oslo_concurrency]
#...
lock_path=/var/lib/nova/tmp

[placement]
#...
region_name=RegionOne
project_domain_name=Default
project_name=service
auth_type=password
user_domain_name=Default
auth_url=http://<CONTROLLER>:5000/v3
username=placement
password=<PLACEMENT_PASSWORD>
# 验证配置文件
grep '^[a-z]' /etc/nova/nova.conf

# 编辑nova-placement-api配置文件

<Directory /usr/bin>
    <IfVersion >= 2.4>
        Require all granted
    </IfVersion>
    <IfVersion < 2.4>
        Order allow,deny
        Allow fro mall
    </IfVersion>
</Directory>

# 重启apache httpd服务
systemctl restart httpd
# 填充nova-api和placement数据库
su -s /bin/sh -c "nova-manage api_db sync" nova
# 填充cell0数据库
su -s /bin/sh -c "nova-manage cell_v2 map_cell0" nova
# 填充cell1数据库
su -s /bin/sh -c "nova-manage cell_v2 create_cell --name=cell1 --verbose" nova
# 填充nova数据库，如果弹出警告无视
su -s /bin/sh -c "nova-manage db sync" nova
# 验证nova cell0 cell1 是否正确注册
su -s /bin/sh -c "nova-manage cell_v2 list_cells" nova
# 验证数据库
mysql -unova -pnova -e "use nova; show tables;"
mysql -unova -pnova -e "use nova_api; show tables;"
mysql -unova -pnova -e "use nova_cell0; show tables;"
mysql -uplacement -pplacement -e "use placement; show tables;"
# 设置启动nova控制节点服务
systemctl start openstack-nova-api.service \
openstack-nova-scheduler.service openstack-nova-conductor.service \
openstack-nova-novncproxy.service
# 设置开机自启动nova控制节点服务
systemctl enable openstack-nova-api.service \
openstack-nova-scheduler.service openstack-nova-conductor.service \
openstack-nova-novncproxy.service
# 创建nova服务实体，一般命名为compute
openstack service create --name nova --description "<DESCRIPTION_INFO>" <SERVICE_NAME>
# 创建compute api服务端点
openstack endpoint create --region RegionOne <SERVICE_NAME> public http://<CONTROLLER>:8774/v2.1
openstack endpoint create --region RegionOne <SERVICE_NAME> internal http://<CONTROLLER>:8774/v2.1
openstack endpoint create --region RegionOne <SERVICE_NAME> admin http://<CONTROLLER>:8774/v2.1
# 创建placementapi服务实体，一般命名为placement
openstack service create --name placement --description "<DESCRIPTION_INFO>" <SERVICE_NAME>
# 创建placementapi服务端点
openstack endpoint create --regionRegionOne placement public http://<CONTROLLER>:8778
openstack endpoint create --regionRegionOne placement internal http://<CONTROLLER>:8778
openstack endpoint create --regionRegionOne placement admin http://<CONTROLLER>:8778
# 验证计算服务是否安装成功
openstack service list
openstack endpoint list
openstack host list
# 查看nova服务节点列表
nova service-list
# 寻找nova计算节点
nova-manage cell_v2 discover_hosts
```

### Node
```bash
# 设置节点时间同步
ntpdate time1.aliyun.com
ntpdate time2.aliyun.com

# 在计算节点安装计算服务软件包
yum install openstack-nova-compute
# 拉取控制节点的配置文件
scp <CONTROLLER>:/etc/nova/nova.conf /etc/nova/
# 在计算节点编辑nova配置文件，注释以下配置并添加vnc配置
vim /etc/nova/nova.conf

[api_database]
connection=mysql+pymysql://nova:<NOVA_DBPASS>@<CONTROLLER>/nova_api

[database]
connection=mysql+pymysql://nova:<NOVA_DBPASS>@<CONTROLLER>/nova

[placement_database]
connection=mysql+pymysql://placement:<PLACEMENT_DBPASS>@<CONTROLLER>/placement

[vnc]
#...
server_proxyclient_address=<COMPUTE>
novncproxy_base_url=http://<CONTROLLER>:6080/vnc_auto.html


# 验证配置是否成功
grep '^[a-z]' /etc/nova/nova.conf
# 验证计算节点是否支持虚拟机的硬件加速，返回值大于1支持，返回0则需要配置libvirt
egrep -c '(vmx|svm)' /proc/cpuinfo
# 如果不支持虚拟机的硬件加速则在nova配置文件中修改libvirt使用qemu而不是kvm
vim /etc/nova/nova.conf

[libvirt]
# ...
virt_type=qemu

# 设置开机自启动计算服务及其依赖
systemctl enable libvirtd.service openstack-nova-compute.service
# 设置启动计算服务及其依赖
systemctl start libvirtd.service openstack-nova-compute.service
# 在控制节点列出计算服务组件以验证每个进程的成功启动和注册
openstack compute service list
# 在控制节点列出认证服务keystone中的API端点以验证与identity服务的连接
openstack catalog list
# 在控制节点列出镜像服务glance中的图像以验证与Image服务的连接
openstack image list
# 在控制节点检查单元格和放置api是否成功运行
nova-status upgrade check
# 在控制节点启动控制台远程连接认证服务器，不启动则无法进行vnc远程登录
systemctl enable openstack-nova-consoleauth
systemctl start openstack-nova-consoleauth
# 在控制节点列出服务组件以验证每个进程的成功启动和注册
openstack compute service list
```

## Neutron

### Controller

```bash
# 连接数据库
mysql -uroot -p
# 创建neuton数据库
create database neutron;
# 对neutron数据库及服务授予权限
grant all on neutron.* to 'neutron'@'localhost' identified by '<NEUTRON_DBPASS>';
grant all on neutron.* to 'neutron'@'%' identified by '<NEUTRON_DBPASS>';
# 使用admin凭证
source admin-openstack.sh
# 创建neutron用户
openstack user create --domain default --password-prompt <NEUTRON_PASSWORD>
# 验证创建用户是否成功
openstack user list
# 使用admin角色添加neutron用户到service服务项目中
openstack role add --project service --user neutron admin
# 安装neutron网络组件
yum install openstack-neutron openstack-neutron-ml2  openstack-neutron-linuxbridge ebtables
# 编辑neutron配置文件
vi /etc/neutron/neutron.conf

[database]
# ...
connection = mysql+pymysql://neutron:<NEUTRON_DBPASS>@<CONTROLLER>/neutron

[DEFAULT]
# ...
core_plugin = ml2
service_plugins =
transport_url = rabbit://openstack:<OPENSTACK_PASSWORD>@<CONTROLLER>
notify_nova_on_port_status_changes = true
notify_nova_on_port_data_changes = true
auth_strategy = keystone

[nova]
# ...
auth_url = http://<CONTROLLER>:5000
auth_type = password
project_domain_name = default
user_domain_name = default
region_name = RegionOne
project_name = service
username = nova
password = <NOVA_PASSWORD>

[oslo_concurrency]
# ...
lock_path = /var/lib/neutron/tmp

[keystone_authtoken]
# ...
www_authenticate_uri = http://<CONTROLLER>:5000
auth_url = http://<CONTROLLER>:5000
memcached_servers = <CONTROLLER>:11211
auth_type = password
project_domain_name = default
user_domain_name = default
project_name = service
username = neutron
password = <NEUTRON_PASSWORD>

# 验证配置是否成功
grep '^[a-z]' /etc/neutron/neutron.conf
# 编辑ml2配置文件
vi /etc/neutron/plugins/ml2/ml2_conf.ini

[ml2]
# ...
type_drivers = flat,vlan,gre,vxlan,geneve
tenant_network_types =
mechanism_drivers = linuxbridge,openvswitch
extension_drivers = port_security

[ml2_type_flat]
# ...
flat_network = public

[securitygroup]
# ...
enable_ipset = true

# 验证配置是否成功
grep '^[a-z]' /etc/neutron/plugins/ml2/ml2_conf.ini
# 编辑linux网桥代理配置文件
vi /etc/neutron/plugins/ml2/linuxbridge_agent.ini

[linux_bridge]
# ...
physical_interface_mappings = public:eth0

[vxlan]
# ...
enable_vxlan = false

[securitygroup]
# ...
enable_security_group = true
firewall_driver = neutron.agent.linux.iptables_firewall.IptablesFirewallDriver

# 验证配置是否成功
grep '^[a-z]' /etc/neutron/plugins/ml2/linuxbridge_agent.ini
# 验证以下所有sysctl值设置为1,以确保linux内核支持网桥过滤器
modprobe br_netfilter
ls /proc/sys/net/bridge
# 编辑sysctl配置文件
vi /etc/sysctl.conf

net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1

# 更新sysctl配置文件执行生效
sysctl -p
# 编辑dhcp配置文件
vi /etc/neutron/dhcp_agent.ini

[DEFAULT]
# ...
interface_driver = linuxbridge
dhcp_driver = neutron.agent.linux.dhcp.Dnsmasq
enable_isolated_metadata = true

# 验证配置是否成功
grep '^[a-z]' /etc/neutron/dhcp_agent.ini
# 编辑metadata配置文件,密码设置为huike
vi /etc/neutron/metadata_agent.ini

[DEFAULT]
# ...
nova_metadata_host = <CONTROLLER>
metadata_proxy_shared_secret = <PROXY_PASSWORD>

# 验证配置是否成功
grep '^[a-z]' /etc/neutron/metadata_agent.ini
# 编辑nova配置文件,密码为huike
vi /etc/nova/nova.conf

[neutron]
# ...
url = http://<CONTROLLER>:9696
auth_url = http://<CONTROLLER>:5000
auth_type = password
project_domain_name = default
user_domain_name = default
region_name = RegionOne
project_name = service
username = neutron
password = <NEUTRON_PASSWORD>
service_metadata_proxy = true
metadata_proxy_shared_secret = <PROXY_PASSWORD>

# 创建一个软链接使ml2配置文件链接至/etc/neutron/plugin.ini
ln -s /etc/neutron/plugins/ml2/ml2_conf.ini /etc/neutron/plugin.ini
# 填充neutron数据库
su -s /bin/sh -c "neutron-db-manage --config-file /etc/neutron/neutron.conf --config-file /etc/neutron/plugins/ml2/ml2_conf.ini upgrade head" neutron
# 重新启动nova-api服务
systemctl restart openstack-nova-api.service
# 设置开机自启动neutron服务
systemctl enable neutron-server.service neutron-linuxbridge-agent.service neutron-dhcp-agent.service neutron-metadata-agent.service
# 启动neutron服务
systemctl start neutron-server.service neutron-linuxbridge-agent.service neutron-dhcp-agent.service neutron-metadata-agent.service
# 创建neutron服务实体，服务名一般为network
openstack service create --name neutron --description "<DESCRIPTION_INFO>" <Service>
# 创建网络服务API端点
openstack endpoint create --region RegionOne network public http://<CONTROLLER>:9696
openstack endpoint create --region RegionOne network internal http://<CONTROLLER>:9696
openstack endpoint create --region RegionOne network admin http://<CONTROLLER>:9696
# 验证neutron服务是否安装成功
openstack network agent list
```

### Node

```bash
# 安装neutron组件
yum install openstack-neutron-linuxbridge ebtables ipset

# 拉取控制节点的neutron配置文件
scp <CONTROLLER>:/etc/neutron/neutron.conf /etc/neutron/neutron.conf
# 编辑neutron配置文件，注释以下配置
vi /etc/neutron/neutron.conf
[database]
# ...
connection = mysql+pymysql://neutron:<NEUTRON_DBPASS>@<CONTROLLER>/neutron

[DEFAULT]
# ...
core_plugin = ml2
service_plugins =
notify_nova_on_port_status_changes = true
notify_nova_on_port_data_changes = true

[nova]
# ...
auth_url = http://<CONTROLLER>:5000
auth_type = password
project_domain_name = default
user_domain_name = default
region_name = RegionOne
project_name = service
username = nova
password = <NOVA_PASSWORD>

# 编辑linuxbridge配置文件
vi /etc/neutron/plugins/ml2/linuxbridge_agent.ini

[linux_bridge]
# ...
physical_interface_mappings = public:eth0

[vxlan]
# ...
enable_vxlan = false

[securitygroup]
# ...
enable_security_group = true
firewall_driver = neutron.agent.linux.iptables_firewall.IptablesFirewallDriver

# 验证配置是否成功
grep '^[a-z]' /etc/neutron/plugins/ml2/linuxbridge_agent.ini
# 验证linux内核是否至此网桥过滤器
modprobe br_netfilter
ls /proc/sys/net/bridge
# 编辑sysctl配置文件
vi /etc/sysctl.conf

net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1

# 更新syyctl配置文件执行生效
sysctl -p
# 编辑nova配置文件来使用网络服务
vi /etc/nova/nova.conf

[neutron]
#...
url = http://<CONTROLLER>:9696
auth_url = http://<CONTROLLER>:5000
auth_type = password
project_domain_name = default
user_domain_name = default
region_name = RegionOne
project_name = service
username = neutron
password = <NEUTRON_PASSWORD>

# 重新启动nova服务
systemctl restart openstack-nova-compute.service
# 设置开机自启动linuxbridge服务
systemctl enable neutron-linuxbridge-agent.service
# 启动linuxbridge服务
systemctl start neutron-linuxbridge-agent.service
# 在controller节点验证操作
openstack network agent list
```

## 创建虚拟机实例

```bash
# 导入admin凭证
source admin-openstack.sh
# 创建外部网络
openstack network create --share --external --provider-physical-network public --provider-network-type flat public
# 验证网络是否创建成功
neutron net-list
# 创建子网
openstack subnet create --network public --allocation-pool start=192.168.30.100,end=192.168.30.130 --dns-nameserver 192.168.30.2 --gateway 192.168.30.2 --subnet-range 192.168.30.0/24 public-subnet
# 验证子网是否创建成功
neutron subnet-list
# 创建一个m1.nano类型的云主机
openstack flavor create --id 0 --vcpus 1 --ram 64 --disk 1 m1.nano
# 查看虚拟机列表
openstack flavor list
# 导入myuser项目凭证
source myuser-openstack.sh
# 生成密钥文件
ssh-keygen -q -N ""
# 创建一个名为mykey的openstack秘钥,即把家目录是里面的公钥上传到openstack上
openstack keypair create --public-key ~/.ssh/id_rsa.pub mykey
# 查看密钥文件
openstack keypair list
# 添加安全组规则允许ICMP
openstack security group rule create --proto icmp default
# 添加安全组规则允许SSH
openstack security group rule create --proto tcp --dst-port 22 default
# 导入myuser项目凭证
source myuser-openstack.sh
# 查看openstack虚拟机列表
openstack flavor list
# 查看openstack镜像列表
openstack image list
# 查看openstack网络列表
openstack network list
# 查看openstack安全组列表
openstack security group list
# 启动虚拟机实例,PROVIDER_NET_ID为public网络id如果只有一个网络则可以忽略--nic选项
openstack server create --flavor m1.nano --image cirros --nic net-id=<PROVIDER_NET_ID> --security-group default --key-name mykey provider-instance
# 查看openstack服务列表
openstack server list
# 使用虚拟控制台访问实例
openstack console url show provider-instance
# 如果启动失败则编辑计算节点的nova配置文件
vi /etc/nova/nova.conf

[libvirt]
virt_type=qemu

# 在计算节点重启nova服务
systemctl restart libvirtd.service openstack-nova-compute.service
# 重启openstack虚拟机
openstack server reboot <INSTANCE_NAME>
# 删除openstack虚拟机
openstack server delete <INSTANCE_NAME>
# 验证是否能远程访问实例
ping -c <IP_ADDR>
# 验证是否能SSH远程访问实例
ssh cirros@<IP_ADDR>
```

## Cinder

```bash
# 连接数据库
mysql -uroot -p
# 创建cinder数据库
create database cinder;
# 对cinder数据库及服务授予权限
grant all on cinder.* to 'cinder'@'localhost' identified by '<CINDER_DBPASS>';
grant all on cinder.* to 'cinder'@'%' identified by '<CINDER_DBPASS>';
# 获得admin凭证
source admin-openstack.sh
# 创建cinder用户
openstack user create --domain default --password-prompt <CINDER_PASSWORD>
# 验证用户是否创建成功
openstack user list
# 使用admin角色将cinder加入service项目
openstack role add --project service --user cinder admin
# 创建cinderv2服务实体 一般命名为volume2
openstack service create --name cinderv2 --description "<DESCRIPTION_INFO>" <SERVICE_NAME>
# 创建cinderv3服务实体 一般命名为volume3
openstack service create --name cinderv3 --description "<DESCRIPTION_INFO>" <SERVICE_NAME>
# 创建cinder服务的api端口
openstack endpoint create --regio RegionOne volumev2 public http://<CONTROLLER>:8776/v2/%\(project_id\)s
openstack endpoint create --regio RegionOne volumev2 internal http://<CONTROLLER>:8776/v2/%\(project_id\)s
openstack endpoint create --regio RegionOne volumev2 admin http://<CONTROLLER>:8776/v2/%\(project_id\)s
openstack endpoint create --regio RegionOne volumev3 public http://<CONTROLLER>:8776/v3/%\(project_id\)s
openstack endpoint create --regio RegionOne volumev3 internal http://<CONTROLLER>:8776/v3/%\(project_id\)s
openstack endpoint create --regio RegionOne volumev3 admin http://<CONTROLLER>:8776/v3/%\(project_id\)s
# 获得admin凭证
source admin-openstack.sh
# 列出openstack服务列表
openstack service list
# 安装openstack cinder软件包
yum install openstack-cinder python-keystone
# 编辑cinder配置文件
vi /etc/cinder/cinder.conf

[database]
# ...
connection = mysql+pymysql://cinder:<CINDER_DBPASS>@<CONTROLLER>/cinder

[DEFAULT]
# ...
transport_url = rabbit://openstack:<OPENSTACK_PASSWORD>@<CONTROLLER>
auth_strategy = keystone

[keystone_authtoken]
# ...
www_authenticate_uri=http://<CONTROLLER>:5000
auth_url=http://<CONTROLLER>:5000
memcached_servers=<CONTROLLER>:11211
auth_type=password
project_domain_id=default
user_domain_id=default
project_name=service
username=cinder
password=<CINDER_PASSWORD>

[oslo_concurrency]
# ...
lock_path = /var/lib/cinder/tmp

# 填充cinder数据库 //有提示不用处理
su -s /bin/sh -c "cinder-manage db sync" cinder
# 验证cinder数据库
mysql -ucinder -pcinder -e"use cinder;show tables;"
# 编辑nova配置文件
vi /etc/nova/nova.conf

[cinder]
os_region_name = RegionOne

# 重新启动nova.api服务
systemctl restart openstack-nova-api.service
# 启动cinder服务
systemctl start openstack-cinder-api.service openstack-cinder-scheduler.service
# 设置开机自启动cinder配套服务
systemctl enable openstack-cinder-api.service openstack-cinder-scheduler.service
# 安装lvm工具包
yum install lvm2 device-mapper-persistent-data
# 启动lvm的metadata服务
systemctl start lvm2-lvmetad.service
# 设置开机自启动lvm的metadata服务
systemctl enable lvm2-lvmetad.service
# 添加一块硬盘,并验证
fdisk -l
# 创建lvm物理卷/dev/sdb
pvcreate /dev/sdb
# 创建lvm卷组cinder-volumes
vgcreate cinder-volumes /dev/sdb
# 查看lvm卷组信息
vgdisplay
# 编辑lvm配置文件，添加过滤器接受/dev/sdb并拒绝其他所有设备
vi /etc/lvm/lvm.conf

devices{
    ...
    filter=["a/sdb/","r/.*/"]
    filter=["a/sda/","a/sdb/","r/.*/"]

# 安装配置组件包
yum install openstack-cinder targetcli python-keystone
# 编辑cinder配置文件，如果lvm部分不存在则创建它
vi /etc/cinder/cinder.conf

[lvm]
volume_driver=cinder.volume.drivers.lvm.LVMVolumeDriver
volume_group=cinder-volumes
iscsi_protocol=iscsi
iscsi_helper=lioadm
volume_backend_name=ISCSI_Storage

[DEFAULT]
# ...
glance_api_servers=http://<CONTROLLER>:9292
enabled_backends=lvm
my_ip=<CONTROLLER>

# 启动cinder依赖服务
systemctl start openstack-cinder-volume.service target.service
# 设置开机自启动cinder依赖服务
systemctl enable openstack-cinder-volume.service target.service
# 获取admin凭证
source admin-openstack.sh
# 查看openstack volume服务已验证是否配置成功
openstack volume service list
```

## Horizon

```bash
# 安装dashboard软件包
yum install openstack-dashboard
# 编辑dashboard配置文件
vim /etc/openstack-dashboard/local_settings

ALLOWED_HOSTS = ['*','localhost']
OPENSTACK_API_VERSIONS = {
    "identity":3,
    "image":2,
    "volume":2,
    "compute":2,
}
OPENSTACK_KEYSTONE_MULTIDOMAIN_SUPPORT = True
OPENSTACK_KEYSTONE_DEFAULT_DOMAIN = "Default"
OPENSTACK_HOST = "<CONTROLLER>"
OPENSTACK_KEYSTONE_DEFAULT_ROLE = "myrole"
OPENSTACK_NEUTRON_NETWORK = {
    'enable_router':False,
    'enable_quotas':False,
    'enable_ipv6':False,
    'enable_distributed_router':False,
    'enable_ha_router':False,
    'enable_fip_topology_check':False,
}
TIME_ZONE = "Asia/Shanghai"

# 编辑dashboard配置文件
vi /etc/httpd/conf.d/openstack-dashboard.conf

WSGIApplicationGroup %{GLOBAL}

# 重新启动httpd服务器和memcached存储服务
systemctl restart httpd.service memcached.service
# 使用浏览器登入dashboard界面 域：default 用户名：myuser 密码：myuser
http://<CONTROLLER>/dashboard
#
```