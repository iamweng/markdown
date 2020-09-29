# Hadoop

## 安装之前准备
```bash
# 关闭 firewalld
systemctl stop firewalld
# 禁止 firewalld 开机自启动
systemctl disable firewalld
# 修改 hostname 文件
vi /etc/hostname
# 配置 hosts 文件
vi /et/hosts
# 同步 hosts 文件
scp /etc/hosts <IPADDR>:/etc/hosts
# 生成公钥和密钥
ssh-keygen -t rsa
# 追加公钥到 authorized_keys
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
# 赋予 authorized_keys 600 权限
chmod 600 ~/.ssh/authorized_keys
# 分发 authorized_keys
scp ~/.ssh/authorized_keys root@<IPADDR>:~/.ssh/
# 安装时间同步软件
yum -y install ntp ntpdate
# 进行时间同步
ntpdate cn.pool.ntp.org
# 将时间写入硬件锁定
hwclock --systohc
```

## 安装 JDK
```bash
# 创建 JDK 目录
cd /opt; mkdir software
# 解压 JDK 压缩包
tar -zxvf jdk-8u241-linux-x64.tar.gz
# 配置 /etc/profile 文件
vi /etc/profile

export JAVA_HOME=/opt/software/jdk1.8.0_241
export JRE_HOME=/opt/software/jdk1.8.0_241/jre
CLASSPATH=.:$JRE_HOME/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin

# 刷新环境变量
source /etc/profile
# 验证 JDK 是否安装成功
java -version
```

## 安装 Hadoop
```bash
# 解压 Hadoop 压缩包
tar -zxvf hadoop-2.6.5.tar.gz
# 配置 /etc/profile 文件
vi /etc/profile

export HADOOP_HOME=/opt/software/hadoop-2.6.5
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```

### 搭建本地模式 Hadoop
```bash
# 在 hadoop 目录下创建 input 目录
cd /opt/software/hadoop-2.6.5; mkdir input
# 复制 hadoop 的 xml 配置文件
cp etc/hadoop/*.xml input
# 执行 MapReduce 程序
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.5.jar grep input output 'dfs[a-z.]+'
# 验证结果
cat output/

# 在 hadoop 目录下创建 wcinput 目录
mkdir wcinput
# 在 wcinput 目录中创建 wcinput 文件
vi wcinput/wcinput
# 执行 MapReduce 程序
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.5.jar wordcount wcinput/wcinput wcoutput
# 验证结果
cat wcoutput/*
```

### 配置完全分布式 Hadoop 集群
```bash
# 创建必要文件夹
cd /opt/software/hadoop-2.6.5
mkdir tmp; mkdir logs; mkdir hdfs; mkdir hdfs/name; mkdir hdfs/data
# 配置 hadoop-2.6.5/etc/hadoop/hadoop-env.sh 文件
vi /opt/software/hadoop-2.6.5/etc/hadoop-env.sh

export JAVA_HOME=/opt/software/jdk1.8.0_241

# 配置 hadoop-2.6.5/etc/hadoop/yarn-env.sh 文件
vi /opt/software/hadoop-2.6.5/etc/hadoop/yarn-env.sh

export JAVA_HOME=/opt/software/jdk1.8.0_241

# 配置 hadoop-2.6.5/etc/hadoop/slaves 文件
vi /opt/software/hadoop-2.6.5/etc/slaves

slave01
slave02

# 配置 hadoop-2.6.5/etc/hadoop/core-site.xml 文件
vi /opt/software/hadoop-2.6.5/etc/core-site.xml

<configuration>
    <property>
        # 定义 HadoopMaster 的 URI 和端口
        <name>fs.defaultFS</name>
        <value>hdfs://master:9000</value>
    </property>

    <property>
        # Hadoop 中的临时存储目录
        <name>hadoop.tmp.dir</name>
        <value>file:/opt/software/hadoop-2.6.5/tmp</value>
    </property>

    <property>
        # 用作序列化文件处理时读写buffer的大小
        <name>io.file.buffer.size</name>
        <value>131702</value>
    </property>
</configuration>

# 配置 hadoop-2.6.5/etc/hadoop/hdfs-site.xml 文件
vi /opt/software/hadoop-2.6.5/etc/hadoop/hdfs-site.xml

<configuration>
    <property>
        # namenode 节点数据存储目录
        <name>dfs.namenode.name.dir</name>
        <value>file:/opt/software/hadoop-2.6.5/hdfs/name</value>
    </property>

    <property>
        # datanode节点数据存储目录
        <name>dfs.datanode.data.dir</name>
        <value>file:/opt/software/hadoop-2.6.5/hdfs/data</value>
    </property>

    <property>
        # 指定 datanode 存储 block 的副本数量
        <name>dfs.replication</name>
        <value>2</value>
    </property>

    <property>
        # 指定 master 的 http 地址
        <name>dfs.namenode.secondary.http-address</name>
        <value>master:50090</value>
    </property>

    <property>
        # 指定 master 的 https 地址
        <name>dfs.namenode.secondary.https-address</name>
        <value>master:50091</value>
    </property>

    <property>
        # 设置是否通过 web 访问 hdfs 上的文件信息
        <name>dfs.webhdfs.enabled</name>
        <value>true</value>
    </property>
</configuration>

# 配置 hadoop-2.6.5/etc/hadoop/yarn-site.xml 文件
vi /opt/software/hadoop-2.6.5/etc/yarn-site.xml

<configuration>
    <property>
        # ResourceManager hostname
        <name>yarn.resourcemanager.hostname</name>
        <value>master</value>
    </property>
    <property>
        # NodeManager上运行的附属服务
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.auxservice.mapreduce.shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
    <property>
        # ResourceManager 对客户端暴露的访问地址
        <name>yarn.resourcemanager.address</name>
        <value>master:8032</value>
    </property>
    <property>
        # ResourceManager 对 ApplicationMaster 暴露的访问地址
        <name>yarn.resourcemanager.scheduler.address</name>
        <value>master:8030</value>
    </property>
    <property>
        # ResourceManager 对 NodeManager 暴露的访问地址
        <name>yarn.resourcemanager.resourcetracker.address</name>
        <value>master:8031</value>
    </property>
    <property>
        # ResourceManager 对管理员暴露的访问地址
        <name>yarn.resourcemanager.admin.address</name>
        <value>master:8033</value>
    </property>
    <property>
        # 用户查看集群信息的访问地址
        <name>yarn.resourcemanager.webapp.address</name>
        <value>master:8088</value>
    </property>
    <property>
        # NodeManager 可用物理内存
        <name>yarn.nodemanager.resource.memory-mb</name>
        <value>2048</value>
    </property>
    <property>
        # Scheduler 可用物理内存
        <name>yarn.scheduler.minimum-allocation-mb</name>
        <value>1024</value>
    </property>
</configuration>

# 配置 mapred-site.xml
cp mapred-site.xml.template mapred-site.xml
vi mapred-site.xml

<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.jobhistory.address</name>
        <value>master:10020</value>
    </property>
    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>master:19888</value>
    </property>
</configuration>

# 将 master 中的配置好的文件传递给 slave01 slave02
scp -r /opt/software/ root@slave01:/opt/
scp -r /opt/software/ root@slave02:/opt/
scp /etc/profile root@slave01:/etc/
scp /etc/profile root@slave02:/etc/
# 刷新 slave01 和 slave02 中的环境变量及验证
source /etc/profile
java -version
hadoop version
# 格式化 Hadoop
cd /opt/software/hadoop-2.6.5/bin/; ./hdfs namenode -format
```

### 启动 Hadoop
```bash
# 启动 hdfs 服务
cd /opt/software/hadoop-2.6.5/sbin/; ./start-dfs.sh
# 启动 yarn 服务
cd /opt/software/hadoop-2.6.5/sbin/; ./start-yarn.sh
# 查看当前进程
jps
# 启动 hdfs 和 yarn 服务
./start-all.sh
# 停止 hdfs 和 yarn 服务
./stop-all.sh
# 停止 hdfs 服务
./stop-dfs.sh
# 停止 yarn 服务
./stop-yarn.sh
```

### 常用命令
```bash
# 向 HDFS 中上传文件
hdfs dfs -put
# 向 HDFS 中下载文件
hdfs dfs -get
# 列出 HDFS 中目录
hdfs dfs -ls /
# 查看所有 job
hadoop job -list
# 删除指定的 job
hadoop job -kill <JobID>
```

# Hive

## 安装之前准备
```bash
# 关闭 firewalld 服务
systemctl stop firewalld
# 设置 firewalld 服务开机不启动
systemctl disable firewalld
# 查询已经安装的 mariadb 软件包
rpm -qa | grep mariadb
# 如果 mariadb 已经安装则卸载
rpm -e --nodeps mariadb-libs-5.5.65-1.el7.x86_64
```

## 安装 mysql 数据库
```bash
# 移动至 mysql 软件包目录中
cd /opt/software/mysql-5.7.31
# 安装 mysql 所需依赖
yum install perl
# 安装 mysql 各组件
rpm -ivh mysql-community-libs*
rpm -ivh mysql-community-common*
rpm -ivh mysql-community-client*
rpm -ivh mysql-community-server*
# 修改 mysql 配置文件
vi /etc/my.cnf

symbolic-links=0
# 设置 innodb 为默认的存储引擎
default-storage-engine=innodb
# 设置每个表的数据单独保存
innodb_file_per_table
# 设置支持中文编码字符集
collation-server=utf8_general_ci
# 将使用的字符编码设定为 utf8
init-connect='SET NAMES utf8'
# 将 MySQL 服务器字符集设定为 utf8
character-set-server=utf8

# 启动 mysql 数据库
systemctl start mysqld
# 验证 mysql 状态
systemctl status mysqld
# 查询 mysql 数据库密码
cat /var/log/mysqld.log | grep password
# 执行 mysql 数据库初始化 密码设置为Password123$
mysql_secure_installation
# 连接本地 mysql 数据库
mysql -uroot -p
# 添加 root 用户本地访问授权
grant all privileges on *.* to root@'localhost' identified by 'Password123$';
# 添加 root 用户远程访问授权
grant all privileges on *.* to root@'%' identified by 'Password123$';
# 刷新授权
flush privileges;
# 验证授权信息
select user,host from mysql.user where user='root';
# 退出 mysql 数据库
exit;
```

## 安装以及配置 hive 软件包
```bash
# 解压 hive 软件包
tar -zxvf /opt/software/apache-hive-2.0.0-bin.tar.gz -C /usr/local/src
# 修改 hive 目录归属用户和用户组为 hadoop
chown -R hadoop:hadoop /usr/local/src/apache-hive-2.0.0-bin
# 设置 hive 环境变量
vi /etc/profile

export HIVE_HOME=/usr/local/src/apache-hive-2.0.0-bin
export PATH=$PATH:/$HIVE_HOME/bin

# 刷新环境变量
source /etc/profile
# 重命名 hive 配置文件
cp /usr/local/src/apache-hive-2.0.0-bin/conf/hive-default.xml.template /usr/local/src/apache-hive-2.0.0-bin/conf/hive-site.xml
# 修改 hive 配置文件
vi /usr/local/src/apache-hive-2.0.0-bin/conf/hive-site.xml

<name>javax.jdo.option.ConnectionURL</name>
<value>jdbc:mysql://master:3306/hive?createDatabaseIfNotExist=true&amp;useSSL=false</value>
<description>JDBC connect string for a JDBC metastore</description>

<property>
<name>javax.jdo.option.ConnectionPassword</name>
<value>Password123$</value>
<description>password to use against s database</description>
</property>

<property>
<name>hive.metastore.schema.verification</name>
<value>false</value>
<description>
Enforce metastore schema version consistency.
True: Verify that version information stored in is compatible with one from Hive jars. Also disable automatic False: Warn if the version information stored in metastore does not match with one from in Hive jars.
</description>
</property>

<property>
<name>javax.jdo.option.ConnectionDriverName</name>
<value>com.mysql.jdbc.Driver</value>
<description>Driver class name for a JDBC metastore</description>
</property>

<property>
<name>javax.jdo.option.ConnectionUserName</name>
<value>root</value>
<description>Username to use against metastore database</description>
</property>

<name>hive.querylog.location</name>
<value>/usr/local/src/apache-hive-2.0.0-bin/tmp</value>
<description>Location of Hive run time structured log file</description>

<name>hive.exec.local.scratchdir</name>
<value>/usr/local/src/apache-hive-2.0.0-bin/tmp</value>

<name>hive.downloaded.resources.dir</name>
<value>/usr/local/src/apache-hive-2.0.0-bin/tmp/resources</value>

<name>hive.server2.logging.operation.log.location</name>
<value>/usr/local/src/apache-hive-2.0.0-bin/tmp/operation_logs</value>

# 创建 hive 目录下的 tmp目录
mkdir /usr/local/src/apache-hive-2.0.0-bin/tmp
# 拷贝 mysql 驱动包
cp /opt/software/mysql-connector-java-5.1.49.jar /usr/local/src/apache-hive-2.0.0-bin/lib/
# 重新启动 hadoop 集群
stop-all.sh;start-all.sh
# 初始化 mysql 数据库
schematool -initSchema -dbType mysql
# 启动 hive
hive
```

# zookeeper

## 安装 zookeeper 软件包

### master
```bash
# 解压 zookeeper 软件包
tar -zxvf /opt/software/zookeeper-3.4.8.tar.gz -C /usr/local/src
# 创建 zookeeper 目录下的 data 和 logs 目录
cd /usr/local/src/zookeeper-3.4.8; mkdir data; mkdir logs
# 创建 myid 文件并写入标识标号
echo 1 > /usr/local/src/zookeeper-3.4.8/data/myid
# 拷贝 zookeeper 默认配置文件
cp /usr/local/src/zookeeper-3.4.8/conf/zoo_sample.cfg /usr/local/src/zookeeper-3.4.8/conf/zoo.cfg
# 修改 zookeeper 配置文件
vi /usr/local/src/zookeeper-3.4.8/conf/zoo.cfg

dataDir=/usr/local/src/zookeeper-3.4.8/data

#autopurge.purgeInterval=1
server.1=master:2888:3888
server.2=slave01:2888:3888
server.3=slave02:2888:3888

# 修改 zookeeper 目录归属用户为 hadoop
chown -R hadoop:hadoop /usr/local/src/zookeeper-3.4.8
# 复制 master 节点 zookeeper 目录到 slave 节点
scp -r /usr/local/src/zookeeper-3.4.8 slave01:/usr/local/src/;scp -r /usr/local/src/zookeeper-3.4.8 slave02:/usr/local/src/
# 追加 /etc/profile 文件
vi /etc/profile

export ZOOKEEPER_HOME=/usr/local/src/zookeeper-3.4.8
export PATH=$PATH:$ZOOKEEPER_HOME/bin
# 刷新环境变量
source /etc/profile
```

### slave
```bash
# 修改 zookeeper 目录的归属用户为 hadoop
chown -R hadoop:hadoop /usr/local/src/zookeeper-3.4.8
# 修改 myid 文件
echo ? > /usr/local/src/zookeeper-3.4.8/data/myid
# 追加 /etc/profile 文件
vi /etc/profile

export ZOOKEEPER_HOME=/usr/local/src/zookeeper-3.4.8
export PATH=$PATH:$ZOOKEEPER_HOME/bin
# 刷新环境变量
source /etc/profile
```

### all
```bash
# 切换到 hadoop 用户
su - hadoop
# 启动 zookeeper 服务
zkServer.sh start
# 查看 zookeeper 服务状态
zkServer.sh status
```

# hbase

## 安装以及配置 habse 软件包目

### master
```bash
# 解压 hbase 软件包
tar -zxvf /opt/software/hbase-1.2.1-bin.tar.gz -C /usr/local/src/
# 追加 /etc/profile 文件
vi /etc/profile

export HBASE_HOME=/usr/local/src/hbase-1.2.1
export PATH=$HBASE_HOME/bin:$PATH

# 刷新环境变量
source /etc/profile
# 修改 hbase-env.sh 文件
vi /usr/local/src/hbase-1.2.1-bin/conf/hbase-env.sh

export JAVA_HOME=/opt/software/jdk1.8.0_241
export HBASE_MANAGES_ZK=false
export HBASE_CLASSPATH=/opt/software/hadoop-2.6.5/etc/hadoop/

# 修改 hbase-site.xml 文件
vi /usr/local/src/hbase-1.2.1/conf/hbase-site.xml

# 使用 9000 端口
<property>
  <name>hbase.rootdir</name>
  <value>hdfs://master:9000/hbase</value>
</property>

# 使用 master 节点 60010 端口
<property>
  <name>hbase.master.info.port</name>
  <value>60010</value>
</property>

# 使用 master 节点 2181 端口
<property>
  <name>hbase.zookeeper.property.clientPort</name>
  <value>2181</value>
</property>

# ZooKeeper 超时时间
<property>
  <name>zookeeper.session.timeout</name>
  <value>120000</value>
</property>

# ZooKeeper 管理节点
<property>
  <name>hbase.zookeeper.quorum</name>
  <value>master,slave01,slave02</value>
</property>

# HBase 临时文件路径
<property>
  <name>hbase.tmp.dir</name>
  <value>/usr/local/src/hbase-1.2.1/tmp</value>
</property>

# 使用分布式 HBase
<property>
  <name>hbase.cluster.distributed</name>
  <value>true</value>
</property>

# 修改 regionservers 文件
vi /usr/local/src/hbase-1.2.1/conf/regionservers

slave01
slave02

# 创建 hbase.tmp.dir 目录
mkdir /usr/local/src/hbase-1.2.1/tmp
# 将 hbase 安装目录分发给 slave01 slave02
scp -r /usr/local/src/hbase-1.2.1/ root@slave01:/usr/local/src/;scp -r /usr/local/src/hbase-1.2.1/ root@slave02:/usr/local/src/
# 修改 hadoop 目录所属用户
chown -R hadoop:hadoop /usr/local/src/hbase-1.2.1/
# 切换用户为 hadoop
su - hadoop
# 刷新环境变量
source /etc/profile
# 启动 hadoop
start-all.sh
# 启动 zookeeper
zkServer.sh start
# 启动 hbase
start-hbase.sh
# 访问 master:60010 验证服务
http://192.168.30.130:60010
# 关闭 hbase
stop-hbase.sh
```

### slaves
```bash
# 追加 /etc/profile 文件
vi /etc/profile

export HBASE_HOME=/usr/local/hbase-1.2.1
export PATH=$HBASE_HOME/bin:$PATH
# 刷新环境变量
source /etc/profile
# 修改 hadoop 目录所属用户
chown -R hadoop:hadoop /usr/local/src/hbase-1.2.1/
# 切换用户为 hadoop
su - hadoop
# 刷新环境变量
source /etc/profile
```

## 常用命令
```bash
# 进入 hbase 命令行
hbase shell
# 查看数据库状态
satatus
# 查看数据库版本
version
# 查看数据表
list
# 创建表 scores 两个字段 grade course
create 'scores','grade','course'
```

# sqoop

## 安装以及配置 sqoop
```bash
# 解压 sqoop 软件包
tar -zxvf /opt/software/sqoop-1.4.7.bin_hadoop-2.6.0.tar.gz -C /usr/local/src
# 修改 sqoop 安装目录所属用户
cd /usr/local/src; chown -R hadoop:hadoop sqoop-1.4.7.bin__hadoop-2.6.0
# 拷贝 sqoop-env 模板文件
cd /usr/local/src/sqoop-1.4.7.bin__hadoop-2.6.0/conf/;cp sqoop-env-template.sh sqoop-env.sh
# 修改 sqoop-env 配置文件
vi sqoop-env.sh

export HADOOP_COMMON_HOME=/opt/software/hadoop-2.6.5
export HADOOP_MAPRED_HOME=/opt/software/hadoop-2.6.5
export HBASE_HOME=/usr/local/src/hbase-1.2.1
export HIVE_HOME=/usr/local/src/apache-hive-2.0.0-bin

# 追加 /etc/profile 文件
vi /etc/profile

export SQOOP_HOME=/usr/local/src/sqoop-1.4.7.bin__hadoop-2.6.0
export PATH=$PATH:$SQOOP_HOME/bin
export CLASSPATH=$CLASSPATH:$SQOOP_HOME/lib

# 拷贝 jdbc 驱动包
cp /opt/software/mysql-connector-java-5.1.49.jar /usr/local/src/sqoop-1.4.7.bin__hadoop-2.6.0/lib/
# 配置 sqoop 连接 hive
cp /usr/local/src/apache-hive-2.0.0-bin/lib/hive-common-2.0.0.jar /usr/local/src/sqoop-1.4.7.bin__hadoop-2.6.0/lib/
# 测试 sqoop 是否正常连接 密码Password123$
sqoop list-databases --connect jdbc:mysql://127.0.0.1:3306/ --username root -P
```

# flume

## 安装以及配置 flume
```bash
# 解压 flume 软件包
tar zxvf /opt/software/apache-flume-1.6.0-bin.tar.gz -C/usr/local/src
# 修改 flume 安装目录所属用户
cd /usr/local/src; chown -R hadoop:hadoop apache-flume-1.6.0-bin
# 追加 /etc/profile 文件
vi /etc/profile

export FLUME_HOME=/usr/local/src/apache-flume-1.6.0-bin
export PATH=$PATH:$FLUME_HOME/bin

# 刷新环境变量
source /etc/profile
# 拷贝 flume-env.sh 模板文件
cd /usr/local/src/apache-flume-1.6.0-bin/conf; cp flume-env.sh.template flume-env.sh
# 修改 flume-env.sh 配置文件
vi /usr/local/src/apache-flume-1.6.0-bin/conf/flume-env.sh

export JAVA_HOME=/usr/local/src/jdk1.8.0_241

# 验证 flume 安装是否成功
flume-ng version
```