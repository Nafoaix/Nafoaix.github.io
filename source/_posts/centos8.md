# 阿里云服务器部署个人网盘服务

> 相关资源：
> kiftd ： https://github.com/KOHGYLW/kiftd 

Linux压缩包：https://cloud.189.cn/t/ruIr2eyeyUVb (访问码:v5pj)

## 1.安装JAVA运行环境
```
yum install java-11-openjdk-devel
```
安装完成后查看版本，输出下面内容说明已经安装好。
```
[root@nafxali ~]# java -version
openjdk version "11.0.9.1" 2020-11-04 LTS
OpenJDK Runtime Environment 18.9 (build 11.0.9.1+1-LTS)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.9.1+1-LTS, mixed mode, sharing)
```

查看防火墙某个端口是否开放
firewall-cmd --query-port=3306/tcp
开放防火墙端口3306
firewall-cmd --zone=public --add-port=3306/tcp --permanent
注意：开放端口后要重启防火墙生效
重启防火墙
systemctl restart firewalld
关闭防火墙端口
firewall-cmd --remove-port=3306/tcp --permanent
查看防火墙状态
systemctl status firewalld
关闭防火墙
systemctl stop firewalld
打开防火墙
systemctl start firewalld
开放一段端口
firewall-cmd --zone=public --add-port=40000-45000/tcp --permanent
查看开放的端口列表
firewall-cmd --zone=public --list-ports
查看被监听(Listen)的端口
netstat -lntp
检查端口被哪个进程占用
netstat -lnp|grep 3306
