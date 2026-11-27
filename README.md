# ChinaAMC Agent Handoff

华夏基金管家 App（v7.0.2）真实采集资料 — **供开发 Agent 写代码使用**，不含 APK 与人类向报告。

## 最新版管家还原

1. 从 [管家设计交接](design/guanjia/README.md) 开始。它包含 8 个分支、33 个 Figma 页面和状态、长图局部及首页素材。
2. 首页 P0002 以 [750 × 3540 px 的 Figma 长稿](design/guanjia/references/screens/home-and-follow/P0002-home.png) 为视觉基准。查看 [AI Studio 还原提示词](design/guanjia/AI-STUDIO-PROMPT.md) 和 [画板清单](design/guanjia/design-manifest.json)。
3. 从 [AGENTS.md](AGENTS.md) 了解证据使用规则。[MASTER_INDEX.json](MASTER_INDEX.json) 保留 v7.0.2 的 21 页实机采集索引。原稿未覆盖且在任务范围内的页面可用相应实机资料补充。

## 目录

```
AGENTS.md                     唯一入口：约束 + 按需读取表 + 字段速查
MASTER_INDEX.json             21 页总索引（唯一事实源）
ENVIRONMENT.md                采集环境（1080×2400 @420dpi）
design/guanjia/                最新管家 Figma 原稿、页面清单、AI Studio 提示词
01_architecture/              routes.json / page_graph.json / Mermaid 图
02_pages/<ID>_*/              page.json + PAGE_SPEC.md + screenshots/ + states/ + annotations/
03_interactions/              interactions.json（90 条交互唯一源）
04_flows/                     business_flows.json + 7 个已观察流程 + 12 个阻塞流程
05_design_system/             design_tokens.json + components.json + 补充文档
06_assets/                    2 张截图裁切参考（非官方素材）
07_mock_data/                 mock_data.json + data_schema.json
08_development_handoff/       DEVELOPMENT_TASKS.json（17 个可并行任务）+ 指南 + 验收标准
```

## 关键数字

- 已采集页面 21（18 完整 + 3 FLAG_SECURE 部分采集）
- 交互 90 条（42 已验证 / 48 未验证）
- 截图 47 张 PNG（1080×2400）+ 16 张交互标注图
- 组件 10 个、已观察流程 7 个、阻塞流程 12 个
- 页面覆盖率 61.76%（21/34 已发现范围）、交互验证率 46.67%

## 边界

金融业务只做模拟交互（登录拦截 → 提示），不接真实资金/身份/下单。
`pending_pages` 与 `verified:false` 的交互不实现、不虚构。详见 `AGENTS.md` 与 `ENVIRONMENT.md`。
