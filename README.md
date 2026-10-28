# 我会永远陪着你 · 7.0.0lite

基于 7.0.0 正式版的核心陪伴版本，分支为 `7.0.0lite`。宿主与 Python 包版本使用合法版本号 `7.0.0+lite`。

保留普通私聊与群聊、多账号与人格、记忆与关系、日程与日记、生活状态、主动联系、分段回复、TTS，以及普通图片、音频、视频和文档理解。

本版不包含 Content、Image、Screen、Reality、Together、Game、Live 七项拓展的实现和资源，也不包含陪伴小屋、其他插件页面或页面管理路由。移除了摄像头、本地音频设备、米家等拓展依赖。

## 安装

使用本分支的 ZIP 安装到 AstrBot，插件目录仍为 `astrbot_plugin_private_companion`。Lite 与完整版使用同一插件身份，是替换安装的两个版本。切换版本前用 AstrBot 备份配置与插件资料；不应把两个版本作为两个插件同时加载。

- 分支：<https://github.com/menglimi/astrbot_plugin_private_companion/tree/7.0.0lite>
- 源码包：<https://github.com/menglimi/astrbot_plugin_private_companion/archive/refs/heads/7.0.0lite.zip>
- 支持 AstrBot 4.27.2 及以上。依赖由 `requirements.txt` 声明，使用宿主 Python 环境。

## 开始使用

1. 在 AstrBot 中连接 Bot、选择聊天模型并配置人格。
2. 打开插件配置 → **账号与会话**，填写私聊目标。向 Bot 私聊发送 `companion_bind` 可查看当前平台实例 ID、Bot 账号、参与者；该命令只展示资料。
3. 添加人格映射：逻辑 Bot ID 必须与目标一致；`persona_id` 使用 AstrBot 人格 ID；`role_id` 是稳定的记忆归属。
4. 开启接管，保存并重载插件。未开启或尚未配置完成时，普通消息继续使用 AstrBot 原有流程。
5. 分段、TTS、天气、新闻、戳一戳等在插件配置中设置。记忆、生活与主动联系的高级偏好见 [配置指南](docs/LITE_CONFIGURATION.md)。

如果启动失败，请查看 AstrBot 日志中的 `code` 和“核心偏好未应用”说明。配置条目整体校验、整体写入；不正确的配置不会启动一半的后台任务。

## 聊天功能

`陪伴 帮助`、`陪伴 状态`、`陪伴 日程`、`陪伴 日记`、`陪伴 梦境`、`陪伴 记忆 <线索>`、`陪伴 主动判定`、`陪伴 答疑 <问题>`。

群聊可用 `陪伴群 帮助`、`陪伴群 状态`、`陪伴群 话题`、`陪伴群 黑话 <词>`、`陪伴群 片段 <线索>`。功能受当前会话权限与实际可用工具约束。

Lite 的高级配置通过 AstrBot 官方配置入口完成；完整版的图形统计、批量清理、数据迁移向导不在此版本中。已有核心数据保留，冻结旧资料不会由 Lite 安装流程迁移或清理。

## 开发与验证

所有文本使用 UTF-8 无 BOM、LF。业务代码在 `companion_v2/`，宿主适配在 `astrbot_host/`，根目录仅 `main.py` 作为入口。

```sh
python -X utf8 tools/build_v2_release.py --output /tmp/companion-lite-release
python -X utf8 tools/verify_v2_release.py --release /tmp/companion-lite-release --extract-to /tmp/companion-lite-verification --sdk /path/to/astrbot/backend/app --run-tests
```

验证使用实际交付包及隔离资料目录，覆盖聊天、记忆、配置事务、语音与分段、主动联系，以及真实 SDK 的加载、停用、重新启用、热重载与零页面路由。
