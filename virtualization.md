# Virtualization

##CentOS Install

```bash
# 创建虚拟机时需要向kernel传入参数设置网卡名称
net.ifnames=0 biosdevname=0
```

## tigervnc-server

```bash
# 安装tigervnc服务
yum install tigervnc-server
# 复制端口配置文件并修改user
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
vim /etc/systemd/system/vncserver@:1.service
# 设置vnc密码
vncpasswd
# 重新加载配置文件
systemctl daemon-reload
# 启动vnc服务
systemctl start vncserver@:1.service
# 设置开机自启动
systemctl enable vncserver@:1.service
# 查看端口是否被监听
netstat -ntlp | grep Xvnc
# 启动防火墙
systemctl start firewalld
# 添加防火墙规则
firewall-cmd --zone=public --add-port=5901/tcp --permanent
# 防火墙规则重载
firewall-cmd --reload
# selinux临时关闭
setenforce 0
# 设置selinux关闭
vim /etc/selinux/config
```

##KVM

```bash
# 安装KVM用户态管理工具和KVM管理工具
yum install qemu-kvm libvirt -y
# 安装手动安装虚拟机工具
yum install virt-install -y
# 设置开机启动KVM管理工具
systemctl enable libvirtd
# 启动KVM管理工具
systemctl start libvirtd
# 创建KVM虚拟机硬盘
qemu-img create -f raw /opt/CentOS-7-x86_64.raw 10G
# 创建KVM虚拟机实例
virt-install --virt-type kvm --name CentOS-7-x86_64 --ram 1024 --cdrom=/tmp/CentOS-7-x86_64-DVD-1810.iso --disk path=/opt/CentOS-7-x86_64.raw --network network=default --graphics vnc,listen=0.0.0.0 --noautoconsole
# 查看所有虚拟机
virsh list --all
# 启动虚拟机
virsh start <VM_NAME>
# 关闭虚拟机
virsh shutdown <VM_NAME>
# 摧毁虚拟机
virsh destory <VM_NAM
# 解除绑定虚拟机
virsh undefine <VM_NAME>
# 修改虚拟机定义文件中的cpu部分
virsh edit <VM_NAME>
<vcpu placement'auto/static' current='<N>'><N></vcpu>
# 查看虚拟机定义文件
cat /etc/libvirt/qemu/<VM_NAME>.xml
# 为虚拟机热添加cpu
virsh setvcpus <VM_NAME> <N> --live
# 修改虚拟机定义文件中的内存部分 1G=1048576
virsh edit <VM_NAME>
<memory unit='KiB'><N></memory>

# 查看虚拟机内存
virsh qemu-monitor-command <VM_NAME> --hmp --cmd info balloon
# 为虚拟机热添加内存
virsh qemu-monitor-command <VM_NAME> --hmp --cmd  ballon <N>
```

### 设置虚拟机和本机在同个网段

```bash
# 添加一个桥接网卡
brctl addbr br0
# 将桥接网卡桥接到eth0
brctl addif br0 eth0
# 查看桥接网卡列表
brctl show
# 删除eth0网卡的ip地址
ip addr del dev eth0 <IP_ADDR/24>
# 将eth0删除的ip地址设置为br0的ip地址
ip addr add dev br0 <IP_ADDR/24>
# 启动br0网卡
ifconfig br0 <IP_ADDR/24> up
# 查看路由信息
route -n
# 添加默认网关
route add default gw <GATEWAY_ADDR>
# 修改虚拟机定义文件中的网卡部分为桥接
virsh edit <VM_NAME>
<interface type='bridge'>
  ...
  <source bridge='br0'/>

# 修改KVM虚拟机中的网卡配置文件
vi /etc/sysconfig/network-scripts/ifcfg-eth<N>

TYPE="Ethernet"
BOOTPROTO="static"
NAME="eth0"
DEVICE="eth0"
ONBOOT="yes"
IPADDR="192.168.30.250"
NETMASK="255.255.255.0"
GATEWAY="192.168.30.2"

# 重启网卡
systemctl restart network
```

### 创建widnows server 2003虚拟机

```bash
# 使用dd命令制作镜像文件
dd if=/dev/cdrom of=/opt/windows-server2003.iso
# 创建一块raw格式大小为10G的虚拟硬盘
qemu-img create -f raw /opt/windows_2003.raw 10G
# 创建windows server2003虚拟机
virt-install --virt-type kvm --name windows-server2003 --ram 1024 --cdrom=/opt/windows-server2003.iso --disk path=/opt/windows_2003.raw --network network=default --graphics vnc,listen=0.0.0.0 --noautoconsole
# 在windows server2003安装过程中重启需要编辑虚拟机配置文件
virsh edit windows-server2003

<disk type='file' device='cdrom'>
  ...
  <source file='/opt/windows-server2003.iso'/>
```

### 图形界面安装kvm虚拟机

```bash
# 安装kvm图形化管理工具
yum install qemu-kvm-tools virt-manager openssh-askpass
```
