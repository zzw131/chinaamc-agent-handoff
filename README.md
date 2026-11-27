# ChinaAMC Agent Handoff

华夏基金管家 App（v7.0.2）真实采集资料 — **供开发 Agent 写代码使用**，不含 APK 与人类向报告。

## 快速开始

1. 先读 `AGENTS.md`（唯一入口，含按需读取表与硬性约束）。
2. 读 `MASTER_INDEX.json` 定位页面 ID。
3. 按任务只读对应文件（见 `AGENTS.md` 第 1 节表格），不要通读全库。

## 目录

```
AGENTS.md                     唯一入口：约束 + 按需读取表 + 字段速查
MASTER_INDEX.json             21 页总索引（唯一事实源）
ENVIRONMENT.md                采集环境（1080×2400 @420dpi）
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
