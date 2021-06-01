1. 安装nodejs
```
去淘宝镜像复制linux版本的nodejs下载链接
https://nodejs.org/dist/v13.0.0/

下载地址
https://nodejs.org/dist/v13.0.0/node-v13.0.0-linux-x64.tar.xz

```

2. 安装wget
```
yum -y install wget
```

3. 输入wget 复制的链接地址下载node
```
进入 cd /usr/local/

wget  https://nodejs.org/dist/v13.0.0/node-v13.0.0-linux-x64.tar.xz
```

4. 解压下载的node包
```
tar xvf node-v13.0.0-linux-x64.tar.xz
```

5. 修改文件夹名称
```
mv node-v13.0.0-linux-x64  node
```

6. 创建软连接，就可以在任意 目录下直接使 用node和npm命令
```
ln -s /usr/local/node/bin/node /usr/local/bin/node

ln -s /usr/local/node/bin/npm /usr/local/bin/npm

如果不不 小心输错了了路路径，重新创建会提示：‘ln: 无法创建符号链
接"/usr/local/bin/npm": 文件已存在’，输入`rm /usr/local/bin/npm`命令清除后
可以重新创建
```
7.测试安装是否成功
```
node -v 查看node版本
npm -v 查看npm版本
```
结束



