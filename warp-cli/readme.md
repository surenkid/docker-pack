# warp-cli

warp-cli 是 Cloudflare Warp 命令行工具,用于管理 Warp 账户和配置 Warp 客户端。

## 项目源地址

https://watermelonwater.tech/cloudflare-warp%e7%9a%84docker%e6%96%b9%e6%a1%88%e4%b8%80%e9%94%ae%e8%a7%a3%e5%86%b3chatgpt%e7%9a%84%e8%ae%bf%e9%97%ae/

## 部署示例

使用docker-compose
```yaml
version: "3.1"

services:
  warpcli:
    image: surenkid/warp-cli:latest
    container_name: warpcli
    restart: unless-stopped
    ports:
      - "65535:65535"
```
