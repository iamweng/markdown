# linux

## command

```bash
# 向文件中追加内容
cat >> <FILE_NAME> << EOF
EOF
# 修改hostname
hostnamectl set-hostname <HOSTNAME>
# 修改selinux config文件
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
# 从其他服务器拉取文件
scp <IP_ADDR>:<FILE_NAME> <DIR>

# 设置启动挂载
vi /etc/fstab
# 设置启动挂载
echo "<command>" >> /etc/rc.d/rc.local
chmod +x /etc/rc.d/rc.local
```

## vsftpd

```bash
# 安装vsftpd模块
yum install vsftpd
# 编辑vsftpd配置文件添加可访问的目录
vi /etc/vsftpd/vsftpd.conf

anon_root=<DIR>
```

## nfs

```bash
# 安装nfs-utils模块
yum install nfs-utils
# 修改nfs-utils配置文件 开放/opt且授权任何用户可读写
vi /etc/exports

/opt *(rw,no_root_squash,sync)

# 其他服务器挂载nfs
mount -t nfs -o rw <IP_ADDR>:/<DIR> <DIR>
```