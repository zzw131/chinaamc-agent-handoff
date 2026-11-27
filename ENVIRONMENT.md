# ENVIRONMENT.md — 采集环境记录

> 记录时间：2026-11-27 10:08 (GMT+8)
> 采集工程师：自主采集 Agent

## 1. 模拟器环境

| 项目 | 值 | 来源/验证方式 |
|---|---|---|
| AVD 名称 | `capture_phone` | 交接说明 / 模拟器运行中 |
| ADB 设备号 | `emulator-5554` | `adb devices -l` 实测 |
| 型号 | `sdk_gphone64_arm64` | `adb shell getprop ro.product.model` 实测 |
| Android 版本 | 15 (API 35) | `adb shell getprop ro.build.version.release` = 15 |
| 系统镜像 | google_apis / arm64-v8a | 交接说明 |
| 物理分辨率 | 1080 × 2400 px | `adb shell wm size` = Physical size: 1080x2400 |
| 物理显示密度 | 420 dpi | `adb shell wm density` = Physical density: 420 |
| 屏幕比例 | 9:20 (0.45) | 1080/2400 计算 |
| 状态栏 | 显示（高度约 0–65px） | 截图实测 |
| 导航栏 | 手势条（底部约 2340–2400px） | 截图实测 |
| 截图分辨率 | 1080 × 2400 PNG（未缩放） | `adb exec-out screencap -p` 输出实测；用 Python 读取 PNG IHDR 验证 |
| 系统字体缩放 | 默认（未修改） | 未执行任何 font scale 修改 |
| 显示密度 | 未修改（420 dpi 默认） | 未执行 wm density 修改 |
| 屏幕方向 | 固定正常竖屏 | 采集期间未旋转；横屏作为独立状态另测 |

## 2. 采集对象 App

| 项目 | 值 | 来源/验证方式 |
|---|---|---|
| App 官方名称 | 华夏基金管家 | 应用宝官方页 + 官网二维码落地页 |
| 开发商 | 华夏基金管理有限公司 | 应用宝详情页 |
| 包名 | `com.chinaamc.MainActivityAMC` | 官网二维码落地页 `https://m.chinaamc.com/r/app_harmony.html` 源码中的 `a.app.qq.com/o/simple.jsp?pkgname=...`；应用宝详情页标题一致 |
| 同公司鸿蒙包名 | `com.chinaamc.fundmanager` | 官网落地页 `store://appgallery.huawei.com/app/detail?id=...`（本次未采集） |
| 版本号 | 7.0.2 | `adb shell dumpsys package` versionName=7.0.2 |
| versionCode | 17190144 | `adb shell dumpsys package` 实测 |
| minSdk / targetSdk | 21 / 30 | `adb shell dumpsys package` 实测 |
| 安装来源 | 应用宝官方 CDN（官网二维码指向的渠道） | APK URL: `http://imtt.dd.qq.com/sjy.00022/sjy.00004/16891/apk/F326BA3CD9C21F882C096F5C33E6D587.apk?fsname=com.chinaamc.MainActivityAMC_7.0.2.apk` |
| 官网 | https://www.chinaamc.com | 搜索结果 + 抓取验证 |
| 官方 App 二维码 | https://www.chinaamc.com/images/jin_code.jpg?v=1 | 官网首页源码 |
| 采集日期 | 2026-11-27 | 当前时间 |
| APK 副本 | 本交接包不含 APK | 原 APK 79,396,629 bytes；采集来源见上一行安装来源 URL |
| 首次安装状态 | 已 `pm clear` 后冷启动，模拟首次启动 | 采集前执行 `adb shell pm clear` |

## 3. 采集方法与能力

| 能力 | 是否可用 | 说明 |
|---|---|---|
| ADB 截图 | ✅ | `adb exec-out screencap -p`，原始 1080×2400 PNG |
| ADB 输入（点击/滑动/文字） | ✅ | `adb shell input tap/swipe/text/keyevent` |
| UI 层级读取 | ✅ 部分 | `adb shell uiautomator dump` 可用；RN/自绘区域文本缺失，需截图+坐标辅助 |
| Activity 读取 | ⚠️ 部分 | 该 App 大量页面为单 Activity + Fragment/RN，`dumpsys activity` 常无有效 mResumedActivity 输出；以截图与 UI 层级为准 |
| 应用数据/账号 | ❌ 无真实账号 | 未登录状态采集；金融业务流程在提交前停止 |
| 网络 | ✅ | 模拟器可联网，能加载真实行情/资讯数据 |

## 4. 采集约束与固定设置

- 采集期间不修改分辨率、密度、字体缩放、语言、时区。
- 默认正常竖屏；横屏仅作为独立状态单独采集。
- 所有截图为原始 PNG，未压缩、未加水印、未修改。
- 不使用真实身份、银行卡、资金账户；不触发任何真实交易。
- 涉及登录/实名/风测/绑卡等流程：在产生真实业务影响前停止并记录阻塞点。
- 采集到的人员姓名/头像等如属真实个人信息，交付时脱敏并标注。

## 5. 已知环境异常与处理

| 现象 | 处理 | 状态 |
|---|---|---|
| 首次启动出现 "System UI isn't responding" 系统弹窗（遮挡 App 页面） | 点击「Wait」（实际坐标约 (318,1370)）；反复出现时 `adb reboot` 后重试 | 已处理，后续偶发 |
| 冷启动后首屏需等待约 8–12 秒，否则截到闪屏/白屏 | 采集前固定 `sleep 8`，并二次确认截图后再采集 | 已纳入流程 |
| 截图预览常被缩放显示为 540×1200 | 点击坐标一律按 1080×2400 原图换算；优先使用 uiautomator dump 的 bounds | 已纳入流程 |
