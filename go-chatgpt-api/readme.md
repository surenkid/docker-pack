# go-chatgpt-api
go-chatgpt-api 是一个使用 undetected_chromedriver 绕过 Cloudflare 验证，以便使用ChatGPT API的工具。同时也支持官方API，可以进行聊天等操作。

## 项目源地址
- https://github.com/linweiyuan/go-chatgpt-api

## 部署示例
使用docker-compose部署
```yaml
version: "3.1"

services:
  gochatgptapi2ktsee:
    image: surenkid/go-chatgpt-api:latest
    container_name: gochatgptapi2ktsee
    hostname: gochatgptapi2ktsee
    restart: unless-stopped
    network_mode: bridge
    ports:
      - 8124:8080
    environment:
      - GIN_MODE=release
      - CHATGPT_PROXY_SERVER=http://chatgptproxyserver2ktsee:9515
    depends_on:
      - undetectedchromedriver2ktsee
      
  undetectedchromedriver2ktsee:
    image: surenkid/undetected-chromedriver:latest
    container_name: undetectedchromedriver2ktsee
    hostname: undetectedchromedriver2ktsee
    restart: unless-stopped
    network_mode: bridge
```
