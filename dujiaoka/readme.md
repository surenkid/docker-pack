# 独角数卡

独角数卡是一个开源的站长自动化售货解决方案，基于流行的laravel框架开发，支持自定义前端模板和多语言包，并集成了多种支付接口。本程序适用于Linux环境，需要一定的上手难度，不支持虚拟主机和Windows服务器。

## 项目源地址

https://github.com/assimon/dujiaoka

## 部署示例

使用docker-compose
```
version: "3.1"

services:
  dujiaoka2ktsee:
    image: surenkid/dujiaoka:latest
    container_name: dujiaoka2ktsee
    hostname: dujiaoka2ktsee
    restart: unless-stopped
    network_mode: bridge
    volumes:
      - /home/volume/dujiaoka/env.env:/app/.env
      - /home/volume/dujiaoka/install.lock:/app/install.lock
    environment:
      WEB_DOCUMENT_ROOT: /app/public
      TZ: Asia/Shanghai
    ports:
      - "80:80"
```
其中配置文件参考[.env](https://github.com/assimon/dujiaoka/blob/master/.env)

