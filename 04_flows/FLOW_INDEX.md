# FLOW_INDEX.md — 业务流程索引

> 数据来源：`MASTER_INDEX.json`、`03_interactions/interactions.json`、`01_architecture/PAGE_RELATIONSHIPS.md`、`ENVIRONMENT.md`。
> 所有 `page_id` / `interaction_id` 均为真实采集 ID。标注「未验证 / blocked」的内容禁止在开发中冒充已还原。
> 机器可读版本：`04_flows/business_flows.json`。

## 1. 流程总览

| 流程 ID | 名称 | 入口 | 关键页面 | 登录拦截点 | 验证状态 | 详细文档 |
|---|---|---|---|---|---|---|
| F001 | 首次启动与协议同意 | App 冷启动 | P0001 → P0002 | 无 | partial_verified | [F001_first_launch_agreement.md](./F001_first_launch_agreement.md) |
| F002 | 未登录浏览基金触发登录拦截 | 首页快捷区/底部资产/我的 | P0002、P0005、P0007、P0021 → P0004 | P0002-I003、P0002-I011、P0005-I004、P0005-I005、P0007-I004、P0021-I002 | partial_verified | [F002_browse_to_login_gate.md](./F002_browse_to_login_gate.md) |
| F003 | 登录页到注册与找回密码分支 | 登录拦截或 P0005-I001 | P0004 → P0033 / P0035 | P0004、P0004-I009 | partial_verified | [F003_login_register_forgot.md](./F003_login_register_forgot.md) |
| F004 | 搜索到搜索结果再到指数详情 | 首页/行情页顶部搜索框 | P0002/P0003 → P0020 → P0021 → P0022 | P0021-I001、P0021-I002、P0022-I003 | verified_main_path | [F004_search_to_index_detail.md](./F004_search_to_index_detail.md) |
| F005 | 设置到协议列表再到协议详情 H5 | 我的 → 设置 | P0005 → P0023 → P0029 → P0030 | 无 | partial_verified | [F005_settings_to_agreement_h5.md](./F005_settings_to_agreement_h5.md) |
| F006 | 首页 Banner 到活动 H5 | 首页顶部 Banner | P0002 → P0037 → P0008(阻塞) | P0037-I001（未验证） | partial_verified | [F006_home_banner_activity_h5.md](./F006_home_banner_activity_h5.md) |
| F007 | 指数行情到指数详情 | 底部导航-指数行情 | P0002 → P0003 → P0022 | P0022-I003（未验证） | partial_verified | [F007_index_quote_to_detail.md](./F007_index_quote_to_detail.md) |

## 2. 阻塞流程（未验证，禁止虚构）

汇总文档：[blocked_flows.md](./blocked_flows.md)

| 阻塞 ID | 名称 | 关联页面 | 阻塞原因 |
|---|---|---|---|
| B001 | 基金详情查看 | P0008 / P0034 | 触发登录拦截，需登录账号，涉及真实资金展示 |
| B002 | 我的自选 | P0010 | 触发登录拦截，未验证 |
| B003 | 意见反馈 | P0015 | 触发登录拦截，未验证 |
| B004 | 交易记录 | B0028（旧编号已重命名） | 需登录，未验证 |
| B005 | 银行卡管理 | B0029（原 pending 同名 P0029） | 需登录 + 实名，未验证 |
| B006 | 基金购买/赎回/转换/定投 | P0036 | 需登录 + 实名 + 风测，涉及真实资金，按安全边界停止 |
| B007 | 实名认证/开户/风险测评 | P0038 | 需真实身份信息，按安全边界停止 |
| B008 | 持仓/收益/交易记录（登录后） | P0039 | 需登录，未验证 |
| B009 | 消息通知/客服中心/帮助中心 | P0040 | 多入口需登录，未验证 |
| B010 | 注册提交与短信验证码 | P0033 | 需真实手机号与短信验证码 |
| B011 | 找回密码身份核实 | P0035 | 需真实姓名与证件号码 |
| B012 | 登录提交 | P0004 | 需真实账号/第三方授权；FLAG_SECURE 仅 UI 层级 |

## 3. 流程与页面覆盖关系

| 页面 | 出现于流程 |
|---|---|
| P0001 | F001 |
| P0002 | F001、F002、F004、F006、F007 |
| P0003 | F004、F007 |
| P0004 | F002、F003（拦截目标） |
| P0005 | F002、F003、F005 |
| P0007 | F002 |
| P0011 | 无独立流程（入口已验证：P0002-I004 / P0005-I007，登录入口 P0011-I001） |
| P0012 | 无独立流程（入口已验证：P0002-I005，开启目标投 P0012-I001 未验证） |
| P0013 | 无独立流程（入口已验证：P0002-I006，测算/开启计划未验证） |
| P0020 | F004 |
| P0021 | F002、F004 |
| P0022 | F004、F007 |
| P0023 | F005 |
| P0024 | 无独立流程（入口 P0023-I005 已验证） |
| P0029 | F005 |
| P0030 | F005 |
| P0031 | 无独立流程（入口 P0023-I001 标注 P0030，ID 冲突，未验证） |
| P0032 | 无独立流程（入口 P0023-I006 已验证） |
| P0033 | F003 |
| P0035 | F003 |
| P0037 | F006 |

> P0011 / P0012 / P0013 / P0024 / P0031 / P0032 为已采集但未纳入独立流程的页面：其入口交互已在 `interactions.json` 中登记，开发时按页面规格直接实现即可；不得因此虚构后续业务流程。

## 4. 使用约定

1. 开发顺序按 `08_development_handoff/DEVELOPMENT_PLAN.md` 的 7 阶段推进。
2. 每个流程的 Mermaid 图中，实线表示已观察并验证的跳转，虚线表示未验证或阻塞分支。
3. 流程中出现的 `P0018`、`B0026`、`B0027`、`P0008`、`P0034` 等均为 `MASTER_INDEX.json` 的 `pending_pages`，不是已采集页面；禁止当作真实页面实现（`interactions.json` 中 `P0004-I003` 目标仍写作旧编号 `P0026`）。
4. 金融业务（购买/赎回/风测/开户/绑卡）只实现模拟交互与登录拦截提示，禁止接真实资金、真实身份信息。
