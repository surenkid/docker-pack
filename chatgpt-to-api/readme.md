# chatgpt-to-api

该项目通过ChatGPT网页版创建一个模拟API。使用AccessToken将ChatGPT模拟成OpenAI API，让用户可以在各种应用程序中访问API，而无需额外付费。模拟后提供的API与官方API基本兼容。

## 项目地址

https://github.com/acheong08/ChatGPT-to-API

## 部署示例
docker-compose部署
```yaml
version: '3'

services:
  chatgpttoapi:
    image: surenkid/chatgpt-to-api:latest
    container_name: chatgpttoapi
    hostname: chatgpttoapi
    restart: unless-stopped
    network_mode: bridge
    ports:
      - '8080:8080'
    environment:
      SERVER_HOST: 0.0.0.0
      SERVER_PORT: 8080
      ADMIN_PASSWORD: TotallySecurePassword
      API_REVERSE_PROXY: https://bypass.churchless.tech/api/conversation
      # PUID: xxx
```
