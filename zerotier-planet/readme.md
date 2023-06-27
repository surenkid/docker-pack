# zerotier-planet
zerotier-planet 是一个自建 ZeroTier 的 Planet 服务器，可以在互联网基础上构建自己的私有网络，实现点对点直连，并通过 P2P 等方式实现形如交换机或路由器上 LAN 设备的内网互联。

## 项目源地址
- https://github.com/xubiaolin/docker-zerotier-planet

## 部署示例
使用docker-compose部署
```yaml
version: "3.1"

services:
  zerotier-planet:
    image: surenkid/zerotier-planet:latest
    container_name: zerotier-planet
    restart: unless-stopped
    ports:
      - "3443:3443"
      - "22083:22083"
      - "22083:22083/udp"
    volumes:
      - ./zt1:/var/lib/zerotier-one/
      - ./ztncui:/opt/key-networks/ztncui/etc/
```

## 其他说明
部署完成后，拷贝planet文件到当前目录备用
```
docker cp zerotier-planet:/app/bin/planet .
```
面板默认账号密码为`admin / password`
