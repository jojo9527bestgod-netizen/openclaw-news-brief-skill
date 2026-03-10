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
- 2026-03-02 已完成修复：补装 `@lancedb/lancedb-darwin-x64` 后，`memory_store` + `memory_recall` 验收通过。
- 当前策略：以插件记忆为主；`MEMORY.md` 与 `memory/YYYY-MM-DD.md` 改为关键事项备份，不再默认双写。

## LanceDB Pro 操作铁律（摘录）
- Rule 6（双层记忆存储）
  - 每次重要坑位/经验，必须写两层记忆：
    - 技术层（fact，importance ≥ 0.8）：症状 / 根因 / 修复 / 预防
    - 原则层（decision，importance ≥ 0.85）：决策原则（tag）/ 触发条件 / 执行动作
  - 每次写入后必须立刻 `memory_recall` 用锚点关键词验证可召回；召不回就重写重存。
  - 两层缺一不可；未完成不得切下一主题。
  - 同步更新相关 SKILL.md 防复发。
- Rule 7（LanceDB 卫生）
  - 记忆条目必须短小原子化（<500 字符）；不存整段对话总结、大块文本、重复内容。
  - 优先结构化、带检索关键词。
- Rule 8（重试前先召回）
  - 任意工具失败/重复报错/异常行为，先 `memory_recall`（报错词 + 工具名 + 症状）再重试。
- Rule 10（编辑前确认目标仓库）
  - 处理记忆插件前，先确认目标包（`memory-lancedb-pro` vs 内置 `memory-lancedb`），结合 `memory_recall + 文件搜索`，避免改错仓库。
- Rule 20（插件代码改动后清 jiti 缓存，强制）
  - 修改 `plugins/` 下任意 `.ts` 后，重启前必须执行 `rm -rf /tmp/jiti/`。
  - 仅配置变更可不清缓存。
