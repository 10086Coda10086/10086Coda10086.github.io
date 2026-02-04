## 1. 安装面板**Portainer**

**定位**：最常用的 Docker 图形化管理面板
**适用场景**：新手 + 中高级用户都合适

### 优点

- 简洁、易上手
- 能管理本机 Docker 以及远程 Docker
- 支持 Stack / Compose
- 内置应用模板（快速部署服务）
- 社区活跃

### 缺点

- 默认无需复杂 RBAC（权限管理）时体验最好
- 栈管理不如 Swarm/Kubernetes 强大

### 安装极简示例（Docker）

```
docker run -d \
  -p 9000:9000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```

访问端口：

```
http://服务器IP:9000
```

---

## 2. 安装Transmission

### 1️⃣ 创建下载目录

你已经把 `sdb` 挂载到 `/data` 下，我们为 Transmission 创建专用目录：

```
sudo mkdir -p /data/transmission/config
sudo mkdir -p /data/transmission/downloads
sudo chown -R lluosu:lluosu /data/transmission
```

- `config` 存放 Transmission 配置文件
- `downloads` 存放下载的文件
- 权限设置为你自己的用户，避免 Docker 访问权限问题

------

### 2️⃣ 编写 Docker Compose 文件

在你的主目录下创建一个文件夹存放 docker-compose 文件，例如：

```
mkdir -p ~/docker/transmission
cd ~/docker/transmission
```

创建 `docker-compose.yml`：

```
version: "3.9"
services:
  transmission:
    image: linuxserver/transmission
    container_name: transmission
    environment:
      - PUID=1000              # 你的 Linux 用户 ID
      - PGID=1000              # 你的 Linux 用户组 ID
      - TZ=Asia/Shanghai       # 时区
    volumes:
      - /data/transmission/config:/config
      - /data/transmission/downloads:/downloads
    ports:
      - 9091:9091   # Web UI
      - 51413:51413 # BitTorrent TCP
      - 51413:51413/udp # BitTorrent UDP
    restart: unless-stopped
```

> 注意：
>
> - `PUID` 和 `PGID` 用 `id lluosu` 查看你的用户和组 ID
> - 端口可以按需要改，如果 9091 被占用，可以换成 9092:9091

------

### 3️⃣ 启动容器

进入 `docker-compose.yml` 所在目录，执行：

```
docker compose up -d
```

检查容器状态：

```
docker ps
```

你应该看到 `transmission` 正在运行。

------

### 4️⃣ 访问 Web UI

在浏览器访问：

```
http://服务器IP:9091
```

- 默认用户名: `transmission`
- 默认密码: `transmission`

可以在 `/data/transmission/config/settings.json` 修改用户名密码，重启容器后生效。

------

### 5️⃣ 常见问题排查

| 问题                            | 解决方法                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| Web UI打不开                    | - 检查端口映射 `9091:9091` 是否被占用 - 防火墙放行 `sudo ufw allow 9091/tcp` |
| 下载目录无法写入                | - 检查 `/data/transmission/downloads` 权限 - 确保 `PUID/PGID` 对应你的用户 |
| Docker 报错 `permission denied` | - 运行 `sudo usermod -aG docker $USER` 后重登录 - 确认 Docker 服务已启动 |

---

## 3. 安装Home Assistant

### 1️⃣ 前提条件

- 已安装 Docker 和 Docker Compose
- 服务器有可用网络
- 建议准备一个宿主机目录用于 Home Assistant 数据（例如 `/data/homeassistant`）

```bash
sudo mkdir -p /data/homeassistant
sudo chown 1000:1000 /data/homeassistant   # Docker 官方容器推荐 PUID=1000
```

> 如果你之前已经做了类似的 Docker 容器映射，可以共用 `/data` 目录。

------

### 2️⃣ 创建 Docker Compose 文件

在宿主机任意目录（比如 `/home/lluosu/docker/homeassistant`）创建 `docker-compose.yml`：

```yaml
version: "3.8"

services:
  homeassistant:
    container_name: homeassistant
    image: ghcr.io/home-assistant/home-assistant:stable
    volumes:
      - /data/homeassistant:/config
    environment:
      - TZ=Asia/Shanghai
    restart: unless-stopped
    network_mode: host
```

**说明**：

- `volumes`：宿主机 `/data/homeassistant` 映射到容器 `/config`，存储 HA 配置文件
- `network_mode: host`：HA 建议使用 host 网络模式，否则部分集成可能受限
- `restart: unless-stopped`：开机自启

------

### 3️⃣ 启动 Home Assistant

在 `docker-compose.yml` 所在目录运行：

```bash
docker compose up -d
```

- `-d` 表示后台运行
- 运行完成后，容器会自动拉取镜像并启动

查看容器状态：

```bash
docker ps
```

------

### 4️⃣ 访问 Home Assistant

打开浏览器访问：

```
http://<服务器IP>:8123
```

首次访问会进入初始化页面，设置管理员账号即可。

------

### 5️⃣ 常见问题

1. **端口冲突**
   - HA 默认 8123，确保没有其他容器或服务占用
   - 如果被占用，可修改 Docker Compose 为 `ports: - "8124:8123"`（8124 是宿主机端口）
2. **网络模式问题**
   - HA 建议 host 网络模式，否则部分 Zigbee/Z-Wave/UPnP 功能可能失效
3. **权限问题**
   - 宿主机目录必须可写 (`chmod 775 /data/homeassistant`)
   - 如果遇到容器无法写入配置，检查 PUID/PGID 是否正确
4. **更新 Home Assistant**

```bash
docker compose pull
docker compose up -d
```

------

