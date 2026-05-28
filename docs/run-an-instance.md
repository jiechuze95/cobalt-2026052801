# 如何运行 cobalt 实例
本教程将帮助你运行自己的 cobalt 处理实例。如果你的实例面向公众，我们强烈建议你也[保护它免受滥用](/docs/protect-an-instance.md)，使用 Turnstile 或 API 密钥或两者兼用。

## 使用 Docker Compose 和 GitHub 包（推荐）
要运行 cobalt docker 包，你需要安装并配置 `docker` 和 `docker-compose`。

如果你需要安装 docker 的帮助，可以在此找到更多信息：
- [如何安装 docker](https://docs.docker.com/engine/install/)
- [如何安装 docker compose](https://docs.docker.com/compose/install/)

## 如何运行 cobalt docker 包：
1. 为 cobalt 配置文件创建一个文件夹，像这样：
    ```sh
    mkdir cobalt
    ```

2. 进入 cobalt 文件夹，创建 docker compose 配置文件：
    ```sh
    cd cobalt && nano docker-compose.yml
    ```
    本示例中使用的是 `nano`，你的发行版中可能没有。你可以使用任何其他文本编辑器。

3. 从[这里复制示例配置](examples/docker-compose.example.yml)并根据需要编辑。
    确保将默认 URL 替换为你自己的，否则 cobalt 将无法正常工作。

4. 最后，启动 cobalt 容器（在 cobalt 目录中）：
    ```sh
    docker compose up -d
    ```

如果你希望实例支持需要身份验证才能查看公开内容的服务，请在与 `docker-compose.yml` 相同的目录中创建 `cookies.json` 文件。示例 cookies 文件[可在此处找到](examples/cookies.example.json)。

cobalt 包将通过 watchtower 自动更新。

如果你希望实例面向公共互联网，强烈建议使用反向代理（如 nginx）。请在线查找教程。

## 在 Docker 外运行 cobalt API（适用于本地开发）
要求：
- node.js >= 18
- git
- pnpm

1. 克隆仓库：`git clone https://github.com/imputnet/cobalt`。
2. 进入 api 目录：`cd cobalt/api`。
3. 安装依赖：`pnpm install`。
4. 在同一目录中创建 `.env` 文件。
5. 将需要的环境变量添加到 `.env` 文件中。运行 cobalt 只需要 `API_URL`。
    - 如果你不知道本地开发使用什么 API URL，请使用 `http://localhost:9000/`。
6. 运行 cobalt：`pnpm start`。

### Ubuntu 22.04 解决方案
需要安装并运行 `nscd`，这样 `ffmpeg-static` 二进制文件才能解析 DNS（[#101](https://github.com/imputnet/cobalt/issues/101#issuecomment-1494822258)）：

```bash
sudo apt install nscd
sudo service nscd start
```

## 环境变量列表
[此部分已移至](/docs/api-env-variables.md)一个专用文档，更易于理解和维护。去看看吧！
