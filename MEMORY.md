# MEMORY.md

## User profile
- User is building a new Xiaohongshu account focused on Beijing travel.
- Goal: lead generation for local Beijing team tour services.
- Daily available time: 2-4 hours.

## Working setup
- OpenClaw Browser Relay is installed and connectable from Chrome.
- Mac power settings configured for AC mode: no system sleep, display sleep allowed.

## Operations notes
- ezBookkeeping is deployed locally at http://localhost:8080.
- Language set to Chinese (Simplified).
- A test expense was recorded: 买锅 131.

## Saved preferences
- 回复语言：中文。
- 新闻简报已升级为统一规则：
  - 时间窗：北京时间前一日 00:00-23:59
  - 总数：严格 15 条
  - 覆盖：国内、国际各至少 5 条
  - 格式：带编号；每条仅
    - 【标题】
    - 【发生了什么】
    - 【影响/看点】
  - 语气：克制、事实导向；结尾固定免责声明。
- 记账偏好：默认账户“现金人民币”；分类尽量规范映射（如厨房用品→家居用品）。

## Memory plugin status
- 已安装并启用 `memory-lancedb-pro`（slot 已切换）。
- 关键运维规则：`JINA_API_KEY` 必须放在 `~/.openclaw/.env` 并重启 Gateway，避免服务进程读不到变量。
- 当前发现：插件 embedding 写入出现 `422`，未完全打通前，关键记忆继续双写到 `MEMORY.md` 与 `memory/YYYY-MM-DD.md`。
