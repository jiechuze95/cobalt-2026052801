# cobalt web
cobalt 前端是一个使用
[sveltekit](https://kit.svelte.dev/) + [vite](https://vitejs.dev/) 构建的静态 Web 应用。

## 配置
- 要运行开发环境，请执行 `pnpm run dev`。
- 要构建前端的发布版本，请执行 `pnpm run build`。

## 环境变量
前端有几个用于配置各种功能的构建时环境变量。要使用它们，你必须在构建前端（或运行 vite 开发服务器）时指定它们。

`WEB_DEFAULT_API` 是运行 cobalt 前端**必需的**。

| 名称                            | 示例                        | 描述                                                                                                    |
|:--------------------------------|:----------------------------|:--------------------------------------------------------------------------------------------------------|
| `WEB_HOST`                      | `cobalt.tools`              | 前端将运行的域名。用于元标签和配置 plausible。                                                           |
| `WEB_PLAUSIBLE_HOST`            | `plausible.io`*             | 启用 plausible 分析，使用提供的主机名作为接收后端。                                                      |
| `WEB_DEFAULT_API`               | `https://api.cobalt.tools/` | 更改前端客户端用于 api 请求的 url。                                                                      |
| `ENABLE_DEPRECATED_YOUTUBE_HLS` | `true`                      | 启用 YouTube HLS 设置条目；允许将相关变量发送到处理实例。                                                 |

\* 除非你已付费使用他们的云服务，否则不要使用 plausible.io 作为接收后端。
   在托管 plausible 社区版时使用你自己的域名。需要时请参阅他们的[文档](https://plausible.io/docs)。

## 链接预填充
要将链接预填充到输入框中并自动开始下载，你可以在 `#` 参数中传递 URL，如下所示：
```
https://cobalt.tools/#https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

链接也可以进行 URI 编码，如下所示：
```
https://cobalt.tools/#https%3A//www.youtube.com/watch%3Fv=dQw4w9WgXcQ
```

## 许可证
cobalt web 代码采用 [CC-BY-NC-SA-4.0](LICENSE) 许可证。

此许可证允许你：
- 以任何媒介或格式复制和再分发代码，以及
- 混合、转换、使用和基于代码构建

只要你：
- 给予原始仓库适当的认可，
- 提供许可证链接并说明是否对代码进行了更改，
- 在**相同许可证**下发布代码，并且
- **不将代码用于任何商业目的**。

仓库中包含的 cobalt 品牌、吉祥物和其他相关资产受***版权保护***，不在许可证覆盖范围内。你***不能***在相同条款下使用它们。

你被允许托管一个带有品牌的***未修改*** cobalt 实例用于**非商业目的**，但这***不***授予你在其他地方使用品牌或以任何方式制作衍生品的许可。

在制作项目的替代版本时，请替换或删除所有品牌（包括名称）。

## 开源致谢
### svelte + sveltekit
cobalt 前端使用 [svelte](https://svelte.dev) 和 [sveltekit](https://svelte.dev/docs/kit/introduction) 构建，这是一个非常高效且出色的框架，我们非常喜欢它。

### libav.js
我们的混流和编码 worker 依赖于 [libav.js](https://github.com/imputnet/libav.js)，这是为浏览器优化的 ffmpeg 构建版本。ffmpeg 构建由许多组件组成，其许可证可以在此处找到：[encode](https://github.com/imputnet/libav.js/blob/main/configs/configs/encode/license.js)、[remux](https://github.com/imputnet/libav.js/blob/main/configs/configs/remux/license.js)。

你可以[在此处支持 ffmpeg](https://ffmpeg.org/donations.html)！

### 字体、图标和资源
cobalt 前端使用了几种不同的字体和图标集。
- [Tabler Icons](https://tabler.io/icons)，在 [MIT](https://github.com/tabler/tabler-icons?tab=MIT-1-ov-file) 许可证下发布。
- [Fluent Emoji by Microsoft](https://github.com/microsoft/fluentui-emoji)，在 [MIT](https://github.com/microsoft/fluentui-emoji/blob/main/LICENSE) 许可证下发布。
- [Noto Sans Mono](https://fonts.google.com/noto/specimen/Noto+Sans+Mono/) 用于下载按钮，在 [OFL](https://fonts.google.com/noto/specimen/Noto+Sans+Mono/about) 许可证下发布。
- [IBM Plex Mono](https://fonts.google.com/specimen/IBM+Plex+Mono/) 用于所有其他文本，在 [OFL](https://fonts.google.com/specimen/IBM+Plex+Mono/license) 许可证下发布。
- 以及 [Redaction](https://redaction.us/) 字体，在 [OFL](https://github.com/fontsource/font-files/blob/main/fonts/other/redaction-10/LICENSE) 许可证下发布（以及 LGPL-2.1）。
- 许多更新横幅取自 [tenor.com](https://tenor.com/)。

### 其他包
- [mdsvex](https://github.com/pngwn/MDsveX) 用于将更新日志转换为 svelte 组件。
- [compare-versions](https://github.com/omichelsen/compare-versions) 用于排序更新日志。
- [svelte-sitemap](https://github.com/bartholomej/svelte-sitemap) 用于为前端生成站点地图。
- [sveltekit-i18n](https://github.com/sveltekit-i18n/lib) 用于以多种不同语言显示 cobalt。
- [vite](https://github.com/vitejs/vite) 用于构建前端。

...以及这些包所依赖的许多其他包。
