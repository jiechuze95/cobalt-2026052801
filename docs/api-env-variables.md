# cobalt api 实例环境变量
你可以使用这些环境变量来自定义处理实例的行为。除 `API_URL` 外，所有变量均为可选。
本文档尚未完成，后续会逐步完善。欢迎改进！

### 通用变量
| 名称                   | 默认值  | 值示例                                |
|:-----------------------|:--------|:--------------------------------------|
| API_URL                |         | `https://api.url.example/`            |
| API_PORT               | `9000`  | `1337`                                |
| COOKIE_PATH            |         | `/cookies.json`                       |
| PROCESSING_PRIORITY    |         | `10`                                  |
| API_INSTANCE_COUNT     |         | `6`                                   |
| API_REDIS_URL          |         | `redis://localhost:6379`              |
| DISABLED_SERVICES      |         | `bilibili,youtube`                    |
| FORCE_LOCAL_PROCESSING | `never` | `always`                              |
| API_ENV_FILE           |         | `/.env`                               |

[*查看详情*](#general)

### 网络变量
| 名称                | 默认值    | 值示例                                |
|:--------------------|:----------|:--------------------------------------|
| API_LISTEN_ADDRESS  | `0.0.0.0` | `127.0.0.1`                           |
| FREEBIND_CIDR       |           | `2001:db8::/32`                       |

#### undici 代理变量
| 名称        | 值示例                                |
|:------------|:--------------------------------------|
| HTTP_PROXY  | `http://user:password@10.0.0.1:1337/` |
| HTTPS_PROXY | `https://10.0.0.2:1337/`              |
| NO_PROXY    | `localhost`                           |

[*查看详情*](#networking)

### 限制变量
| 名称                     | 默认值  | 值示例 |
|:-------------------------|:--------|:-------|
| DURATION_LIMIT           | `10800` | `18000` |
| TUNNEL_LIFESPAN          | `90`    | `120`   |
| RATELIMIT_WINDOW         | `60`    | `120`   |
| RATELIMIT_MAX            | `20`    | `30`    |
| SESSION_RATELIMIT_WINDOW | `60`    | `60`    |
| SESSION_RATELIMIT_MAX    | `10`    | `10`    |
| TUNNEL_RATELIMIT_WINDOW  | `60`    | `60`    |
| TUNNEL_RATELIMIT_MAX     | `40`    | `10`    |

[*查看详情*](#limits)

### 安全变量
| 名称              | 默认值  | 值示例                                |
|:------------------|:--------|:--------------------------------------|
| CORS_WILDCARD     | `1`     | `0`                                   |
| CORS_URL          |         | `https://web.url.example`             |
| TURNSTILE_SITEKEY |         | `1x00000000000000000000BB`            |
| TURNSTILE_SECRET  |         | `1x0000000000000000000000000000000AA` |
| JWT_SECRET        |         | 参见 [详情](#security)              |
| JWT_EXPIRY        | `120`   | `240`                                 |
| API_KEY_URL       |         | `file://keys.json`                    |
| API_AUTH_REQUIRED |         | `1`                                   |

[*查看详情*](#security)

### 服务特定变量
| 名称                             | 值示例                   |
|:---------------------------------|:-------------------------|
| CUSTOM_INNERTUBE_CLIENT          | `IOS`                    |
| YOUTUBE_SESSION_SERVER           | `http://localhost:8080/` |
| YOUTUBE_SESSION_INNERTUBE_CLIENT | `WEB_EMBEDDED`           |
| YOUTUBE_ALLOW_BETTER_AUDIO       | `1`                      |
| ENABLE_DEPRECATED_YOUTUBE_HLS    | `key`                    |
| YOUTUBE_PLAYER_ID                | `abcdefff`               |

[*查看详情*](#service-specific)

## general
[*跳转到表格*](#general-vars)

### API_URL
> [!NOTE]
> API_URL 是运行 API 实例所必需的。

你的实例将通过此 URL 可以被访问。可以是外部或内部地址，但必须是有效的 URL，否则隧道功能将无法工作。

值为一个 URL。

### API_PORT
API 服务器可访问的端口。

值为 1024 到 65535 之间的数字。

### COOKIE_PATH
`cookies.json` 文件的路径，相对于 cobalt 实例的当前工作目录（通常是主文件夹 src/api）。

### PROCESSING_PRIORITY
ffmpeg 子进程的 `nice` 值。仅在 Unix 系统上可用。

注意：nice 值越高，优先级越低。你可以[在此阅读更多关于 nice 的信息](https://en.wikipedia.org/wiki/Nice_(Unix))。

值为一个数字。

### API_INSTANCE_COUNT
仅在 Linux 和 node.js `>=23.1.0` 上支持。配置后，cobalt 将生成多个子实例，在子实例之间进行请求负载均衡。使用此选项需要配置 `API_REDIS_URL`。

值为一个数字。

### API_REDIS_URL
配置后，cobalt 将使用此 redis 实例用于隧道缓存。当 `API_INSTANCE_COUNT` 大于 1 时是必需的，否则子实例将无法共享缓存。

值为一个 URL。

### DISABLED_SERVICES
逗号分隔的列表，用于禁用某些服务。

值为 cobalt 支持的服务字符串。

### FORCE_LOCAL_PROCESSING
值为字符串：`never`（默认）、`session` 或 `always`：
- 当变量未定义或设置为 `never` 时，所有请求都可以通过 POST 请求中的 `localProcessing` 设置偏好。
- 当设置为 `session` 时，只有来自会话（Bearer 令牌）客户端的请求将被强制使用本地设备处理。
- 当设置为 `always` 时，所有请求都将被强制使用本地设备处理，无论偏好如何。

### API_ENV_FILE
`key=value` 格式的环境变量文件的 URL 或本地路径。用于动态重新加载环境变量。**并非所有环境变量都能通过此方式更新。**（例如，速率限制器在启动 cobalt 时实例化，无法更改）

## networking
[*跳转到表格*](#networking-vars)

### API_LISTEN_ADDRESS
定义 API 实例的本地地址。如果你使用 docker 容器，通常不需要配置此项。

值为一个本地 IP 地址。

### HTTP_PROXY, HTTPS_PROXY, NO_PROXY
代理的 URL，将传递给 [`EnvHttpProxyAgent`](https://undici.nodejs.org/#/docs/api/EnvHttpProxyAgent) 用于代理外部请求。如果使用代理时某些 cobalt 功能出现问题，请[提交新 issue](https://github.com/imputnet/cobalt/issues)！

引用自 [undici 文档](https://undici.nodejs.org/#/docs/api/EnvHttpProxyAgent)：
> 当设置了 `HTTP_PROXY` 和 `HTTPS_PROXY` 时，`HTTP_PROXY` 用于 HTTP 请求，`HTTPS_PROXY` 用于 HTTPS 请求。如果只设置了 `HTTP_PROXY`，则 `HTTP_PROXY` 同时用于 HTTP 和 HTTPS 请求。如果只设置了 `HTTPS_PROXY`，则仅用于 HTTPS 请求。

> `NO_PROXY` 是一个逗号或空格分隔的主机名列表，不应使用代理。列表可以包含前导通配符字符（`*`）。如果设置了 `NO_PROXY`，EnvHttpProxyAgent() 将对与列表匹配的主机的请求绕过代理。如果 `NO_PROXY` 设置为 `"*"`，EnvHttpProxyAgent() 将对所有请求绕过代理。

值为字符串：
- `HTTP_PROXY`/`HTTPS_PROXY`：URL 或主机名。
- `NO_PROXY`：逗号或空格分隔的主机名列表。

### API_EXTERNAL_PROXY（已弃用）
> [!WARNING]
> 此环境变量已弃用，将在未来版本中移除。请更新配置以使用 `HTTP_PROXY` 或 `HTTPS_PROXY`，如上所述。

代理的 URL，将传递给 [`EnvHttpProxyAgent`](https://undici.nodejs.org/#/docs/api/EnvHttpProxyAgent)，用于代理外部请求。仅支持 HTTP(S)。

值为一个 URL。

### FREEBIND_CIDR
用于为 cobalt 请求随机分配地址的 IPv6 前缀。仅在 Linux 系统上可用。

设置 `FREEBIND_CIDR` 允许 cobalt 为每次下载选择一个随机 IP，并用于该特定下载的所有请求。

要在 cobalt 中使用 freebind，你需要先遵循其[安装说明](https://github.com/imputnet/freebind.js?tab=readme-ov-file#setup)。

如果你想使用此选项并在 docker 容器中运行 cobalt，你还需要将 `API_LISTEN_ADDRESS` 环境变量设置为 `127.0.0.1`，并将容器的 `network_mode` 设置为 `host`。

值为一个 IPv6 范围。

## limits
[*跳转到表格*](#limit-vars)

### DURATION_LIMIT
媒体时长限制，单位为**秒**。

值为一个数字。

### TUNNEL_LIFESPAN
隧道信息在内存中存储的时长，**单位为秒**。

建议保持默认值或尽可能低，以确保效率和用户隐私。

值为一个数字。

### RATELIMIT_WINDOW
API 请求（非会话请求）的速率限制时间窗口，单位为**秒**。

值为一个数字。

### RATELIMIT_MAX
在 `RATELIMIT_WINDOW` 时间窗口内允许的 API 请求数量。

值为一个数字。

### SESSION_RATELIMIT_WINDOW
会话创建请求的速率限制时间窗口，单位为**秒**。

值为一个数字。

### SESSION_RATELIMIT_MAX
在 `SESSION_RATELIMIT_WINDOW` 时间窗口内允许的会话请求数量。

值为一个数字。

### TUNNEL_RATELIMIT_WINDOW
隧道（代理/流）请求的速率限制时间窗口，单位为**秒**。

值为一个数字。

### TUNNEL_RATELIMIT_MAX
在 `TUNNEL_RATELIMIT_WINDOW` 时间窗口内允许的隧道请求数量。

值为一个数字。

## security
[*跳转到表格*](#security-vars)

> [!NOTE]
> 要启用 Turnstile 机器人防护，必须同时设置 `TURNSTILE_SITEKEY`、`TURNSTILE_SECRET` 和 `JWT_SECRET`。三个都需要设置。

### CORS_WILDCARD
定义是否启用跨源资源共享。启用后，你的实例将可以从外部网页访问。

值为数字，`0` 或 `1`。

### CORS_URL
配置[跨源资源共享源](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Allow-Origin)。如果 `CORS_WILDCARD` 设置为 `0`，你的实例将仅从此 URL 可用。

值为一个 URL。

### TURNSTILE_SITEKEY
[Cloudflare Turnstile](https://www.cloudflare.com/products/turnstile/) 站点密钥，由 Web 客户端用于请求和解决挑战以证明用户不是机器人。

值为特定密钥。

### TURNSTILE_SECRET
[Cloudflare Turnstile](https://www.cloudflare.com/products/turnstile/) 密钥，由处理实例用于验证客户端是否成功解决了挑战。

值为特定密钥。

### JWT_SECRET
用于签发 JWT 令牌以进行请求认证的密钥。值必须是一个随机、安全且足够长的字符串（超过 16 个字符）。

值为特定密钥。

### JWT_EXPIRY
cobalt 签发的 JWT 令牌的有效期时长，单位为秒。

值为一个数字。

### API_KEY_URL
外部或本地密钥数据库的 URL。对于本地文件，你需要使用 `file://` 协议指定本地路径。

参见"如何保护你的 cobalt 实例"文档中的 [api key 部分](/docs/protect-an-instance.md#api-key-file-format)了解详情。

值为一个 URL。

### API_AUTH_REQUIRED
设置为 `1` 时，用户在访问 API 前始终需要以某种方式进行身份验证（通过 API 密钥或 Turnstile，如果已启用）。

值为数字，`0` 或 `1`。

## service-specific
[*跳转到表格*](#service-specific-vars)

### CUSTOM_INNERTUBE_CLIENT
将替代默认客户端使用的 innertube 客户端。

值为字符串。

### YOUTUBE_SESSION_SERVER
[yt-session-generator](https://github.com/imputnet/yt-session-generator) 实例的 URL。用于自动为 YouTube 拉取 `poToken` 和 `visitor_data`。可以是本地或远程。

值为一个 URL。

### YOUTUBE_SESSION_INNERTUBE_CLIENT
与 botguard 的（Web）`poToken` 和 `visitor_data` 兼容的 innertube 客户端。

值为字符串。

### YOUTUBE_PLAYER_ID
用于 YouTube 获取的播放器 ID 逗号分隔列表。
如果定义，cobalt 会在每次客户端初始化时选择其中一个，否则默认使用当前最新的播放器 ID。

值为字符串。

### YOUTUBE_ALLOW_BETTER_AUDIO
设置为 `1` 时，如果用户通过 `youtubeBetterAudio` 请求，cobalt 将尝试使用更高质量的音频。这会对带有会话的辅助 YouTube 客户端的速率限制产生负面影响。

值为数字，`0` 或 `1`。

### ENABLE_DEPRECATED_YOUTUBE_HLS
值为字符串：`never`（默认）、`key` 或 `always`：
- 当变量未定义或设置为 `never` 时，POST 请求中的 `youtubeHLS` 将被忽略。
- 当设置为 `key` 时，只有来自 API 密钥客户端的请求才能在 POST 请求中使用 `youtubeHLS`。
- 当设置为 `always` 时，所有请求都能在 POST 请求中使用 `youtubeHLS`。
