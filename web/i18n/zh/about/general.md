<script lang="ts">
    import { t } from "$lib/i18n/translations";
    import { contacts, docs } from "$lib/env";

    import SectionHeading from "$components/misc/SectionHeading.svelte";
</script>

<section id="summary">
<SectionHeading
    title={$t("about.heading.summary")}
    sectionId="summary"
/>

cobalt 帮助你保存来自你喜爱网站的任何内容：视频、音频、照片或 GIF。只需粘贴链接即可开始使用！

没有广告、追踪器、付费墙或其他烦人的东西。只是一个方便的网页应用，随时随地为你服务。
</section>

<section id="motivation">
<SectionHeading
    title={$t("about.heading.motivation")}
    sectionId="motivation"
/>

cobalt 的创建是为了公共利益，保护人们免受替代下载工具推送的广告和恶意软件的侵害。
我们相信最好的软件是安全、开放且易于使用的。所有 imput 项目都遵循这些基本原则。
</section>

<section id="privacy-efficiency">
<SectionHeading
    title={$t("about.heading.privacy_efficiency")}
    sectionId="privacy-efficiency"
/>

所有对后端的请求都是匿名的，所有关于潜在文件隧道的信息都是加密的。
我们实行严格的零日志政策，不存储或追踪任何关于个人用户的信息。

如果请求需要额外处理，如重新封装或转码，cobalt 会在
你的设备上直接处理媒体文件。这确保了最佳的效率和隐私。

如果你的设备不支持本地处理，则会使用基于服务器的实时处理。
在这种情况下，处理后的媒体会直接流式传输到客户端，而不会存储在服务器磁盘上。

你可以[启用强制隧道](/settings/privacy#tunnel)来进一步增强隐私。
启用后，cobalt 将对所有下载的文件进行隧道传输，而不仅仅是那些需要的文件。
没有人会知道你从哪里下载内容，即使是你的网络服务提供商。
他们只会看到你正在使用一个 cobalt 实例。
</section>

<section id="community">
<SectionHeading
    title={$t("about.heading.community")}
    sectionId="community"
/>

cobalt 被无数艺术家、教育工作者和内容创作者用来做他们热爱的事情。
我们始终与社区保持联系，共同努力让 cobalt 变得更加实用。
欢迎[加入对话](/about/community)！

我们相信互联网的未来是开放的，这就是为什么 cobalt 采用
[源代码优先](https://sourcefirst.com/)模式并且[易于自行托管]({docs.instanceHosting})。

如果你的朋友托管了一个处理实例，只需向他们询问域名并在[实例设置中添加](/settings/instances#community)即可。

你可以随时在 [GitHub]({contacts.github}) 上查看源代码并做出贡献。
我们欢迎所有的贡献和建议！
</section>
