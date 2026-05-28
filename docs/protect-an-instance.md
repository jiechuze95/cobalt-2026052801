# 如何保护你的 cobalt 实例
如果你持续收到大量未知流量，影响了实例的性能，那么启用机器人防护可能是个好主意。

> [!NOTE]
> 本教程在最新的 cobalt 10 官方版本上可以可靠运行。
> 我们无法保证与其他任何版本的完全兼容性。

## 配置 Cloudflare Turnstile
Turnstile 是一个免费、安全且尊重隐私的验证码替代方案。
cobalt 使用它自动过滤机器人和自动化脚本。
你的实例不需要通过 Cloudflare 代理即可使用 Turnstile。
你只需要一个免费的 Cloudflare 账户即可开始。

Cloudflare 仪表板界面可能会随时间变化，但基本操作应该保持不变。

> [!WARNING]
> 永远不要分享 Turnstile 密钥，始终保持私密。如果不慎泄露，请在小部件设置中轮换密钥。

1. 打开 [Cloudflare 仪表板](https://dash.cloudflare.com/) 并登录你的账户

2. 登录后，在侧边栏中选择 `Turnstile`
<div align="left">
    <p>
        <img src="images/protect-an-instance/sidebar.png" width="250" />
    </p>
</div>

3. 点击 `Add widget`
<div align="left">
    <p>
        <img src="images/protect-an-instance/add.png" width="550" />
    </p>
</div>

4. 输入小部件名称（可以是任何名称，如 "cobalt"）
<div align="left">
    <p>
        <img src="images/protect-an-instance/name.png" width="450" />
    </p>
</div>

5. 添加你希望小部件生效的 cobalt 前端域名，你可以随时更改此列表
    - 如果你想将处理实例与 [cobalt.tools](https://cobalt.tools/) 前端一起使用，请将 `cobalt.tools` 添加到列表中
<div align="left">
    <p>
        <img src="images/protect-an-instance/domain.png" width="450" />
    </p>
</div>

6. 选择 `invisible` 小部件模式
<div align="left">
    <p>
        <img src="images/protect-an-instance/mode.png" width="450" />
    </p>
</div>

7. 点击 `create`

8. 保持包含 sitekey 和密钥的页面打开，稍后你会用到它们。
如果你关闭了它，不用担心！
只需打开相同的 Turnstile 页面，点击你新创建的 Turnstile 小部件的"settings"即可。

<div align="left">
    <p>
        <img src="images/protect-an-instance/created.png" width="450" />
    </p>
</div>

你已成功创建了 Turnstile 小部件！
现在是时候将其添加到你的处理实例中了。

### 在处理实例上启用 Turnstile
本教程假设你的 `environment` 变量列表中只有 `API_URL`。
如果你有其他变量，只需在现有变量之后添加新的即可。

> [!CAUTION]
> 永远不要使用教程中的任何值，尤其是 `JWT_SECRET`！

1. 用你选择的文本编辑器打开 `docker-compose.yml` 配置文件。
2. 复制 Turnstile sitekey 和密钥，将它们粘贴到各自的变量中。
`TURNSTILE_SITEKEY` 用于 sitekey，`TURNSTILE_SECRET` 用于密钥：
```yml
environment:
    API_URL: "https://your.instance.url.here.local/"
    TURNSTILE_SITEKEY: "2x00000000000000000000BB" # 使用你的密钥
    TURNSTILE_SECRET: "2x0000000000000000000000000000000AA" # 使用你的密钥
```
3. 生成一个 `JWT_SECRET`。我们建议使用长度至少为 64 个字符的字母数字组合。
此字符串将用作所有 JWT 密钥的盐值。

    你可以使用 `pnpm -r token:jwt` 生成随机密钥，或使用任何你喜欢的方式。

```yml
environment:
    API_URL: "https://your.instance.url.here.local/"
    TURNSTILE_SITEKEY: "2x00000000000000000000BB" # 使用你的密钥
    TURNSTILE_SECRET: "2x0000000000000000000000000000000AA" # 使用你的密钥
    JWT_SECRET: "bgBmF4efNCKPirD" # 创建新密钥，不要使用这个
```
4. 重启 docker 容器。

## 配置 API 密钥
如果你想在 Web 界面之外使用你的实例，你需要一个 API 密钥！

> [!NOTE]
> 本教程假设你将密钥文件保存在实例服务器的本地。
> 如果你希望将文件上传到远程位置，
> 请将 `API_KEYS_URL` 的值替换为文件的直接 URL
> 并跳过第二步。

> [!WARNING]
> 远程存储密钥文件时，请确保它不被公开访问，
> 并且链接要么经过身份验证（通过查询参数），要么无法猜测。
>
> 如果 API 密钥泄露，你将需要更新/删除所有 UUID 来撤销它们。

1. 按照[下方的 schema 和示例](#api-key-file-format)创建一个 `keys.json` 文件。

2. 将 `keys.json` 暴露给 docker 容器：
```yml
volumes:
    - ./keys.json:/keys.json:ro # ro - 只读
```

3. 将密钥文件的路径添加到容器环境变量中：
```yml
environment:
    # ... 其他变量 ...
    API_KEY_URL: "file:///keys.json"
```

4. 重启 docker 容器。

## 使用 API 密钥但不使用 Turnstile 限制实例访问
默认情况下，API 密钥是附加的，意味着它们不是*必需的*，
而是与 Turnstile 或无身份验证（常规 IP 哈希速率限制）配合使用。

要始终要求身份验证（通过密钥或 Turnstile，如果已配置），将 `API_AUTH_REQUIRED` 设置为 1：
```yml
environment:
    # ... 其他变量 ...
    API_AUTH_REQUIRED: 1
```

- 如果同时启用了密钥和 Turnstile，则不会有任何变化。
- 如果只配置了密钥，则所有没有有效 API 密钥的请求都将被拒绝。

### 为什么默认不将密钥设为独占？
密钥可以用于绕过速率限制，
同时保持 API 其余部分的速率限制，且不使用 Turnstile。

## API 密钥文件格式
文件是一个 JSON 序列化对象，结构如下：
```typescript

type KeyFileContents = Record<
    UUIDv4String,
    {
        name?: string,
        limit?: number | "unlimited",
        ips?: (CIDRString | IPString)[],
        userAgents?: string[],
        allowedServices?: "all" | string[],
    }
>;
```

其中 *`UUIDv4String`* 是 UUIDv4 标识符的字符串版本。
- **name** 是供你自己参考的字段，cobalt 不会在任何地方使用它。

- **`limit`** 指定 API 密钥在 `RATELIMIT_WINDOW` 环境变量指定的时间窗口内可以发出的请求数量。
    - 省略时，将使用 `RATELIMIT_MAX` 中指定的限制。
    - 也可以设置为 `"unlimited"`，此时 API 密钥将绕过所有速率限制。

- **`ips`** 包含一个允许的 IP 范围数组，可以指定为单个 IP 或 CIDR 范围（例如 *`["192.168.42.69", "2001:db8::48", "10.0.0.0/8", "fe80::/10"]`*）。
    - 指定后，只有来自这些 IP 范围的请求才能使用指定的 API 密钥。
    - 省略时，任何 IP 都可以使用该 API 密钥发出请求。

- **`userAgents`** 包含一个允许的 User-Agent 数组，支持通配符（例如 *`["cobaltbot/1.0", "Mozilla/5.0 * Chrome/*"]`*）。
    - 指定后，`user-agent` 不在此数组中的请求将被拒绝。
    - 省略时，可以指定任何 User-Agent 使用该 API 密钥发出请求。

- **`allowedServices`** 是一个允许的服务数组或 `"all"`。
    - 指定 `"all"` 时，密钥将能够访问所有支持的服务，即使它们通过 `DISABLED_SERVICES` 被全局禁用。
    - 指定服务数组时，密钥只能访问数组中包含的服务。
    - 省略时，密钥将使用全局支持的服务列表。

- 如果同时设置了 `ips` 和 `userAgents`，令牌将同时受这两个参数限制。
- 如果 cobalt 检测到你的密钥文件有任何问题，它将被忽略，并在控制台打印警告。

示例密钥文件如下所示：
```json
{
    "b5c7160a-b655-4c7a-b500-de839f094550": {
        "limit": 10,
        "ips": ["10.0.0.0/8", "192.168.42.42"],
        "userAgents": ["*Chrome*"]
    },
    "b00b1234-a3e5-99b1-c6d1-dba4512ae190": {
        "limit": "unlimited",
        "ips": ["192.168.1.2"],
        "userAgents": ["cobaltbot/1.0"]
    }
}
```

如果你正在配置密钥文件，**不要使用示例中的 UUID**，而是生成你自己的。如果你安装了 node.js，可以运行以下命令：
`node -e "console.log(crypto.randomUUID())"`
