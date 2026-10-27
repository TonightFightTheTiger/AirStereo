# AirStereo 项目索引

## 项目简介

AirStereo 是 Windows 桌面 AirPlay 发送器，从系统默认输出采集声音并通过局域网发送到兼容音响；当前发布说明以 README 标注的版本为准。HomePod 与 AirPlay 是各自权利人的商标，本项目与 Apple 无关联。

## 关键入口

| 内容 | 位置 |
| --- | --- |
| 当前 WinUI 3 桌面应用 | `winui3/AirStereo.WinUI.csproj`、`winui3/WinUiMainWindow.cs` |
| 应用状态与操作 | `winui3/WinUiController.cs` |
| Windows 输出端点控制 | `src/Audio/LocalOutputMute.cs` |
| AirPlay 发现、协议与会话 | `src/ReceiverCatalog.cs`、`src/Api.cs`、`src/Session/` |
| 构建、验收、发布 | `docs/BUILD.md`、`docs/TESTING.md`、`docs/README.release.md` |
| GitHub 云端 EXE 安装包 | `.github/workflows/windows-installer.yml` |
| 版本记录 | `CHANGELOG.md`、`docs/releases/` |

## 项目约定

- 当前桌面入口是 WinUI 3；`src/Ui` 是历史 WinForms 界面，改动前先确认目标工程是否引用它。
- 音量写入走共享 AirPlay API；设备选择、HomePod 原生立体声对和双独立音响分组走共享路由逻辑。
- 用户配置向后兼容；本机静音必须在停止/退出时恢复原端点的原始状态。
- 功能文档写入 README/CHANGELOG 或 `docs/` 对应页面；硬件行为需标明在真实音响上验收，不把离线自检描述成设备验证。
- 仓库没有额外的 `AGENTS.md` 或 Projects 专用工作流程文件；本索引记录现有目录结构和文档中的约定。

## 当前工作备注

- 增加应用运行期间 Windows 默认输出音量到当前所选 AirPlay 音响的同步，轮询间隔 250 ms；系统音量百分比映射到 AirPlay 目标音量百分比。
- Windows Actions workflow 构建应用并打包 `AirStereo-Setup.exe` 作为短期 artifact，不自动创建 Release。
- 依赖 Windows Core Audio `IAudioEndpointVolume` 读取端点标量音量；需要在 Windows 上用 HomePod mini 实机验证响应、默认输出设备切换及本机静音选项的组合行为。
