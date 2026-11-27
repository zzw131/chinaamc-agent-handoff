# P0002 首页固定素材

请先读 [素材清单](asset-manifest.json)。清单记录每个 PNG 的用途、原稿位置、显示尺寸和来源。所有坐标以 750 × 3540 的首页原稿为准。在 375 px 宽的视口按清单的显示尺寸排版。

| 素材 | 文件位置 | 用法 |
| --- | --- | --- |
| 首页经理形象 | [manager-home@4x.png](characters/manager-home@4x.png) | Figma 图层原样 4× 导出，实存 301 × 419 px。用透明 PNG 原比例显示。 |
| 默认快捷入口 | [icons/shortcuts](icons/shortcuts/) | 从左至右为全部基金、我的自选、会员中心、目标投、养老专区。每张 280 × 280 px，显示为 35 × 35 CSS px。 |
| 轮播末页已观察到的入口 | [亲子账户](icons/shortcuts/family-account@4x.png) · [基金投顾](icons/shortcuts/fund-advisory@4x.png) | 与默认五项分开。 |
| Hot 角标 | [hot-badge@4x.png](icons/shortcuts/hot-badge@4x.png) | 会员中心和目标投共用。按清单作为独立 PNG 叠加在图标右上方。 |
| 消息、搜索、信息精选标记 | [icons/header](icons/header/) | 消息图含圆底与红点。搜索图仅含放大镜。 |
| 底部导航的首页选中态及四个未选中态 | [icons/navbar](icons/navbar/) | 使用清单对应文件。基准显示为 20 × 20 CSS px，文字另排。 |
| 热销榜、收益榜、定投榜 | [icons/ranking](icons/ranking/) | 使用全徽章 PNG，按清单保留图层内部留白。 |

实现时将这些文件复制到项目静态资源目录，保留透明通道、宽高比和原色。画面出现同名元素时直接引用指定路径。禁止用图标库、Emoji、文字或重新生成的图替代这些资产。遇到无法读取的文件，报告清单里的相对路径。快捷入口以 [当前默认截图](references/shortcuts-default.png) 和清单的 `home_shortcuts_current` 为准。只在轮播末页使用 [对应截图](references/shortcuts-carousel-end.png) 中的两个入口。其余定位参考 [P0002 原稿](../../references/screens/home-and-follow/P0002-home.png)。活期通和积分兑好礼保留作早期设计参考。

新增四个图标以用户截图裁切为输入，用内置 ImageGen 修复皇冠与白色 V、目标投字标及弧线、盾牌雨伞、向右上升的图形。统一白色圆角底板及透明外缘，按截图校准图案大小和色彩。Hot 从截图提取。沿用既有全部基金、我的自选、亲子账户及其他固定素材。
