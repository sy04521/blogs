1.安装前清理工作；    
清理原有的mysql数据库；
使用以下命令查找出安装的mysql软件包和依赖包

```
rpm -pa | grep mysql
```
显示结果如下：
```
mysql80-community-release-el7-1.noarch
mysql-community-server-8.0.11-1.el7.x86_64
mysql-community-common-8.0.11-1.el7.x86_64
mysql-community-libs-8.0.11-1.el7.x86_64
mysql-community-client-8.0.11-1.el7.x86_64
使用以下命令依次删除上面的程序

yum remove mysql-xxx-xxx-
```
删除mysql的配置文件，卸载不会自动删除配置文件，首先使用如下命令查找出所用的配置文件
```
find / -name mysql
```
可能的显示结果如下：
```
/etc/logrotate.d/mysql
/etc/selinux/targeted/active/modules/100/mysql
/etc/selinux/targeted/tmp/modules/100/mysql
/var/lib/mysql
/var/lib/mysql/mysql
/usr/bin/mysql
/usr/lib64/mysql
/usr/local/mysql
```
根据需求使用以下命令 依次 对配置文件进行删除
```
rm -rf /var/lib/mysql
```


2. 下载
```
https://dev.mysql.com/downloads/repo/yum/

在下载按钮右击复制地址
```

3. 登陆阿里云远程连接或者xshell连接服务器
4. 在usr/local下新建mysql文件夹
```
mkdir /usr/local/mysql
```

5. 进入mysql文件夹
```
wget https://dev.mysql.com/get/mysql80-community-release-el8-1.noarch.rpm
```
6.安装 yum repo文件并更新 yum 缓存；
```
rpm -ivh https://dev.mysql.com/get/mysql80-community-release-el8-1.noarch.rpm
```
执行结果：

会在/etc/yum.repos.d/目录下生成两个repo文件mysql-community.repo mysql-community-source.repo

更新 yum 命令
```
yum clean all
yum makecache
```
7. 使用 yum安装mysql
当我们在使用yum安装mysql时，yum默认会从yum仓库中安装mysql最新的GA版本；如何选择自己的版本；

第一步： 查看mysql yum仓库中mysql版本，使用如下命令
```
yum repolist all | grep mysql
```
可以看到 MySQL 5.5 5.6 5.7为禁用状态 而MySQL 8.0为启用状态；

第二步 使用 yum-config-manager 命令修改相应的版本为启用状态最新版本为禁用状态，根据需要安装的版本修改。
```
yum-config-manager --disable mysql80-community #关闭8.0版本
yum-config-manager --enable mysql57-community #开启5.7版本
```
或者可以编辑 mysql repo文件，
```
cat /etc/yum.repos.d/mysql-community.repo 
```
将相应版本下的enabled改成 1 即可；
8.安装mysql 命令如下
```
yum install mysql-community-server
```
9.开启mysql 服务
```
systemctl start mysqld.service
```

10.Centos8修改mysql8.0版本的用户密码
```
修改/etc/my.cnf配置文件

vim /etc/my.cnf

添加skip-grant-tables配置
```

重启mysql服务
```
sudo systemctl restart mysqld
```
进入mysql控制台编辑
```
输入

mysql
```
在mysql控制台依次输入
```
use mysql;

set global validate_password.policy=0;

alter user 'root'@'localhost' identified by '12345678'; 
其中set global validate_password.policy=0作用将mysql检查密码强度降为最低，只检查密码的位数。如果你的密码强度很高，就没必要输入该条指令
```
但是这里执行最后一条指令的时候，会提示这样的一条错误：

ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement
```
原因就是我们修改的 /etc/my.cnf那条配置，允许我们不输入密码进入了mysql控制台，这里我们只需要输入

flush privileges;

alter user 'root'@'localhost' identified by '12345678'; 

从数据库表里刷新权限后再更新就能完成操作了。
```
退出控制台，重新修改/etc/my.cnf配置，将skip-grant-tables配置删除，重新启动mysql

```
sudo systemctl restart mysqld

 mysql -uroot -p12345678

密码设定生效
```
11.设置MySQL开放外网访问权限
查看root用户host及加密方式
```
#登录mysql
mysql -uroot -proot

#查看用户权限情况
use mysql;
select host,user,plugin from user;
```
12.更新用户host
```
#更新root用户的host，%表示任意IP都可连接，即可远程连接，localhost表示只能本地连接
update user set host='%' where user='root';

#查看更新结果
select host,user from user;
```
13.修改root用户权限
```
#修改root权限，第一个*表示通配数据库，也可指定数据库，第二个*表示通配表，也可指定表
grant all on *.* to 'root'@'%';

#更新权限
flush privileges;

#退出mysql
quit
```
14.放行端口或关闭防火墙
```
#查看防火墙运行状态
systemctl status firewalld
```

```
#放行指定端口（这里当然是放行3306），permenent表示设置为持久
firewall-cmd --permanent --add-port=3306/tcp

#查看放行结果
firewall-cmd --query-port=3306/tcp

#放行成功后重启防火墙
systemctl restart firewalld

#关闭指定端口
firewall-cmd --permanent --remove-port=3306/tcp

#关闭防火墙
systemctl stop firewall

```


