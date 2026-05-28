<script lang="ts">
    import { contacts, docs, partners } from "$lib/env";
    import { t } from "$lib/i18n/translations";

    import SectionHeading from "$components/misc/SectionHeading.svelte";
    import BetaTesters from "$components/misc/BetaTesters.svelte";
</script>

<section id="imput">
<SectionHeading
    title="imput"
    sectionId="imput"
/>

cobalt 由 [imput](https://imput.net/) 用心打造 ❤️

我们是一个只有两个人的小团队，但我们非常努力地开发优秀的软件，造福每一个人。
如果你喜欢我们的工作，请考虑在[捐赠页面](/donate)上支持我们！
</section>

<section id="testers">
<SectionHeading
    title={$t("about.heading.testers")}
    sectionId="testers"
/>

非常感谢我们的测试人员，他们提前测试更新并确保其稳定性。
他们还帮助我们发布了 cobalt 10！
<BetaTesters />

所有链接均为外部链接，指向他们的个人网站或社交媒体。
</section>

<section id="partners">
<SectionHeading
    title={$t("about.heading.partners")}
    sectionId="partners"
/>

cobalt 的部分处理基础设施
由我们的长期合作伙伴 [royalehosting.net]({partners.royalehosting}) 提供！
</section>

<section id="meowbalt">
<SectionHeading
    title={$t("general.meowbalt")}
    sectionId="meowbalt"
/>

meowbalt 是 cobalt 的快速吉祥物，一只非常有表现力的猫，热爱高速互联网。

你在 cobalt 中看到的所有精美的 meowbalt 插画
都由 [GlitchyPSI](https://glitchypsi.xyz/) 创作。
他也是这个角色的原创者。

imput 拥有 meowbalt 角色设计的合法权利，
但不包括 GlitchyPSI 创作的特定插画作品。

我们喜爱 meowbalt，因此需要制定一些规则来保护他：
- 你不能将 meowbalt 的角色设计用于同人创作以外的任何形式。
- 你不能将 meowbalt 的设计或插画用于商业用途。
- 你不能在自己的项目中使用 meowbalt 的设计或插画。
- 你不能以任何形式使用或修改 GlitchyPSI 创作的 meowbalt 插画。

如果你创作了 meowbalt 的同人作品，请在
[我们的 Discord 服务器](/about/community)上分享，我们很乐意看到！
</section>

<section id="licenses">
<SectionHeading
    title={$t("about.heading.licenses")}
    sectionId="licenses"
/>

cobalt API（处理服务器）代码是开源的，采用 [AGPL-3.0]({docs.apiLicense}) 许可证。

cobalt 前端代码采用 [源代码优先](https://sourcefirst.com/)模式，采用 [CC-BY-NC-SA 4.0]({docs.webLicense}) 许可证。

我们不得不将前端设为源代码优先模式，以阻止投机者利用我们的工作牟利，
以及防止他们创建恶意克隆来欺骗用户并损害我们的公众形象。
除了商业用途之外，它遵循与许多开源许可证相同的原则。

我们依赖许多开源库，同时也创建和发布自己的库。
你可以在 [GitHub]({contacts.github}) 上查看完整的依赖列表！
</section>
