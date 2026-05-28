<script lang="ts">
    import { t } from "$lib/i18n/translations";
    import SectionHeading from "$components/misc/SectionHeading.svelte";
</script>

<section id="general">
<SectionHeading
    title={$t("about.heading.general")}
    sectionId="general"
/>

这些条款仅适用于使用官方 cobalt 实例时。
在其他情况下，你可能需要联系实例托管者以获取准确信息。
</section>

<section id="saving">
<SectionHeading
    title={$t("about.heading.saving")}
    sectionId="saving"
/>

保存功能简化了从互联网下载内容的过程，
我们对保存的内容的使用方式不承担任何责任。

处理服务器像高级代理一样运作，从不将任何请求的内容写入磁盘。
一切都由内存处理，在隧道完成后永久清除。
我们没有下载日志，无法识别任何人。

你可以在[隐私政策](/about/privacy)中了解更多关于隧道工作原理的信息。
</section>

<section id="responsibility">
<SectionHeading
    title={$t("about.heading.responsibility")}
    sectionId="responsibility"
/>

你（最终用户）对如何使用我们的工具、如何使用和分发生成的内容负责。
在使用他人内容时请注意尊重原创者并始终标注出处。
确保你没有违反任何条款或许可证。

用于教育目的时，请始终引用来源并标注原创者。

合理使用和标注出处对每个人都有益。
</section>

<section id="abuse">
<SectionHeading
    title={$t("about.heading.abuse")}
    sectionId="abuse"
/>

由于 cobalt 完全匿名，我们无法自动检测滥用行为。
但是，你可以通过电子邮件向我们报告此类活动，我们会尽力手动处理：abuse[at]imput.net

**此邮箱不用于用户支持，如果你的问题与滥用无关，你不会收到回复。**

如果你遇到问题，可以通过[社区页面](/about/community)上任何首选方式寻求支持。
</section>
