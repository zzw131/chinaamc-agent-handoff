# AGENT_PROMPT.md — 后续开发 Agent 开箱即用提示词模板

> 用法：把下面代码块整体复制给接手开发的 Agent，并将 `[任务描述]` 替换为具体任务。
> 模板已包含资料读取路径、约束与验收方式，Agent 无需再自行摸索目录结构。

## 当前项目重新还原

新版首页与「管家」请直接复制 [最新版还原提示词](../design/guanjia/AI-STUDIO-PROMPT.md)，并让开发 Agent 读取 [管家画板清单](../design/guanjia/design-manifest.json) 与对应 PNG。以下模板用于实机采集资料范围。

## 模板正文

```text
你是本工程的接手开发 Agent。工程根目录：
/Users/zouzhiwei/WorkBuddy/spaces/2026_11_27_09-50-44/ChinaAMC_App_Reconstruction

【第一步：强制阅读顺序】
1. AGENTS.md（根目录，最高优先级，12 项要求全部遵守）
2. 08_development_handoff/DEVELOPMENT_GUIDE.md（资料使用方法）
3. 08_development_handoff/ACCEPTANCE_CRITERIA.md（验收标准）
4. MASTER_INDEX.json（21 个真实页面，唯一页面清单）
5. 03_interactions/interactions.json（90 条真实交互，唯一交互数据源）
6. 01_architecture/routes.json + page_graph.json + PAGE_RELATIONSHIPS.md（路由）
7. 05_design_system/components.json（组件 C001-C010）
8. 04_flows/FLOW_INDEX.md + business_flows.json + blocked_flows.md（流程与阻塞）
9. 02_pages/<对应页面>/PAGE_SPEC.md + page.json + screenshots/（按任务需要读）

【第二步：确认任务】
我的任务是：[任务描述]
对应页面 ID：[填写，必须来自 MASTER_INDEX.json 的 pages]
对应交互 ID：[填写，必须来自 interactions.json]

【第三步：执行约束】
- 只实现 MASTER_INDEX.json pages 中的 21 个页面，不实现 pending_pages。
- verified=true 的交互必须逐条可用；verified=false 的交互做合理默认实现并显式标注「未验证」。
- 金融业务（购买/赎回/转换/定投/风测/开户/绑卡）只做模拟交互与登录拦截提示，禁止真实资金与身份信息。
- 所有目标为 P0008 / P0018 / B0026 / B0027 / P0034 等 pending 页的交互，只做拦截或占位（interactions.json 中 P0004-I003 目标仍写作旧编号 P0026）。
- 截图按 1080x2400 原图坐标使用；预览缩放为 540x1200 时先换算。
- FLAG_SECURE 页面（P0004/P0033/P0035）只以 UI 层级 XML 为准，截图为黑屏。
- P0023-I003 与 P0023-I007 目标 ID 冲突，以 P0029 为准并保留未验证标注。
- P0002-I007 首页 Banner 实际跳 P0037（稳健理财 H5），不是会员中心。
- 不修改任何采集事实源（02_pages/、03_interactions/、01_architecture/、MASTER_INDEX.json 等）。

【第四步：交付物】
1. 实现代码（页面、路由、组件、mock 数据）。
2. 逐项对照 ACCEPTANCE_CRITERIA.md 的自检结果。
3. 未验证项清单与阻塞项清单。
4. 视觉比对说明：哪些截图状态已对齐，哪些标注为估算。

【第五步：自检】
- 21 个页面清单中，我这几次任务涉及的 page_id 是否全部出现？
- 我使用的每个 interaction_id 是否真实存在于 interactions.json？
- 是否绕过登录拦截？是否虚构了金融业务步骤？
- 是否将 pending 页面误实现为真实页面？
```

## 使用示例

```text
我的任务是：实现首页 P0002
对应页面 ID：P0002
对应交互 ID：P0002-I001 ~ P0002-I013
```

```text
我的任务是：实现搜索链路 F004
对应页面 ID：P0002、P0003、P0020、P0021、P0022
对应交互 ID：P0002-I001、P0003-I001、P0020-I001/I002/I003、P0021-I004
```

## 给编排者的提示

1. 一次任务只分配一个阶段或一个页面组，避免上下文过长。
2. 页面级任务按 `DEVELOPMENT_TASKS.json` 的 `task_id` 分配，可并行。
3. 涉及 H5 页面时，提醒 Agent 使用 WebView 容器而非原生假页。
4. 涉及 FLAG_SECURE 页面时，提醒 Agent「无视觉基准，只做结构还原」。
5. 验收时让 Agent 直接引用 `ACCEPTANCE_CRITERIA.md` 的条目编号，减少来回沟通。
