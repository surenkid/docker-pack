# openai-api-proxy-key-pool
它是一个反向代理服务，用于代理 OpenAI API 的请求。它还提供了轮询池 key 调用功能，使您能够在访问 OpenAI API 时轮询使用不同的 API 密钥，并支持配置文件，方便管理 API 密钥。您可以使用Go编译、Docker部署或Docker Compose部署该服务，并可以在端口8124上运行该服务，发送您的请求到http://localhost:8124

## 项目源地址
- https://github.com/surenkid/openai-api-proxy-key-pool

## 部署示例
使用docker-compose部署
```yaml
version: '3'

services:
  openai-api-proxy-key-pool:
    image: surenkid/openai-api-proxy-key-pool:latest
    ports:
      - "8124:8124"
    volumes:
      - ./config/config.json:/config/config.json
```
