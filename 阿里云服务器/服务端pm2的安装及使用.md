 - pm2介绍
```
PM2(process manager 2)是具有内置负载均衡器器的nodejs应⽤用程序的⽣生产进程管理理器器。它能使你
的程序永久保持活跃状态，⽆无需停机即可重新加载它们，简化常⻅见的系统管理理任务。
```
- pm2特性
  


```
  应用程序的日志管理理
```

- 集群模式：Node.js负载平衡和零宕机时间重新加载
```
- 性能监控：可以在终端中监控您的应用程序并检查应用程序运行行状况（CPU使⽤用率，使⽤用的
内存等）
- 多平台支持：适用于Linux（稳定）和macOS（稳定）和Windows（稳定）
```
- pm2安装

```
全局安装就ok
npm install pm2 -g
```
- pm2启动项目

```
pm2 start app.js

{
  "name": "x-server",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www",
    "pm2":"pm2 start ./bin/www"
  },
  "dependencies": {
    "cookie-parser": "~1.4.4",
    "crypto": "^1.0.1",
    "debug": "~2.6.9",
    "express": "~4.16.1",
    "express-jwt": "^6.0.0",
    "http-errors": "~1.6.3",
    "jade": "~1.11.0",
    "jsonwebtoken": "^8.5.1",
    "morgan": "~1.9.1",
    "multer": "^1.4.2",
    "mysql": "^2.18.1"
  }
}

>  客户端 vue2 中npm run pm2  就可以运行
```
- pm2 生成配置文件
```
pm2 ecosystem
```
- 配置文件
- 
```
  apps: [
    {
      name: 'app',
      script: './bin/www',
      watch: true,
      autorestart: true,
      instances: 1, //开启的进程数 0 代表 所有
      ignore_watch: [
        // 不不用监听的文件
        'node_modules',
        'logs',
      ],
      max_memory_restart: '1G', //最大内存
      error_file: './logs/app-err.log', // 错误日志文件
      out_file: './logs/app-out.log',
      log_date_format: 'YYYY-MM-DD HH:mm:ss', // 给每行行日志标记一个时间
      env: {
        NODE_ENV: 'development',
      },
      env_production: {
        NODE_ENV: 'production',
      },
    },
   
  ],

  deploy: {
    production: {
      user: 'root',
      host: '47.100.231.238',
      ref: 'origin/master', //
      repo: 'GIT_REPOSITORY',
      path: 'DESTINATION_PATH',
      'pre-deploy-local': '',
      'post-deploy':
        'npm install && pm2 reload ecosystem.config.js --env production',
      'pre-setup': '',
    },
  },
```
