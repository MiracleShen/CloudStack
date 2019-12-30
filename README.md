# CloudStack

1、基本环境安装
yum -y update
//首先要关闭NetworkManager，否则后面网络会不通，官方文件上没有写
systemctl stop NetworkManager
systemctl disable NetworkManager
//安装桥接组件
yum install bridge-utils -y
//新增ifcfg-cloudbr0
vim /etc/sysconfig/network-scripts/ifcfg-cloudbr0
//ifcfg-cloudbr0里的配置文件
DEVICE=cloudbr0
TYPE=Bridge
ONBOOT=yes
BOOTPROTO=static
IPV6INIT=no
IPV6_AUTOCONF=no
DELAY=5
IPADDR=172.16.10.2
GATEWAY=172.16.10.1
NETMASK=255.255.255.0
DNS1=8.8.8.8
DNS2=8.8.4.4
STP=yes
USERCTL=no
NM_CONTROLLED=no
//修改主网卡
vim /etc/sysconfig/network-scripts/ifcfg-eth0
//eth0，实际网卡的配置
TYPE=Ethernet
BOOTPROTO=none
DEFROUTE=yes
NAME=eth0
DEVICE=eth0
ONBOOT=yes
BRIDGE=cloudbr0
//重启网卡
systemctl enable network
systemctl restart network
//更改Hosts
vim /etc/hosts 
//Hosts内容
127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
::1 localhost localhost.localdomain localhost6 localhost6.localdomain6
172.16.10.2 srvr1.cloud.priv
//网卡重启
systemctl restart network
//修改Selinux
vim /etc/selinux/config 
//Selinux内容
SELINUX=permissive

2、cloudstack-management Node安装

