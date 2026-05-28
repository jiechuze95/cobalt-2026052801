# cobalt api 文档
方法、可接受的值、请求头、响应以及与从 cobalt API 实例发起和解析请求相关的所有内容。

> [!IMPORTANT]
> 托管的 API 实例（如 `api.cobalt.tools`）使用了机器人防护，**不**应在未经明确许可的情况下用于其他项目。如果你想使用 cobalt API，应[托管自己的实例](/docs/run-an-instance.md)或向实例所有者请求访问权限。

- [POST /](#post)
- [POST /session](#post-session)
- [GET /](#get)
- [GET /tunnel](#get-tunnel)

所有端点（`GET /` 除外）都有速率限制，并在 `RateLimit-*` 请求头中返回当前速率限制状态，遵循["RateLimit Header Fields for HTTP" 规范](https://www.ietf.org/archive/id/draft-polli-ratelimit-headers-02.html#name-header-specifications)。

## 身份验证
API 实例可能被配置为要求你进行身份验证。
如果是这种情况，你通常会收到一个[错误响应](#error-response)，
其中包含 **`api.auth.<method>.missing`** 错误码，告诉你需要某种身份验证方式。

身份验证通过传递 `Authorization` 请求头来完成，包含
身份验证方案和令牌：
```
Authorization: <scheme> <token>
```

目前，cobalt 支持两种身份验证方式。实例可以选择
同时配置两者，或都不配置：
- [`Api-Key`](#api-key-authentication)
- [`Bearer`](#bearer-authentication)

### Api-Key 身份验证
API 密钥身份验证是最直接的方式。实例所有者
会为你分配一个 API 密钥，你可以像这样使用它进行身份验证：
```
Authorization: Api-Key aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee
```

如果你是实例所有者并希望配置 API 密钥身份验证，
请参阅[实例](run-an-instance.md#api-key-file-format)文档！

### Bearer 身份验证
cobalt 服务器可以配置为签发 JWT Bearer 令牌，这些是短期令牌，
供普通用户使用（例如通过挑战后）。
目前，cobalt 可以为成功解决的 [turnstile](run-an-instance.md#list-of-all-environment-variables) 挑战签发令牌（如果实例配置了 turnstile）。生成的令牌像这样传递：
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## POST `/`
cobalt 的主要处理端点。

> [!IMPORTANT]
> 每个 `POST /` 请求都必须包含正确的 `Accept` 和 `Content-Type` 请求头。

```
Accept: application/json
Content-Type: application/json
```

### 请求体
内容类型：`application/json`

不想阅读文字表格？
你可以直接从代码中阅读 [API schema](/api/src/processing/schema.js)！

### API schema
除 `url` 外，所有键都是可选的。值选项之间用 `/` 分隔。

#### 通用
| 键                  | 类型      | 描述/值                                                   | 默认值     |
|:-------------------|:----------|:----------------------------------------------------------|:-----------|
| `url`              | `string`  | 源 URL                                                    | *必填*     |
| `audioBitrate`     | `string`  | `320 / 256 / 128 / 96 / 64 / 8` (kbps)                   | `128`      |
| `audioFormat`      | `string`  | `best / mp3 / ogg / wav / opus`                           | `mp3`      |
| `downloadMode`     | `string`  | `auto / audio / mute`                                     | `auto`     |
| `filenameStyle`    | `string`  | `classic / pretty / basic / nerdy`                        | `basic`    |
| `videoQuality`     | `string`  | `max / 4320 / 2160 / 1440 / 1080 / 720 / 480 / 360 / 240 / 144` | `1080`     |
| `disableMetadata`  | `boolean` | 标题、艺术家和其他信息将不会添加到文件中                     | `false`    |
| `alwaysProxy`      | `boolean` | 始终隧道传输所有文件，即使不是必需的                        | `false`    |
| `localProcessing`  | `string`  | `disabled / preferred / forced`                           | `disabled` |
| `subtitleLang`     | `string`  | 任何有效的 ISO 639-1 语言代码                              | *无*       |

#### 服务特定选项
| 键                      | 类型      | 描述/值                                    | 默认值  |
|:------------------------|:----------|:-------------------------------------------|:--------|
| `youtubeVideoCodec`     | `string`  | `h264 / av1 / vp9`                        | `h264`  |
| `youtubeVideoContainer` | `string`  | `auto / mp4 / webm / mkv`                 | `auto`  |
| `youtubeDubLang`        | `string`  | 任何有效的 ISO 639-1 语言代码               | *无*    |
| `convertGif`            | `boolean` | 将 Twitter GIF 转换为实际的 GIF 格式        | `true`  |
| `allowH265`             | `boolean` | 允许来自 TikTok 的 H265/HEVC 视频          | `false` |
| `tiktokFullAudio`       | `boolean` | 下载视频中使用的原始音频                    | `false` |
| `youtubeBetterAudio`    | `boolean` | 如果可能，优先使用更高质量的 YouTube 音频    | `false` |
| `youtubeHLS`            | `boolean` | 从 YouTube 下载时使用 HLS 格式              | `false` |

### 响应
内容类型：`application/json`

响应将始终是一个包含 `status` 键的 JSON 对象，其值为以下之一：
- `tunnel`：cobalt 正在为你代理和/或重混/转码文件。
- `local-processing`：cobalt 正在为你代理文件，但你需要在本地进行重混/转码。
- `redirect`：cobalt 将把你重定向到直接的服务 URL。
- `picker`：有多个项目可供选择，应显示选择器。
- `error`：出错了，这是错误代码。

### tunnel/redirect 响应
| 键           | 类型     | 值                                                       |
|:-------------|:---------|:---------------------------------------------------------|
| `status`     | `string` | `tunnel / redirect`                                      |
| `url`        | `string` | cobalt 隧道的 URL，或重定向到外部链接                      |
| `filename`   | `string` | cobalt 为下载文件生成的文件名                              |

### 本地处理响应
| 键           | 类型       | 值                                                          |
|:-------------|:-----------|:------------------------------------------------------------|
| `status`     | `string`   | `local-processing`                                          |
| `type`       | `string`   | `merge`、`mute`、`audio`、`gif` 或 `remux`                   |
| `service`    | `string`   | 来源服务（`youtube`、`twitter`、`instagram` 等）              |
| `tunnel`     | `string[]` | 隧道 URL 数组                                               |
| `output`     | `object`   | 输出文件详情（[见下文](#output-object)）                     |
| `audio`      | `object`   | 音频特定详情（可选，[见下文](#audio-object)）                |
| `isHLS`      | `boolean`  | 输出是否为 HLS 格式（可选）                                  |

#### output 对象
| 键          | 类型      | 值                                                                              |
|:------------|:----------|:--------------------------------------------------------------------------------|
| `type`      | `string`  | 输出文件的 MIME 类型                                                              |
| `filename`  | `string`  | 输出文件的文件名                                                                  |
| `metadata`  | `object`  | 与文件关联的元数据（可选，[见下文](#outputmetadata-object)）                       |
| `subtitles` | `boolean` | 隧道是否包含字幕文件                                                              |

#### output.metadata 对象
此表中的所有键都是可选的。

| 键             | 类型     | 描述                       |
|:---------------|:---------|:---------------------------|
| `album`        | `string` | 专辑名称或合集标题          |
| `composer`     | `string` | 曲目作曲家                  |
| `genre`        | `string` | 曲目的流派                  |
| `copyright`    | `string` | 版权信息或所有权详情        |
| `title`        | `string` | 曲目或媒体文件的标题        |
| `artist`       | `string` | 艺术家或创作者名称          |
| `album_artist` | `string` | 专辑的艺术家或创作者名称    |
| `track`        | `string` | 曲目在专辑中的编号或位置    |
| `date`         | `string` | 发行日期或创建日期          |
| `sublanguage`  | `string` | 字幕语言代码 (ISO 639-2)   |

#### audio 对象
| 键          | 类型      | 值                                                       |
|:------------|:----------|:---------------------------------------------------------|
| `copy`      | `boolean` | 定义是否复制音频编解码器数据                              |
| `format`    | `string`  | 输出音频格式                                              |
| `bitrate`   | `string`  | 音频格式的首选比特率                                       |
| `cover`     | `boolean` | 隧道是否包含封面图片文件（可选）                           |
| `cropCover` | `boolean` | 封面图片是否应裁剪为正方形（可选）                         |

### picker 响应
| 键              | 类型     | 值                                                                                             |
|:----------------|:---------|:-----------------------------------------------------------------------------------------------|
| `status`        | `string` | `picker`                                                                                       |
| `audio`         | `string` | 当图片幻灯片（如 TikTok）有通用背景音频时返回（可选）                                            |
| `audioFilename` | `string` | cobalt 生成的文件名，如果 `audio` 存在则返回（可选）                                             |
| `picker`        | `array`  | 包含各个媒体项的对象数组                                                                        |

#### picker 对象
| 键           | 类型      | 值                      |
|:-------------|:----------|:------------------------|
| `type`       | `string`  | `photo` / `video` / `gif` |
| `url`        | `string`  |                         |
| `thumb`      | `string`  | 缩略图 URL（可选）       |

### 错误响应
| 键           | 类型     | 值                        |
|:-------------|:---------|:--------------------------|
| `status`     | `string` | `error`                   |
| `error`      | `object` | 错误代码和可选上下文       |

#### error 对象
| 键           | 类型     | 值                                                      |
|:-------------|:---------|:--------------------------------------------------------|
| `code`       | `string` | 说明失败原因的机器可读错误代码                            |
| `context`    | `object` | 附加错误上下文（可选）                                    |

#### error.context 对象
| 键           | 类型     | 值                                                                        |
|:-------------|:---------|:--------------------------------------------------------------------------|
| `service`    | `string` | 来源服务（可选）                                                           |
| `limit`      | `number` | 最大可下载视频时长或速率限制窗口（可选）                                     |

## POST `/session`
用于生成 JWT 令牌（如果已启用）。目前，cobalt 仅在客户端提交 [turnstile](run-an-instance.md#list-of-all-environment-variables) 挑战解决方案时支持生成令牌。

turnstile 挑战响应通过 `cf-turnstile-response` 请求头提交。

### 响应体
| 键              | 类型       | 描述                                             |
|:----------------|:-----------|:-------------------------------------------------|
| `token`         | `string`   | 用于后续请求身份验证的 `Bearer` 令牌               |
| `exp`           | `number`   | 表示令牌有效期的秒数                              |

失败时将返回[错误响应](#error-response)。

## GET `/`
提供基本的实例信息。

### 响应
内容类型：`application/json`

| 键          | 类型     | 描述                                             |
|:------------|:---------|:-------------------------------------------------|
| `cobalt`    | `object` | 关于 cobalt 实例的信息                            |
| `git`       | `object` | 关于当前运行的代码库的信息                        |

#### cobalt 对象
| 键                 | 类型       | 描述                                   |
|:-------------------|:-----------|:---------------------------------------|
| `version`          | `string`   | cobalt 版本                            |
| `url`              | `string`   | 实例 URL                               |
| `startTime`        | `string`   | 实例启动时间（Unix 毫秒）               |
| `turnstileSitekey` | `string`   | turnstile 小部件的站点密钥（可选）      |
| `services`         | `string[]` | 此实例支持的服务数组                    |

#### git 对象
| 键          | 类型     | 描述         |
|:------------|:---------|:-------------|
| `commit`    | `string` | 提交哈希     |
| `branch`    | `string` | git 分支     |
| `remote`    | `string` | git 远程仓库 |

## GET `/tunnel`
用于文件隧道（代理/重混/转码）的端点。响应为文件流。所有错误均通过 [HTTP 状态码](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status)报告。

### 返回的请求头
- `Content-Length`：文件大小，单位为字节。在已知确切最终文件大小时返回。
- `Estimated-Content-Length`：估计的文件大小，单位为字节。在不知道真实 `Content-Length` 时返回。
这是粗略估计，**不应**用于严格大小验证。
可用于在 UI 中显示大致的下载进度。

### 可能的 HTTP 状态码
- 200：OK
- 401：Unauthorized
- 403：Bad Request
- 404：Not Found
- 429：Too Many Requests（超出速率限制，检查 [RateLimit-* 请求头](https://www.ietf.org/archive/id/draft-polli-ratelimit-headers-02.html#name-header-specifications)）
- 500：Internal Server Error
