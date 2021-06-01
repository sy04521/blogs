1. 编译安装：            
  
-安装gcc编译环境：
```
yum install -y gcc-c++
```
-安装zlib-devel库：
```
yum install -y zlib-devel
```
-安装OpenSSL密码库：
```
yum install -y openssl openssl-devel
```
-安装pcre正则表达式库
```
yum install -y pcre pcre-devel
```
2.安装nginx cd /usr/loacl/
```
nginx下载官⽹网：http://nginx.org/en/download.html
wget http://nginx.org/download/nginx-1.16.0.tar.gz
tar xvf nginx-1.16.0.tar.gz
mkdir nginx
cd nginx-1.16.0
./configure --prefix=/usr/local/nginx --with-http_ssl_module --withhttp_
stub_status_module --with-pcre
make && make install
```
3.启停nginx服务
```
启动：
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
查看是否启动成功:
ps -ef | grep nginx
关闭：
/usr/local/nginx/sbin/nginx -s stop
```
4.开放80端口防火墙
```
firewall-cmd --zone=public --add-port=80/tcp --permanent 开放80端⼝口
firewall-cmd --reload 重启
```
5.把vue打包后的 文件上传到nginx/html下