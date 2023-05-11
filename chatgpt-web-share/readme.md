# ChatGPT Web Share
共享一个 ChatGPT 账号给多用户同时使用的 web 应用，使用 FastAPI + Vue3 开发。可用于朋友之间共享或合租 ChatGPT 账号。支持 ChatGPT Plus / 设置对话模型 / 用户请求限制等功能。

## 项目源地址

- https://github.com/moeakwak/chatgpt-web-share

## 部署示例
使用docker-compose部署
```yaml
version: "3.1"

services:
  chatgptwebshare2ktsee:
    image: surenkid/chatgpt-web-share:latest
    container_name: chatgptwebshare2ktsee
    hostname: chatgptwebshare2ktsee
    restart: unless-stopped
    network_mode: bridge
    ports:
      - 8123:80
    volumes:
      - /home/volume/chatgptwebshare/data:/data
      - /home/volume/chatgptwebshare/logs:/app/logs
      - /home/volume/chatgptwebshare/config.yaml:/app/backend/api/config/config.yaml
    environment:
      - TZ=Asia/Shanghai
      - CHATGPT_BASE_URL=https://ai.fakeopen.com/api/
```
其中config.yaml示例
```yaml
print_sql: false
host: 127.0.0.1
port: 8000
data_dir: /data
database_url: 'sqlite+aiosqlite:////data/database.db'
run_migration: false
jwt_secret: jwt_secret
jwt_lifetime_seconds: 86400
cookie_max_age: 86400
user_secret: user_secret
sync_conversations_on_startup: true
create_initial_admin_user: true
initial_admin_username: admin
initial_admin_password: password
ask_timeout: 600
chatgpt_access_token: 'abc'
chatgpt_paid: true
chatgpt_base_url: 'https://ai.fakeopen.com/api/'
log_dir: /app/logs
console_log_level: INFO
request_log_counter_time_window: 2592000
request_log_counter_interval: 1800
ask_log_time_window: 2592000
sync_conversations_regularly: 'yes'
```
