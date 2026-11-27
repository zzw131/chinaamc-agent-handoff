# INTERACTION_RULES.md — 交互字段与规则说明

> 本文件解释 `interactions.json` 的字段语义、坐标口径与开发使用规则。

## 字段定义

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | string | 交互唯一 ID，格式 `P<页面ID>-I<三位序号>` |
| `page` | string | 所属页面 ID，关联 `MASTER_INDEX.json` |
| `name` | string | 元素名称（来自截图/UI 层级观察） |
| `type` | string | 触发方式：单击/输入/上下滑动/左右滑动等 |
| `cond` | string | 触发前置条件（如「未勾选协议时」） |
| `result` | string | 操作后观察到的结果；含「未验证」标记的为推测 |
| `target` | string | 目标页面 ID 或目标页面状态 |
| `verified` | boolean | `true`=实际点击并观察结果；`false`=未验证 |
| `bounds` | string | 点击热区，格式 `[left,top][right,bottom]`，原始截图像素坐标 |

## 坐标口径

- 原点：截图左上角 (0,0)。
- 单位：原始物理像素（1080×2400 基准截图）。
- 归一化：x/width ÷ 1080，y/height ÷ 2400。
- 来源：`uiautomator dump` 的实测 bounds；H5 页面部分为截图观察估算。
- 长页面中若元素超出首屏，bounds 为其在对应滚动位置截图的坐标。

## 开发使用规则

1. **已验证交互**（`verified=true`）必须 1:1 还原目标页面 / 状态。
2. **未验证交互**（`verified=false`）按合理默认实现，但须显式标注，不得声称已还原。
3. 所有 `target` 指向的页面必须存在于 `MASTER_INDEX.json`；若为阻塞页（P0018 等），按 UNRESOLVED.md 处理。
4. 涉及登录 / 实名 / 资金的交互必须保留登录拦截与安全边界，不得绕过。
5. 点击热区建议按 bounds 实现，并保留适当内边距容差（移动端手指热区 ≥ 44dp）。

## 交互标注图

各页面 `annotations/` 目录中的 `*_交互标注_*.png` 由 bounds 自动生成，彩色矩形框对应交互 ID。
FLAG_SECURE 页面无可见截图，因而无标注图，以 UI 层级 XML 为准。
