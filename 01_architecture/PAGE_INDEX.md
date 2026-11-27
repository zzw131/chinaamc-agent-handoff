# PAGE_INDEX.md — 页面索引

| 页面ID | 名称 | 模块 | 层级 | 类型 | 状态 | 登录 | 说明文档 |
|---|---|---|---|---|---|---|---|
| P0001 | 启动与隐私协议 | 公共 | 0 | 弹窗/启动页 | captured | 否 | 02_pages/P0001_启动与协议/PAGE_SPEC.md |
| P0002 | 首页 | 首页 | 1 | 原生 Fragment | captured | 否 | 02_pages/P0002_首页/PAGE_SPEC.md |
| P0003 | 指数行情 | 行情 | 1 | 原生 Fragment | captured | 否 | 02_pages/P0003_指数行情/PAGE_SPEC.md |
| P0004 | 登录 | 账户 | 1 | 原生 Activity (FLAG_SECURE) | captured_partial | 是 | 02_pages/P0004_登录/PAGE_SPEC.md |
| P0005 | 我的 | 我的 | 1 | 原生 Fragment | captured | 否 | 02_pages/P0005_我的/PAGE_SPEC.md |
| P0007 | 全部基金 | 基金 | 2 | WebView (H5) | captured | 否 | 02_pages/P0007_全部基金/PAGE_SPEC.md |
| P0011 | 会员中心 | 会员 | 2 | 原生 RN Activity | captured | 否 | 02_pages/P0011_会员中心/PAGE_SPEC.md |
| P0012 | 目标投 | 活动 | 2 | WebView (H5活动) | captured | 否 | 02_pages/P0012_目标投/PAGE_SPEC.md |
| P0013 | 养老专区 | 养老 | 2 | WebView (H5) | captured | 否 | 02_pages/P0013_养老专区/PAGE_SPEC.md |
| P0020 | 搜索 | 搜索 | 2 | 原生 Activity | captured | 否 | 02_pages/P0020_搜索/PAGE_SPEC.md |
| P0021 | 搜索结果 | 搜索 | 3 | 原生 Activity | captured | 否 | 02_pages/P0021_搜索结果/PAGE_SPEC.md |
| P0022 | 指数详情 | 行情 | 3 | WebView (H5) | captured | 否 | 02_pages/P0022_指数详情/PAGE_SPEC.md |
| P0023 | 设置 | 我的 | 2 | 原生 Activity | captured | 否 | 02_pages/P0023_设置/PAGE_SPEC.md |
| P0024 | 关于华夏基金管家 | 我的 | 3 | 原生 Activity | captured | 否 | 02_pages/P0024_关于/PAGE_SPEC.md |
| P0029 | 协议列表 | 我的 | 3 | 原生 Activity | captured | 否 | 02_pages/P0029_协议列表/PAGE_SPEC.md |
| P0030 | 协议详情H5 | 公共 | 4 | WebView (CommonWebActivity) | captured | 否 | 02_pages/P0030_协议详情H5/PAGE_SPEC.md |
| P0031 | 长辈模式 | 我的 | 3 | 原生 Activity | captured | 否 | 02_pages/P0031_长辈模式/PAGE_SPEC.md |
| P0032 | 推送设置 | 我的 | 3 | 原生 Activity | captured | 否 | 02_pages/P0032_推送设置/PAGE_SPEC.md |
| P0033 | 注册 | 账户 | 2 | 原生 Activity (FLAG_SECURE) | captured_partial | 否 | 02_pages/P0033_注册/PAGE_SPEC.md |
| P0035 | 找回密码 | 账户 | 2 | 原生 Activity (FLAG_SECURE) | captured_partial | 否 | 02_pages/P0035_找回密码/PAGE_SPEC.md |
| P0037 | 稳健理财H5 | 活动 | 2 | WebView (CommonWebActivity) | captured | 否 | 02_pages/P0037_稳健理财H5/PAGE_SPEC.md |

## 待处理/阻塞页面

| 页面ID | 名称 | 状态 | 说明 |
|---|---|---|---|
| P0006 | 首页快捷区-全部基金跳转过渡 | duplicate | 与 P0007 合并 |
| P0008 | 基金详情 | blocked | 点击基金触发登录拦截；需登录账号，未验证 |
| P0010 | 我的自选 | blocked | 触发登录拦截；未验证 |
| P0014 | Android 桌面（误触） | duplicate | 返回键退出到桌面，非 App 页面 |
| P0015 | 意见反馈 | blocked | 触发登录拦截；未验证 |
| P0016 | 常见问题 | pending | 未采集 |
| P0017 | 投顾解约 | pending | 未采集 |
| P0018 | 相关协议 | pending | 未采集 |
| B0024 | 关于华夏基金管家（旧编号） | duplicate | 已采集，见 PAGES P0024 |
| B0026 | 注册（旧编号） | duplicate | 已以 UI 结构采集，见 P0033 |
| B0027 | 忘记密码（旧编号） | duplicate | 已以 UI 结构采集，见 P0035 |
| B0028 | 交易记录 | blocked | 需登录，未验证 |
| B0029 | 银行卡管理 | blocked | 需登录+实名，未验证 |
| B0030 | 长辈模式（旧编号） | duplicate | 已采集，见 PAGES P0031 |
| B0031 | 消息推送/个性化设置（旧编号） | duplicate | 已采集为 P0032 |
| P0034 | 基金详情 | blocked | 需登录账号，涉及真实资金展示，未验证 |
| P0036 | 基金购买/赎回/转换/定投流程 | blocked | 需登录+实名+风测，涉及真实资金，按安全边界停止 |
| P0038 | 实名认证/开户/风险测评 | blocked | 需真实身份信息，按安全边界停止，不得虚构 |
| P0039 | 持仓/收益/交易记录（登录后） | blocked | 需登录，未验证 |
| P0040 | 消息通知/客服中心/帮助中心 | blocked | 多入口需登录，未验证 |
