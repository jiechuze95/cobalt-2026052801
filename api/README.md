# cobalt api
此目录包含 cobalt api 的源代码。它使用 [express.js](https://www.npmjs.com/package/express) 和满满的爱构建而成！

## 运行你自己的实例
如果你想出于任何目的运行自己的实例，请[按照此指南操作](/docs/run-an-instance.md)。
除非你打算为开发/调试目的运行 cobalt，否则我们建议使用 docker compose。

## 访问 api
目前没有公开可用的预托管 api。
如果你想使用 cobalt api，我们建议[部署你自己的实例](/docs/run-an-instance.md)。

你可以在此处阅读 [api 文档](/docs/api.md)。

## 支持的服务
此列表并非最终版本，会随时间不断扩展！
如果所需服务尚未受支持，请随时创建相应的 issue（或 pull request 👀）。

| 服务              | 视频 + 音频   | 仅音频        | 仅视频        | 元数据       | 丰富文件名      |
| :--------         | :-----------: | :--------: | :--------: | :------: | :-------------: |
| bilibili          | ✅            | ✅         | ✅         | ➖         | ➖              |
| bluesky           | ✅            | ✅         | ✅         | ➖         | ➖              |
| dailymotion       | ✅            | ✅         | ✅         | ✅         | ✅              |
| instagram         | ✅            | ✅         | ✅         | ➖         | ➖              |
| facebook          | ✅            | ❌         | ✅         | ➖         | ➖              |
| loom              | ✅            | ❌         | ✅         | ✅         | ➖              |
| newgrounds        | ✅            | ✅         | ✅         | ✅         | ✅              |
| ok.ru             | ✅            | ❌         | ✅         | ✅         | ✅              |
| pinterest         | ✅            | ✅         | ✅         | ➖         | ➖              |
| reddit            | ✅            | ✅         | ✅         | ❌         | ❌              |
| rutube            | ✅            | ✅         | ✅         | ✅         | ✅              |
| snapchat          | ✅            | ✅         | ✅         | ➖         | ➖              |
| soundcloud        | ➖            | ✅         | ➖         | ✅         | ✅              |
| streamable        | ✅            | ✅         | ✅         | ➖         | ➖              |
| tiktok            | ✅            | ✅         | ✅         | ❌         | ❌              |
| tumblr            | ✅            | ✅         | ✅         | ➖         | ➖              |
| twitch clips      | ✅            | ✅         | ✅         | ✅         | ✅              |
| twitter/x         | ✅            | ✅         | ✅         | ➖         | ➖              |
| vimeo             | ✅            | ✅         | ✅         | ✅         | ✅              |
| vk videos & clips | ✅            | ❌         | ✅         | ✅         | ✅              |
| youtube           | ✅            | ✅         | ✅         | ✅         | ✅              |

| 图标    | 含义                  |
| :-----: | :---------------------- |
| ✅      | 支持                    |
| ➖      | 不合理/不可能           |
| ❌      | 不支持                  |

### 附加说明或功能（按服务）
| 服务       | 说明或功能                                                                                                           |
| :--------  | :-----                                                                                                               |
| instagram  | 支持 reels、照片和视频。允许你从多媒体帖子中选择要保存的内容。                                                        |
| facebook   | 仅支持公开可访问的视频内容。                                                                                         |
| pinterest  | 支持照片、gif、视频和故事。                                                                                          |
| reddit     | 支持 gif 和视频。                                                                                                    |
| snapchat   | 支持 spotlights 和故事。允许你从故事中选择要保存的内容。                                                              |
| rutube     | 支持 yappy 和私密链接。                                                                                              |
| soundcloud | 支持私密链接。                                                                                                       |
| tiktok     | 支持带或不带水印的视频、无水印幻灯片图片以及完整（原始）音频。                                                        |
| twitter/x  | 允许你从多媒体帖子中选择要保存的内容。由于当前管理层变动，可能不是 100% 可靠。                                         |
| vimeo      | 音频下载仅适用于 dash。                                                                                              |
| youtube    | 支持视频、音乐和短视频。8K、4K、HDR、VR 和高帧率视频。丰富的元数据和配音。h264/av1/vp9 编解码器。                     |

## 许可证
cobalt api 代码采用 [AGPL-3.0](LICENSE) 许可证。

此许可证允许你修改、分发和将代码用于任何目的，
只要你：
- 在使用或修改代码的任何部分时给予原始仓库适当的认可，
- 提供许可证链接并说明是否对代码进行了更改，并且
- 在**相同许可证**下发布代码

## 开源致谢
### ffmpeg
cobalt 依赖 ffmpeg 进行媒体文件的混流和编码。ffmpeg 绝对非常出色，我们很荣幸能像其他人一样免费使用它。我们认为它应该得到更多的认可。

你可以[在此处支持 ffmpeg](https://ffmpeg.org/donations.html)！

### youtube.js
cobalt 依赖 **[youtube.js](https://github.com/LuanRT/YouTube.js)** 与 youtube 的 innertube api 进行交互，没有这个包就不可能实现。

你可以通过他们 GitHub 页面上列出的各种方式支持开发者！
（链接如上）

### 其他众多依赖
cobalt-api 还依赖于：

- **[content-disposition-header](https://www.npmjs.com/package/content-disposition-header)** 用于简化 `content-disposition` 头部的提供。
- **[cors](https://www.npmjs.com/package/cors)** 用于在 expressjs 中管理跨源资源共享。
- **[dotenv](https://www.npmjs.com/package/dotenv)** 用于从 `.env` 文件加载环境变量。
- **[express](https://www.npmjs.com/package/express)** 作为 cobalt 服务器的基础。
- **[express-rate-limit](https://www.npmjs.com/package/express-rate-limit)** 用于对 api 端点进行速率限制。
- **[ffmpeg-static](https://www.npmjs.com/package/ffmpeg-static)** 用于根据平台获取 ffmpeg 的二进制文件。
- **[hls-parser](https://www.npmjs.com/package/hls-parser)** 用于根据规范解析 HLS 播放列表（非常令人印象深刻的东西）。
- **[ipaddr.js](https://www.npmjs.com/package/ipaddr.js)** 用于解析 IP 地址（用于速率限制）。
- **[nanoid](https://www.npmjs.com/package/nanoid)** 用于为每个请求的隧道生成唯一标识符。
- **[set-cookie-parser](https://www.npmjs.com/package/set-cookie-parser)** 用于解析 cobalt 从某些服务接收到的 cookie。
- **[undici](https://www.npmjs.com/package/undici)** 用于发出 HTTP 请求。
- **[url-pattern](https://www.npmjs.com/package/url-pattern)** 用于将提供的链接与支持的模式进行匹配。
- **[zod](https://www.npmjs.com/package/zod)** 用于锁定 api 请求模式。
- **[@datastructures-js/priority-queue](https://www.npmjs.com/package/@datastructures-js/priority-queue)** 用于排序流缓存以便将来清理（不使用 redis）。
- **[@imput/psl](https://www.npmjs.com/package/@imput/psl)** 作为域名解析器，是我们 [psl](https://www.npmjs.com/package/psl) 的分支。

...以及这些包所依赖的许多其他包。
