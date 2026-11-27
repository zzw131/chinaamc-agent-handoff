# 华夏基金 App · 管家设计交接

这份资料供 Google AI Studio Build 还原「管家」。包含 8 个功能分支、33 个原尺寸 Figma PNG、长页面局部图、页面清单和可直接粘贴的还原提示词。首页 P0002 以 750 × 3540 px 的最新 Figma 原稿为准。

## 从 AI Studio 开始

1. 解压后打开 [还原提示词](./AI-STUDIO-PROMPT.md)。创建 Build 项目时，移动网页演示选 Web，做原生 Android 再选 Android。
2. 第一轮把提示词和首页三张连续局部图一起提交，先验收首页。局部图按原尺寸裁切，能看清小字号与图标。整页 PNG 用来对照滚动顺序和页面比例。
3. 首页通过后，按照 [页面清单](./design-manifest.json) 按模块补充图片。每次附一组主页面与相关状态，引用文件名，验收后再做下一组。用 [思维导图](./references/mind-map.png) 看全局范围。

| 首轮优先级 | 参考文件 |
| --- | --- |
| 首页首屏 | [01-top.png](./references/detail/P0002-home/01-top.png) |
| 主题配置 | [02-theme.png](./references/detail/P0002-home/02-theme.png) |
| 榜单、品牌及页底导航 | [03-bottom.png](./references/detail/P0002-home/03-bottom.png) |
| 首页全长 | [P0002-home.png](./references/screens/home-and-follow/P0002-home.png) |
| 原图中的活动图及页底品牌裁切 | [活动图](./assets/home/campaign-banner.png) · [页底品牌](./assets/home/footer-brand.png) |

## 接续已有实现

在已有的 AI Studio 项目中，让开发 Agent 以本仓库最新 `master` 的 `design/guanjia/` 为视觉输入，从 P0002 开始重做并逐段截图对照。无法直接读取仓库图片时，下载本目录的三个首页局部 PNG 随提示词一并添加。AI Studio 的 GitHub 导入入口用于已有项目代码。当前仓库提供设计和采集资料，已有实现的代码在其原项目中接续。 [Google AI Studio Build 文档](https://aistudio.google.com/docs/aistudio-build-mode)

## 与 WorkBuddy 对照

P0002 是本轮明确匹配的采集页，当前视觉基准为最新版 Figma 首页。实机首页截图与 XML 保留采集时的布局、尺寸及操作证据。按本次范围重做时，应当优先以新稿的首页入口和导航为准。其他有原稿的管家页面和状态按本目录画板实现。对原稿未覆盖、且任务仍要求实现的页面，参考相应实机采集资料。

## 读图约定

- `references/screens/` 每张图对应一个原稿画板。`references/detail/` 是长图的原尺寸裁切，连续切片可有少量重叠。
- 清单的 `screen` 是页面，`state` 是同一页面的状态，`overlay` 是弹层或提示，`reference` 是历史方案或专项示例。节点链接、尺寸与散列值均在清单中。
- `assets/home/` 是从最新首页 PNG 裁切的两个图像，用于忠实呈现源图中的活动图和品牌标志。图像尺寸与裁切坐标可在设计清单中查看。
- 画板体现视觉和可见状态。点击路径以明确的按钮、文字及本次提示词给定的演示关系为依据。涉及基金交易、账户、二维码的交互在演示中使用静态数据或模拟状态。

原始画板和整理版保留在 Figma。 [管家原稿](https://www.figma.com/design/4uckCBF4HEkhbi4PAgerzV?node-id=862-866) · [整理后的最新版首页](https://www.figma.com/design/4uckCBF4HEkhbi4PAgerzV?node-id=8507-2536) · [管家思维导图](https://www.figma.com/design/4uckCBF4HEkhbi4PAgerzV?node-id=8506-2087)
