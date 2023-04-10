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

