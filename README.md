<div align="center">
    <br/>
    <p>
        <img src="web/static/favicon.png" title="cobalt" alt="cobalt logo" width="100" />
    </p>
    <p>
        保存你喜爱内容的最佳方式
        <br/>
        <a href="https://cobalt.tools">
            cobalt.tools
        </a>
    </p>
    <p>
        <a href="https://discord.gg/pQPt8HBUPu">
            💬 社区 Discord 服务器
        </a>
        <br/>
        <a href="https://x.com/justusecobalt">
            🐦 推特
        </a>
        <a href="https://bsky.app/profile/cobalt.tools">
            🦋 Bluesky
        </a>
    </p>
    <br/>
</div>

cobalt 是一个不会让你抓狂的媒体下载器。它友好、高效，没有广告、追踪器、付费墙或其他乱七八糟的东西。

粘贴链接，获取文件，继续前行。就这么简单，就该如此。

### cobalt 单体仓库
此单体仓库包含 api、前端及相关包的源代码：
- [api 目录与说明](/api/)
- [web 目录与说明](/web/)
- [packages 目录](/packages/)

它还包含 [docs 目录](/docs/) 中的文档：
- [如何运行 cobalt 实例](/docs/run-an-instance.md)
- [如何保护 cobalt 实例](/docs/protect-an-instance.md)
- [cobalt api 实例环境变量](/docs/api-env-variables.md)
- [cobalt api 文档](/docs/api.md)

### 伦理声明
cobalt 是一个让下载公开内容变得更简单的工具。它**不承担任何责任**。
最终用户对其下载的内容、使用和分发方式负全部责任。
cobalt 从不缓存任何内容，它[像一个高级代理一样工作](/api/src/stream/)。

cobalt 绝不是盗版工具，也不能被用作此类用途。
它只能下载免费且公开可访问的内容。
相同的内容可以通过任何现代网页浏览器的开发者工具下载。

### 贡献
如果你考虑为 cobalt 做出贡献，首先，谢谢你！在开始之前，请查看[贡献指南](/CONTRIBUTING.md)，它们会帮助你立即做出最好的贡献。

### 致谢
cobalt 由 [royalehosting.net](https://royalehosting.net/?partner=cobalt) 赞助。我们的一部分基础设施托管在他们的网络上。我们非常感谢他们的善意和支持！

### 许可证
有关许可信息，请参阅 [api](api/README.md) 和 [web](web/README.md) 的 README。
除非另有说明，本仓库的其余部分采用 [AGPL-3.0](LICENSE) 许可证。

---

## Docker 本地部署指南

### 环境要求

| 依赖 | 版本要求 | 说明 |
|------|---------|------|
| Docker | >= 20.10 | 容器运行环境 |
| Docker Compose | >= 2.0 | 容器编排工具 |
| Node.js | >= 20 | 前端构建需要 |
| pnpm | >= 9 | 前端依赖管理 |

### 端口规划

| 服务 | 端口 | 说明 |
|------|------|------|
| cobalt-api | 9000 | 后端 API 服务 |
| cobalt-web | 8888 | 前端 Web 界面 |

> **注意**：部署前请确保端口未被占用，可使用 `ss -tlnp | grep ':端口号'` 检查。

### 第一步：克隆代码

```bash
git clone https://github.com/jiechuze95/cobalt-2026052801.git
cd cobalt-2026052801
```

### 第二步：构建前端

```bash
# 安装 pnpm（如未安装）
npm install -g pnpm

# 进入前端目录
cd web

# 安装依赖
pnpm install

# 构建前端（替换为你的实际 IP 和 API 地址）
WEB_DEFAULT_API="http://你的IP:9000/" WEB_HOST="你的IP" pnpm build

# 返回项目根目录
cd ..
```

构建完成后，静态文件会输出到 `web/build` 目录。

### 第三步：创建 Nginx 配置

在项目根目录创建 `nginx.conf` 文件：

```nginx
server {
    listen 3000;
    server_name _;
    
    root /usr/share/nginx/html;
    index index.html;
    
    # 启用 gzip 压缩
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
    
    # 静态资源缓存
    location /_app/ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # SPA 路由
    location / {
        try_files $uri $uri.html $uri/ /index.html;
    }
    
    # 健康检查
    location /health {
        return 200 'ok';
        add_header Content-Type text/plain;
    }
}
```

### 第四步：创建 Docker Compose 配置

在项目根目录创建 `docker-compose.yml` 文件：

```yaml
services:
    cobalt-api:
        build:
            context: .
            dockerfile: Dockerfile
        container_name: cobalt-api
        init: true
        restart: unless-stopped
        ports:
            - "9000:9000/tcp"
        environment:
            API_URL: "http://你的IP:9000/"
            API_PORT: "9000"
            CORS_WILDCARD: "1"
            DURATION_LIMIT: "10800"
            RATELIMIT_WINDOW: "60"
            RATELIMIT_MAX: "20"

    cobalt-web:
        image: nginx:alpine
        container_name: cobalt-web
        restart: unless-stopped
        ports:
            - "8888:3000/tcp"
        volumes:
            - ./web/build:/usr/share/nginx/html:ro
            - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
        depends_on:
            - cobalt-api
```

> **重要**：将 `http://你的IP:9000/` 替换为你的实际服务器 IP 地址。

### 第五步：构建并启动服务

```bash
# 构建后端 API 镜像
docker compose build cobalt-api

# 启动所有服务
docker compose up -d

# 查看运行状态
docker ps --filter "name=cobalt" --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
```

### 第六步：验证部署

```bash
# 测试 API 服务
curl http://localhost:9000/

# 测试前端页面
curl -I http://localhost:8888/
```

正常响应示例：

**API 响应**：
```json
{
  "cobalt": {
    "version": "11.7.1",
    "url": "http://你的IP:9000/",
    "services": ["bilibili", "youtube", "tiktok", "twitter", ...]
  }
}
```

**前端响应**：HTTP 200 OK

### 访问地址

| 服务 | 地址 |
|------|------|
| 前端页面 | http://你的IP:8888/ |
| 后端 API | http://你的IP:9000/ |

### 常用运维命令

```bash
# 查看日志
docker logs -f cobalt-api      # API 日志
docker logs -f cobalt-web      # 前端日志

# 停止所有服务
docker compose down

# 重启服务
docker restart cobalt-api cobalt-web

# 重新构建并启动
docker compose down
docker compose build cobalt-api
docker compose up -d

# 查看资源占用
docker stats cobalt-api cobalt-web
```

### 支持的平台

cobalt 支持以下 21 个平台的媒体下载：

bilibili · bluesky · dailymotion · facebook · instagram · loom · ok.ru · pinterest · newgrounds · reddit · rutube · snapchat · soundcloud · streamable · tiktok · tumblr · twitch clips · twitter/x · vimeo · vk · youtube

### 支持的语言

| 语言 | 代码 | 状态 |
|------|------|------|
| English | en | ✅ |
| Русский | ru | ✅ |
| 简体中文 | zh | ✅ |

### Cookies 配置

Cookies 用于访问需要登录才能查看的公开内容（如 Instagram 私密帖子、Twitter 需登录内容等）。

#### 创建 cookies.json 文件

在项目根目录（与 `docker-compose.yml` 同级）创建 `cookies.json` 文件：

```json
{
    "instagram": [
        "mid=xxx; ig_did=xxx; csrftoken=xxx; ds_user_id=xxx; sessionid=xxx"
    ],
    "instagram_bearer": [
        "token=xxx",
        "token=IGT:2:xxx"
    ],
    "twitter": [
        "auth_token=xxx; ct0=xxx"
    ],
    "youtube": [
        "cookie=xxx; b=xxx"
    ],
    "reddit": [
        "client_id=xxx; client_secret=xxx; refresh_token=xxx"
    ],
    "vimeo": [
        "access_token=xxx"
    ]
}
```

#### 各平台 Cookies 获取方法

| 平台 | 需要的 Cookies | 获取方式 |
|------|---------------|---------|
| **Instagram** | `mid`, `ig_did`, `csrftoken`, `ds_user_id`, `sessionid` | 浏览器 F12 → Application → Cookies → instagram.com |
| **Instagram Bearer** | `token` | 浏览器 F12 → Network → 找到 API 请求 → 复制 Bearer token |
| **Twitter/X** | `auth_token`, `ct0` | 浏览器 F12 → Application → Cookies → twitter.com |
| **YouTube** | `cookie`, `b` | 浏览器 F12 → Application → Cookies → youtube.com |
| **Reddit** | `client_id`, `client_secret`, `refresh_token` | Reddit 开发者后台创建 App 获取 |
| **Vimeo** | `access_token` | Vimeo 开发者后台生成 |

#### 配置 Docker 挂载

修改 `docker-compose.yml`，添加环境变量和卷挂载：

```yaml
services:
    cobalt-api:
        build:
            context: .
            dockerfile: Dockerfile
        container_name: cobalt-api
        init: true
        restart: unless-stopped
        ports:
            - "9000:9000/tcp"
        environment:
            API_URL: "http://你的IP:9000/"
            API_PORT: "9000"
            CORS_WILDCARD: "1"
            COOKIE_PATH: "/cookies.json"          # 添加这行
        volumes:
            - ./cookies.json:/cookies.json:ro     # 添加这行（只读挂载）
```

#### 更新配置后重启

```bash
# 编辑 cookies.json 后重启服务
docker restart cobalt-api

# 或者重新部署
docker compose down && docker compose up -d
```

#### 注意事项

- **安全性**：`cookies.json` 包含敏感信息，不要提交到 Git（已在 `.gitignore` 中排除）
- **格式**：每个平台可以有多个 cookies（数组形式），cobalt 会随机使用
- **更新**：Cookies 会过期，需要定期更新
- **只读挂载**：Docker 中使用 `:ro` 只读挂载，提高安全性
- **Bearer Token**：Instagram 的 bearer token 不需要 `Bearer` 前缀，直接填写 token 值

### 常见问题

**Q: 端口被占用怎么办？**
修改 `docker-compose.yml` 中的端口映射，如将 `8888:3000` 改为 `8889:3000`。

**Q: 如何配置 cookies 支持需要登录的服务？**
在 `docker-compose.yml` 的 `cobalt-api` 环境变量中添加：
```yaml
environment:
    COOKIE_PATH: "/cookies.json"
volumes:
    - ./cookies.json:/cookies.json
```

**Q: 如何更新到最新版本？**
```bash
git pull
cd web && pnpm install && WEB_DEFAULT_API="http://你的IP:9000/" pnpm build && cd ..
docker compose build cobalt-api
docker compose up -d
```

