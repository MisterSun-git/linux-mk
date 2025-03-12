# Linux常用命令
### 查看系统
``` bash
uname -a # 查看内核/操作系统/CPU信息 
head -n 1 /etc/issue # 查看操作系统版本 
cat /proc/cpuinfo # 查看CPU信息 
hostname # 查看计算机名 
lspci -tv # 列出所有PCI设备 
lsusb -tv # 列出所有USB设备 
lsmod # 列出加载的内核模块 
env # 查看环境变量资源 
free -m # 查看内存使用量和交换区使用量 
df -h # 查看各分区使用情况 
du -sh <目录名> # 查看指定目录的大小 
grep MemTotal /proc/meminfo # 查看内存总量 
grep MemFree /proc/meminfo # 查看空闲内存量 
uptime # 查看系统运行时间、用户数、负载 
cat /proc/loadavg # 查看系统负载磁盘和分区 
mount | column -t # 查看挂接的分区状态 
fdisk -l # 查看所有分区 
swapon -s # 查看所有交换分区 
hdparm -i /dev/hda # 查看磁盘参数(仅适用于IDE设备) 
dmesg | grep IDE # 查看启动时IDE设备检测状况网络 
ifconfig # 查看所有网络接口的属性 
iptables -L # 查看防火墙设置 
route -n # 查看路由表 
netstat -lntp # 查看所有监听端口 
netstat -antp # 查看所有已经建立的连接 
netstat -s # 查看网络统计信息进程 
ps -ef # 查看所有进程 
top # 实时显示进程状态用户 
w # 查看活动用户 
id <用户名> # 查看指定用户信息 
last # 查看用户登录日志 
cut -d: -f1 /etc/passwd # 查看系统所有用户 
cut -d: -f1 /etc/group # 查看系统所有组 
crontab -l # 查看当前用户的计划任务服务 
chkconfig –list # 列出所有系统服务 
chkconfig –list | grep on # 列出所有启动的系统服务程序 
rpm -qa # 查看所有安装的软件包

```


### Ubuntu安装
``` bash
apt update #更新
apt install -y git # 安装git
```

### 修改网卡
``` shell
#编辑网卡信息
vi /etc/sysconfig/network-scripts/ifcfg-ens33 
#重启网卡
service network restart

```
### 防火墙设置
``` shell
firewall-cmd --list-ports #查看已经开放的端口
firewall-cmd --zone=public --add-port=8080/tcp --permanent #放行端口 --zone 作用域 --permanent 永久生效
#端口放行后需要重启下防火墙
systemctl reload firewalld #重启防火墙
firewall-cmd --query-port=5601/tcp #查询指定端口是否开启成功
systemctl stop firewalld #停止防火墙
systemctl stop firewalld.service #停止防火墙
systemctl start firewalld #启动防火墙
systemctl start firewalld.service #启动防火墙
systemctl status firewalld #查看防火墙是否在运行
systemctl status firewalld.service #查看防火墙
systemctl stop firewalld.service #临时关闭防火墙
systemctl disable firewalld.service #永久关闭防火墙
```
### 修改主机名称
``` shell
#查看主机名称
cat /etc/hostname
#编辑主机名称
vi /etc/hostname
```
### 修改host
``` shell
vi /etc/hosts
```
### 查看全局路径
``` shell
echo $PATH
```

### rsync批量同步
``` shell

#!/bin/bash

#判断参数数量
if [ $# -lt 1 ]
then
    echo Not Enough Arguement!
    exit;
fi

#遍历集群所有机器
for host in rab1 rab2
do
echo ============== $host ====================
#遍历所有目录
for file in $@
    do
        #判断文件是否存在
        if [ -e $file ]
            then
                #获取父目录
                pdir=$(cd -P $(dirname $file);pwd)
                #获取当前文件的名称
                fname=$(basename $file)
                ssh $host "mkdir -p $pdir"
                rsync -av $pdir/$fname $host:$pdir
            else
                echo $file does not exists!
        fi
    done
done

```

### 查看大文件
``` shell
du -h -x --max-depth=1
```
### Linux开机自启动
``` shell
#查看开机启动项
chkconfig
#查看已启动的服务
systemctl list-units --type=service
#查看是否设置开机启动
systemctl list-unit-files | grep enable
```