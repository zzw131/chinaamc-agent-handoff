# AGENTS.md — 华夏基金管家 App 还原：开发 Agent 唯一入口

> 你是写代码 Agent。本仓库是真实 App（华夏基金管家 v7.0.2，`com.chinaamc.MainActivityAMC`）的采集事实源，不是给人看的设计说明。
> **按本文件索引精确取用，不要通读全库。** 所有 ID、坐标、路径均为实测；标注「未验证」的一律不得当作已还原实现。

---

## 最新管家设计任务

涉及首页或「管家」画板还原时，先读 [设计交接](design/guanjia/README.md)、[还原提示词](design/guanjia/AI-STUDIO-PROMPT.md) 和 [设计清单](design/guanjia/design-manifest.json)。设计清单给出本轮 33 张 Figma 画板及状态；下文 21 页、1080×2400 和实测交互约束用于 v7.0.2 的采集资料。

- P0002 首页当前视觉基准为 [Figma 首页原稿](design/guanjia/references/screens/home-and-follow/P0002-home.png)，尺寸 750×3540。界面文字、导航、入口、版式和滚动顺序按新稿。经理形象及首页图标一律使用 [固定素材清单](design/guanjia/assets/ui/asset-manifest.json) 的 PNG，保留显示尺寸及状态。首页默认快捷入口以清单的 `home_shortcuts_current.default` 为准，依次为全部基金、我的自选、会员中心、目标投、养老专区。
- 其他有 Figma 设计依据的管家页面与状态按设计清单复现。实机截图、XML 与已验证交互用来查采集时的行为及补充未覆盖的页面。实测坐标属于原实机截图。只有触发控件在新稿中仍存在且含义一致时，才沿用对应行为。
- 优先验收新版 P0002 的首屏、滚动和页底；其余模块按提示词分批实现。设计图中示出的界面状态可以作为演示范围，未实测的跳转与业务流程应标明。

## 0. 硬性约束（先读）

1. **技术栈自由**：资料与框架无关；不要假设 React/Vue/Flutter。
2. **视觉基准**：1080×2400 px @ 420dpi，正常竖屏。dp = px / 2.625。见 `ENVIRONMENT.md`。
3. **金融安全**：购买/赎回/转换/定投/开户/风测/绑卡/持仓等只做模拟交互（点击 → 登录拦截 → 提示），禁止接真实资金、身份、下单。
4. **禁止虚构**：`MASTER_INDEX.json → pending_pages` 与 `interactions.json → verified:false` 的内容不实现、不补全、不臆造后续页面。
5. **事实源只读**：`MASTER_INDEX.json`、`01_architecture/`、`02_pages/`、`03_interactions/`、`04_flows/`、`05_design_system/`、`ENVIRONMENT.md` 是采集事实，禁止修改。

---

## 1. 按需读取表（不要全读）

| 你要做的事 | 只读这些文件 |
|---|---|
| 第一次接手，建立全局认知 | 本文件 + `MASTER_INDEX.json`（pages 数组） |
| 建路由 / 页面骨架 | `01_architecture/routes.json`、`page_graph.json` |
| 实现某个页面 | `02_pages/<页面ID>_*/page.json` + 同目录 `PAGE_SPEC.md` + `screenshots/` |
| 实现点击 / 跳转 / 滑动 | `03_interactions/interactions.json`（按 `page` 字段过滤） |
| 实现导航关系 / 返回 | `01_architecture/PAGE_RELATIONSHIPS.md`、`04_flows/business_flows.json` |
| 写样式 / 主题变量 | `05_design_system/design_tokens.json`（机器读）+ `DESIGN_SYSTEM.md`（人读补充） |
| 复用组件 | `05_design_system/components.json` + `COMPONENTS.md` |
| 造数据 | `07_mock_data/mock_data.json` + `data_schema.json`（`MOCK_` 前缀为虚构数据） |
| 领开发任务 | `08_development_handoff/DEVELOPMENT_TASKS.json`（17 个可并行任务） |
| 验收自查 | `08_development_handoff/ACCEPTANCE_CRITERIA.md` |

**关键索引**：`MASTER_INDEX.json` 是总线。每页含 `id / name / module / level / type / spec / shots[] / states[] / components[] / source / targets[] / priority`。先在这里定位页面 ID，再进 `02_pages/` 取详情。

---

## 2. 数据字段速查

**interactions.json**（90 条，唯一交互源）
```
id          P0002-I007 格式（页面ID-I三位序号）
page        所属页面 ID，关联 MASTER_INDEX.pages[].id
type        单击 / 输入 / 上下滑动 / 左右滑动
cond        触发前置条件
result      操作结果
target      目标页面 ID（P0008 为已知阻塞页，不实现）
verified    true=实测；false=未验证（禁止当作已还原）
bounds      点击热区 "[left,top][right,bottom]"，原始像素，原点左上角
```
归一化：x÷1080，y÷2400。热区由 uiautomator 实测；H5 部分为截图估算。

**page.json**（每页一份）：`id/name/module/level/type/entry/login_required/status/priority/description/files/states/components/source/targets/interactions[]`。

**design_tokens.json**：`colors / typography / spacing / radius / border / shadow / opacity / sizes`，每个 token 带 `value / unit / source(measured|estimated) / note`。source=estimated 的不要当精确值。

---

## 3. 截图与标注图

- `02_pages/<页>/screenshots/` — 原始 PNG（1080×2400）+ uiautomator XML。
- `02_pages/<页>/states/` — 状态截图（键盘弹出、展开、滚动等）。
- `02_pages/<页>/annotations/` — 交互热区标注图（彩色框 + 交互 ID），由 bounds 自动生成；**与原始截图分开**。
- `06_assets/` — 仅 2 张截图裁切参考图（`reference_crops`，非原始设计素材），不要当官方切图使用。

FLAG_SECURE 页面（P0004 登录 / P0033 注册 / P0035 找回密码）**无可见截图**，只有 `screenshots/*.xml`（UI 层级）。这三页视觉无法 1:1 还原，用同 App 原生页设计语言实现并在交付说明中标注「视觉未验证」。

---

## 4. 页面状态与实现边界

**已实现范围**：`MASTER_INDEX.pages` 共 21 页，其中 18 页 `captured`、3 页 `captured_partial`（FLAG_SECURE）。

**不实现**：`pending_pages` 中 10 个 `blocked` + 3 个 `pending`（基金详情、自选、交易、持仓、实名、风测、银行卡等）。

**已验证交互仅 42/90**：其余 48 项按合理默认实现但须标注未验证。不要因为交互 ID 存在就认为行为已被确认。

**流程**：`04_flows/` 有 7 个已观察流程（F001–F007）+ 12 个阻塞流程（B001–B012）。只实现 F001–F007 主路径；阻塞流程仅做入口提示。

---

## 5. 验收

按 `08_development_handoff/ACCEPTANCE_CRITERIA.md` 逐项自查。核心：
- 21 页页面 ID / 名称严格一致。
- 42 条已验证交互的目标页面 / 结果严格一致。
- 涨跌色用中国习惯（涨红跌绿）。
- 每页固定区/滚动区/吸顶/悬浮行为与 `PAGE_SPEC.md` 第 3 节一致。
- 未验证项显式标注，不冒充已还原。

---

## 6. 禁止事项

- 不虚构 `pending_pages` 页面、协议正文、服务端接口、真实数据。
- 不接真实资金 / 身份 / 验证码 / 第三方授权。
- 不把 `06_assets/` 的截图裁切参考当官原始素材。
- 不修改事实源（第 0 节第 5 条）。
- 不把 estimated token 当精确设计值。
- 不用 P0008（基金详情）等阻塞页作为可落地跳转目标；点击后按登录拦截处理。
