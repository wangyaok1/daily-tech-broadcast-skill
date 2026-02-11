# 每日科技播报 Skill

从可抓取信息源（新浪科技、IT之家等）拉取当日科技新闻标题，生成简报。**无需 API Key**，适合 OpenClaw/Clawdbot 定时推送或手动触发。

## 用途

- 定时任务：每天固定时间向 Discord/飞书等频道推送「每日科技新闻简报」
- 手动触发：用户说「执行每日科技播报」时生成并返回简报
- Token 消耗低（exec 脚本输出，无需大段网页注入模型）

## 安装

1. 将本目录**整体拷贝**到你的 OpenClaw workspace 的 `skills/` 下，例如：
   ```text
   <workspace>/skills/daily-tech-broadcast/
   ├── README.md
   ├── SKILL.md
   ├── requirements.txt
   └── scripts/
       └── broadcast.py
   ```
2. 依赖：仅用 Python 3 标准库，无需 `pip install`。若需扩展可参考 `requirements.txt`。
3. 确保 `python3.11`（或 `python3`）可用。

## 运行

```bash
cd skills/daily-tech-broadcast/scripts
python3.11 broadcast.py
```

简报输出到 stdout，可直接作为消息内容发送。

## 定时任务配置示例（OpenClaw cron）

在 cron 的 `payload.message` 中使用（替换路径为你的 workspace 路径）：

```text
请使用每日科技播报技能，执行播报并将结果发送到当前频道。使用命令：cd <workspace>/skills/daily-tech-broadcast/scripts && python3.11 broadcast.py 2>&1
```

例如 workspace 为 `/root/clawd` 时：

```text
请使用每日科技播报技能，执行播报并将结果发送到当前频道。使用命令：cd /root/clawd/skills/daily-tech-broadcast/scripts && python3.11 broadcast.py 2>&1
```

保持 `deliver: true` 及正确的 `channel`、`to`，以便结果投递到目标频道。

## 信息源列表（可维护）

| 来源     | URL                     |
|----------|-------------------------|
| 新浪科技 | https://tech.sina.com.cn/ |
| IT之家   | https://www.ithome.com/   |

在 `scripts/broadcast.py` 的 `NEWS_SOURCES` 中可增删或调整顺序。无需 API Key，直接 HTTP 抓取。

## 故障与降级

- 单源失败时继续使用其他源。
- 若全部失败，会输出一段降级提示（仍通过 stdout），便于用户知晓。

## 发布为独立仓库（B）

在 GitHub 上新建仓库后，将本目录**全部内容**（含 `SKILL.md`、`scripts/`、`README.md`、`requirements.txt`、`.gitignore`、`_meta.json`、`.clawhub/origin.json`）推送上去即可。他人可 clone 后拷贝到自己的 workspace `skills/` 使用。

## ClawHub 发布（C）

目录已包含 `_meta.json` 与 `.clawhub/origin.json`，符合 ClawHub 规范。发布到技能市场时，在 ClawHub 侧提交或按文档完成发布流程即可；若支持从 GitHub 仓库 URL 安装，可直接指向上述独立仓库。

## 开放与许可

本 Skill 可单独复制、修改、分发。推送到独立仓库或通过 ClawHub 安装时，保持本目录结构即可。
