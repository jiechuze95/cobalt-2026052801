<script lang="ts">
    import env from "$lib/env";
    import { t } from "$lib/i18n/translations";

    import SectionHeading from "$components/misc/SectionHeading.svelte";
</script>

<section id="general">
<SectionHeading
    title={$t("about.heading.general")}
    sectionId="general"
/>

cobalt 的隐私政策很简单：我们不会收集或存储任何关于你的信息。
你所做的完全是你自己的事，与我们或其他任何人无关。

这些条款仅适用于使用官方 cobalt 实例时。
在其他情况下，你可能需要联系实例托管者以获取准确信息。
</section>

<section id="local">
<SectionHeading
    title={$t("about.heading.local")}
    sectionId="local"
/>

使用设备本地处理的工具在离线状态下工作，完全在本地运行，
不会将任何处理后的数据发送到任何地方。
在适用时，它们会被明确标注。
</section>

<section id="saving">
<SectionHeading
    title={$t("about.heading.saving")}
    sectionId="saving"
/>

使用保存功能时，cobalt 可能需要代理或重新封装/转码文件。
如果是这种情况，会为此目的创建一个临时隧道，
并在 90 秒内存储有关媒体的最少必要信息。

在未经修改的官方 cobalt 实例上，
**所有隧道数据都使用只有最终用户才能访问的密钥进行加密**。

加密的隧道数据可能包括：
- 来源服务的名称。
- 媒体文件的原始 URL。
- 用于区分不同处理类型的内部参数。
- 最少的文件元数据（生成的文件名、标题、作者、创建年份、版权信息）。
- 可能用于隧道过程中 URL 失败情况的原始请求的最少信息。

这些数据在 90 秒后从服务器的 RAM 中不可逆地清除。
只要 cobalt 的源代码未被修改，没有人可以访问缓存的隧道数据，即使是实例所有者也是如此。

隧道中的媒体数据从不在任何地方存储/缓存。
一切都是实时处理的，即使在重新封装和转码过程中也是如此。
cobalt 隧道的功能类似于匿名代理。

如果你的设备支持本地处理，
则加密的隧道信息包含的信息会少得多，因为它会返回给客户端。

参阅 [GitHub 上的相关源代码](https://github.com/imputnet/cobalt/tree/main/api/src/stream)
了解更多关于其工作原理的信息。
</section>

<section id="encryption">
<SectionHeading
    title={$t("about.heading.encryption")}
    sectionId="encryption"
/>

临时存储的隧道数据使用 AES-256 标准进行加密。
解密密钥仅包含在访问链接中，从不被记录/缓存/存储在任何地方。
只有最终用户才能访问链接和加密密钥。
密钥是为每个请求的隧道唯一生成的。
</section>

{#if env.PLAUSIBLE_ENABLED}
<section id="plausible">
<SectionHeading
    title={$t("about.heading.plausible")}
    sectionId="plausible"
/>

我们使用 [Plausible](https://plausible.io/) 来获取活跃 cobalt 用户的大概数量，
完全匿名。绝不会存储任何关于你或你的请求的可识别信息。
所有数据都是匿名化和汇总的。
我们自行托管和管理 cobalt 使用的 [Plausible 实例](https://{env.PLAUSIBLE_HOST}/)。

Plausible 不使用 cookies，完全符合 GDPR、CCPA 和 PECR 的规定。

如果你希望退出匿名分析，可以在[隐私设置](/settings/privacy#analytics)中进行操作。
如果你选择退出，Plausible 脚本将完全不会被加载。

[了解更多关于 Plausible 对隐私的承诺](https://plausible.io/privacy-focused-web-analytics)。
</section>
{/if}

<section id="cloudflare">
<SectionHeading
    title={$t("about.heading.cloudflare")}
    sectionId="cloudflare"
/>

我们使用 Cloudflare 服务来：
- DDoS 和滥用防护。
- 机器人防护（Cloudflare Turnstile）。
- 托管和部署静态渲染的网页应用（Cloudflare Workers）。

这些都是为所有人提供最佳体验所必需的。
Cloudflare 是我们所知的所有上述解决方案中最注重隐私且最可靠的提供商。

Cloudflare 完全符合 GDPR 和 HIPAA 的规定。

[了解更多关于 Cloudflare 对隐私的承诺](https://www.cloudflare.com/trust-hub/privacy-and-data-protection/)。
</section>
