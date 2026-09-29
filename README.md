# 心率广播 (HRBroadcast)

## 项目简介

心率广播是一款运行在 Wear OS / ColorOS Watch 上的第三方应用，专为 OPPO Watch 4 Pro 等原本不支持心率广播的手表设计。它通过读取手表的光学心率传感器（PPG），将数据封装成标准的 BLE 心率服务（UUID: `0x180D`），并以蓝牙广播的方式发送，从而让自行车码表、跑步机、Zwift、Keep 等设备实时接收心率数据。

## 功能特性

- 读取手表 PPG 心率传感器数据
- 标准 BLE 心率服务（`0x180D`）与心率测量特征值（`0x2A37`）
- 应用内实时显示：
  - 广播状态（灰色未开启 / 绿色已开启）
  - 当前心率数值（bpm）
  - 已连接客户端数量
- 一键启动 / 停止广播
- 崩溃日志捕获，便于排查问题
- 前台服务 + WakeLock，尽量保持后台运行

## 环境要求

- 手表系统：Wear OS 3 / Android 11 及以上
- 开发环境：Android Studio + Kotlin
- 最低 SDK：API 30 (Android 11)
- 目标 SDK：API 33

## 安装 APK

1. 从本仓库的 Releases 页面下载最新 `app-release.apk`。
2. 通过 ADB 安装到手表：`adb install -r app-release.apk`
3. 首次打开应用，授予 **身体传感器**、**蓝牙广播**、**蓝牙连接** 权限。

## 使用步骤

1. 打开应用，点击 **"启动广播"**。
2. 界面顶部状态变为 **绿色 ● 广播已开启**。
3. 将手表紧贴手腕佩戴，等待几秒即可看到实时心率数值。
4. 在接收端（码表、跑步机、Zwift 等）搜索蓝牙心率设备，选择包含 `0x180D` 服务的设备连接。

> 提示：ColorOS Watch 可能会压制通知显示，但前台服务仍可正常运行。


## 注意事项

1. 心率传感器必须贴合皮肤：PPG 光学传感器在未佩戴时不会产生有效数据。
2. BLE 地址随机化：Android 系统会随机化蓝牙 MAC 地址，接收端应通过 `0x180D` 服务 UUID 识别设备，而非 MAC 地址。
3. 设备名称限制：ColorOS 下广播包中携带设备名称可能触发 `Parcel: Reading a NULL string` 错误，建议接收端按服务 UUID 过滤。
4. 防止后台被杀：建议将应用加入 Doze 白名单：`adb shell dumpsys deviceidle whitelist +com.example.hrbroadcast`

## 常见问题

**Q：为什么搜不到广播？**
A：请确保手表已佩戴并紧贴皮肤，且接收端支持标准蓝牙心率服务（`0x180D`）。部分设备需要手动选择"按服务 UUID 搜索"。

**Q：为什么心率一直显示 `--`？**
A：可能是手表未贴合皮肤，或 ColorOS 限制了后台传感器访问。请保持应用在前台，并确认权限已授予。

**Q：为什么应用会闪退？**
A：应用内已集成崩溃日志捕获，重新打开应用后点击 **"查看上次崩溃"** 可查看具体原因。欢迎提交 Issue 反馈。

## 开源协议

本项目采用 MIT License 开源。你可以自由使用、修改、分发，甚至用于商业用途，只需保留原始版权声明。

## 联系方式

- 作者：你的名字
- 项目地址：[https://github.com/QYCQ-official/oppowatch4pro-HeartRateBroadcast](https://github.com/QYCQ-official/oppowatch4pro-HeartRateBroadcast)

---

如果这个项目对你有帮助，欢迎点一个 ⭐ Star 支持一下！
