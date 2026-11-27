# DEVELOPMENT_GUIDE.md — 开发接手指南

> 面向接手开发 Agent。所有事实以本工程内文件为准，**不要臆测未采集内容**。
> 先读根目录 `AGENTS.md`，再读本指南。

## 0. 一句话原则

截图定视觉、交互表定行为、路由表定跳转、组件表定复用、mock 数据定填充、标注「未验证/blocked」的内容只做占位与拦截提示。

## 1. 如何使用截图

1. 基准参数：`1080 × 2400 px`、`420 dpi`、正常竖屏、状态栏约 `0–65px`、底部手势条约 `2340–2400px`（见 `ENVIRONMENT.md`）。
2. 索引入口：`MASTER_INDEX.json` → `pages[].shots`，或 `01_architecture/PAGE_INDEX.md`。每个页面的 `PAGE_SPEC.md` 第 2 节也列出全部截图与 UI dump。
3. 截图类型：
   - `*_default_*.png`：默认态，作为视觉基准。
   - `*_scroll_*.png`：滚动后状态，用于确认长页面与吸顶行为。
   - `*_tab*.png`：Tab 切换态。
   - `*.xml`：`uiautomator dump`，用于读 RN / 自绘 / FLAG_SECURE 页面文本与 `bounds`。
4. 坐标一律按原图 1080×2400 使用。预览工具常缩放为 540×1200，点击/量测前先换算。
5. FLAG_SECURE 页面（`P0004` 登录、`P0033` 注册、`P0035` 找回密码）截图为黑屏，**只能以 UI 层级 XML 为准**，不得凭空编造视觉样式。
6. 截图与 UI 层级统一在 `02_pages/<页面目录>/screenshots/`，直接以此为准。
7. 已知干扰：`P0001` 含 System UI 弹窗异常截图，非 App 页面，不要还原为产品界面。

## 2. 如何查交互

1. 唯一数据源：`03_interactions/interactions.json`（90 条）。
2. 字段含义：`id`、`page`、`name`、`type`（单击/输入/滑动）、`cond`（触发条件）、`result`（结果）、`target`（目标 page_id）、`verified`（是否验证）、`bounds`（热区，`[left,top][right,bottom]`）。
3. 按页面过滤：`[i for i in interactions if i['page']=='P0002']`。
4. 页面视角：读 `02_pages/<页面>/PAGE_SPEC.md` 第 5 节「交互清单」，已按页面整理好。
5. `verified: false` 的交互：实现合理默认行为即可，但必须在代码注释与验收清单中标注「未验证」，不得冒充已还原。
6. `target` 为 `P0008` / `P0018` / `B0026` / `B0027` / `P0034` 等 pending 页时：只实现登录拦截或占位，不要实现目标页内容（`interactions.json` 中 `P0004-I003` 目标仍写作旧编号 `P0026`）。
7. 热区用于还原点击区域与视觉比例；归一化坐标 = 像素值 / 1080（x、width）与 / 2400（y、height）。

## 3. 如何查路由

1. `01_architecture/routes.json`：21 个页面的 `page_id` / `type` / `entry` / `path` / `login_required`。
2. `01_architecture/PAGE_RELATIONSHIPS.md` 与 `01_architecture/page_graph.json`：页面跳转边（`from_` / `to`）。
3. `MASTER_INDEX.json` 每页的 `source` / `targets`：页面来源与目标。
4. 流程视角：`04_flows/FLOW_INDEX.md` + `04_flows/business_flows.json`，含触发条件、分支、登录拦截点、返回路径。
5. 路由实现约定：
   - `login_required: true` 的页面只有 `P0004`；其他需要登录的入口以「登录拦截」形式跳到 `P0004`，不要直接放行。
   - H5 页（P0007 / P0012 / P0013 / P0022 / P0030 / P0037）用 WebView 容器，返回遵循原生返回栈。
   - 原生 Fragment 页面（P0002 / P0003 / P0005）用底部导航切换。

## 4. 如何查设计系统

1. 已确认存在的主文件：`05_design_system/components.json`（10 个组件，含 `pages`、`variant`、`note`）。
2. 同目录的 `DESIGN_SYSTEM.md` / `design_tokens.json` / `COMPONENTS.md` 为补充设计规范，由并行任务产出；**使用前先确认文件存在**，不存在时以 `components.json` 与各 `PAGE_SPEC.md` 为准。
3. 可观察的硬指标（来自采集记录）：
   - 状态栏高约 65px；标题栏约 56px；底部导航高 131px（y 2206–2337），选中态红色。
   - 搜索框圆角胶囊、高约 84px；首页 Banner ViewPager 高 633px，含指示器。
   - 常规内容左右边距约 32px。
   - 涨红跌绿（中国习惯）。
4. 未采集精确色值/字号的，按截图观感还原并在验收记录标注「估算」，不要声称精确匹配。

## 5. 如何复用组件

`components.json` 中 10 个组件的复用映射：

| 组件 | 名称 | 主要页面 | 复用要点 |
|---|---|---|---|
| C001 | 顶部状态栏+标题栏 | P0001/P0002/P0005/P0023 | 区分原生/RN/H5 三类变体 |
| C002 | 底部导航栏 | P0002/P0003/P0005 | 5 tab，选中态红色，高度 131px |
| C003 | 搜索框 | P0002/P0003/P0020 | 轮播提示词态与输入态分开 |
| C004 | Banner 轮播 | P0002/P0011 | 首页 ViewPager 633px + 指示器 |
| C005 | 活动/功能卡片 | P0002/P0011/P0012/P0013 | 多色渐变 + 文案 + CTA |
| C006 | 列表项（基金/指数） | P0003/P0007/P0020/P0021 | 名称+代码+涨跌幅+净值/PE |
| C007 | 数据表格/热力图 | P0003/P0007/P0022 | 涨红跌绿，排序表头 |
| C008 | 输入框/表单 | P0004/P0005 | 账号输入、账户类型、协议勾选 |
| C009 | 按钮组 | P0004/P0021/P0022 | 红色主按钮 + 描边次按钮 |
| C010 | 信息/风险提示文本 | P0002/P0003/P0007 | 底部小字，带 ⓘ 图标 |

复用规则：
- 组件 ID 用于跨文件追踪，代码中建立 `C00x → 组件实现` 的映射。
- 同一组件在不同页面出现时只实现一份，通过 variant 区分。
- 组件命中的页面以 `components.json` 的 `pages` 为准，不要自行扩展。

## 6. 如何使用 mock 数据

1. 每个 `PAGE_SPEC.md` 第 7 节给出该页面可观察字段的 mock 结构与 `data_source: "observed_from_screenshot"`、`mock: true`。
2. `07_mock_data/` 目录由并行任务产出示例数据（如 `mock_data.json`、`data_schema.json`）；**使用前确认文件存在**，以其中字段为准，不要自行发明字段名。
3. 数据分类：
   - 静态：页面标题、协议文本、备案号。
   - 动态：指数涨跌幅、基金净值、Banner 活动（用 mock 固定值，不要接真实资金接口）。
   - 用户输入：搜索词、账号、验证码（本地模拟，禁止真实短信）。
   - 账户相关：资产、持仓、自选（需登录，未采集真实值，只能使用明确标注的脱敏 mock）。
4. 严禁：真实账号、真实身份、真实银行卡、真实资金数值、伪造验证码成功态。

## 7. 如何做视觉验收

1. 基准：`1080×2400` 截图；用同一分辨率渲染后逐页对比。
2. 检查固定区/滚动区/吸顶/悬浮元素位置：`PAGE_SPEC.md` 第 3 节。
3. 检查组件一致性：C001–C010 的尺寸、圆角、颜色、选中态。
4. 每个已采集 `state` 都要有对应截图比对（如 P0002 的 default / banner_variant / scroll_1~3）。
5. 差异分级：影响布局/可点击性为阻塞级；纯色值/字重细微偏差为可接受并记录。
6. FLAG_SECURE 页面无截图基准，只做结构验收，并在报告中标注「无视觉基准」。

## 8. 如何做交互验收

1. 逐条执行 `interactions.json` 中 `verified: true` 的交互，核对 `result` 与 `target`。
2. `verified: false` 的交互：验证「有合理默认行为」且「有未验证标注」即可，不计为已还原。
3. 登录拦截点必须命中 `P0004`，不得绕过（见 `04_flows/blocked_flows.md`）。
4. 返回行为：原生页返回上一页，H5 内页返回上一 H5，底部导航页面保持 tab 状态。
5. 流程级验收按 `04_flows/FLOW_INDEX.md` 逐条走查 F001–F007。
6. 全局标准见 `08_development_handoff/ACCEPTANCE_CRITERIA.md`。

## 9. 常见坑

- 截图预览被缩放：点击坐标按 1080×2400 换算，优先用 XML 的 `bounds`。
- 冷启动需等待 8–12 秒，否则截到闪屏/白屏。
- P0001 的 System UI 弹窗不是 App 页面。
- `P0023-I003` 与 `P0023-I007` 目标 ID 冲突（P0018 vs P0029），以 P0029 为准并保留未验证标注。
- `B0029`（原 pending 同名 `P0029`）是银行卡管理；已采集 `P0029` 是协议列表，以已采集页为准。
