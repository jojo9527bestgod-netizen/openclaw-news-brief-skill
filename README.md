# openclaw-news-brief-skill

用于 OpenClaw 的新闻简报技能：一次生成 **国内+国际共 15 条** 中文新闻要点，结构统一、可读性高，适合日更快报。

## 功能

- 总条数固定：15 条
- 覆盖范围：国内与国际都要覆盖（默认国内 8 条 + 国际 7 条，可按提示词调整）
- 每条固定四段：序号（【1·国内】~【15·国际】）、标题、发生了什么、影响/看点
- 支持“昨日窗口”模式（北京时间前一日 00:00-23:59）
- 结尾自动附带统一声明
- 支持检索异常时的“单源快报版”回退策略

## 输出格式（带序号）

```
【1·国内】【新闻标题】
【发生了什么（1-2句）】
【影响/看点（1句）】
【2·国内】【新闻标题】
……
【8·国内】【新闻标题】
……
【9·国际】【新闻标题】
……
【15·国际】【新闻标题】

以上基于公开可核验信源整理，后续以官方最新通报为准。
```

## 当前检索链路

当前版本已接入 **Tavily 候选检索**，再配合权威信源完成核验。

### 执行顺序

```text
Tavily 搜索 → 候选池 → 权威源核验 → 去重/筛选 → 输出 15 条
```

### Tavily 的职责

- 找候选新闻
- 补充来源链接
- 提供初步摘要

### Tavily 不负责

- 直接替代最终成稿
- 替代权威源核验
- 把单一来源结果直接当成已确认事实

## 文件说明

- `skills/news-brief-15/SKILL.md`：技能主说明与执行规则
- `skills/news-brief-15/references/sources.md`：推荐信源分层
- `skills/news-brief-15/references/tavily-workflow.md`：Tavily 检索工作流说明

## 推荐信源策略

1. 第一层（主干）：Reuters / AP / BBC / 新华社 / 央视
2. 第二层（校验）：政府与国际组织官方通报
3. 第三层（补充）：区域与财经媒体，仅作背景说明

## 使用建议

- 需要稳定格式时：直接触发该 skill 生成 15 条简报
- 需要最新资讯时：优先走 Tavily 候选检索 + 权威源核验
- 需要更严格可靠性时：对关键条目启用双源核验
- 需要发布到内容平台时：可在此基础上再做口语化改写

## ⚠️ 常见问题：DuckDuckGo 搜索被拦截

如果运行时报 `DuckDuckGo returned a bot-detection challenge`，说明 OpenClaw 的默认搜索用的是 DuckDuckGo，但被检测为机器人请求。

### 解决方法：配置 Tavily 搜索

在 OpenClaw 全局配置 `~/.openclaw/openclaw.json` 中做两处修改：

**1. 添加 Tavily 插件**
```json
"plugins": {
  "entries": {
    "tavily": {
      "enabled": true,
      "config": {
        "webSearch": {
          "apiKey": "tvly-你的APIKey",
          "baseUrl": "https://api.tavily.com"
        }
      }
    }
  }
}
```

**2. 将默认搜索切换为 Tavily**
```json
"tools": {
  "web": {
    "search": {
      "provider": "tavily"
    }
  }
}
```

然后重启 OpenClaw Gateway：
```bash
openclaw gateway restart
```

Tavily 有免费额度（每月1000次），注册地址：https://tavily.com

## ⚠️ 常见问题：MiniMax 模型超时（LLM idle timeout / Request timed out）

如果日志中报错 `LLM idle timeout (60s)` 或 `Request timed out before a response was generated`，说明模型在推理模式下生成内容太慢，超出了默认超时限制。

### 解决方法：关闭推理思考 + 增大超时

在 OpenClaw 全局配置 `~/.openclaw/openclaw.json` 中：

**1. 在 `agents.defaults` 中添加以下配置**
```json
"agents": {
  "defaults": {
    "timeoutSeconds": 300,
    "thinkingDefault": "off",
    "models": {
      "minimax/MiniMax-M2.7": {
        "params": {
          "thinking": "off"
        }
      }
    }
  }
}
```

**2. 重启 Gateway**
```bash
openclaw gateway restart
```

### 说明

- `timeoutSeconds: 300`：将单次任务超时从默认 120 秒延长到 300 秒
- `thinkingDefault: "off"`：关闭推理思考模式，生成速度更快
- `models["minimax/MiniMax-M2.7"].params.thinking: "off"`：对 MiniMax-M2.7 模型单独关闭推理

## 分支说明

- 当前主分支：`main`
- 旧分支 `openclaw-skill` 已删除
