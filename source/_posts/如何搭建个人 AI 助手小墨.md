---
title: 从零开始搭建个人 AI 助手：小墨的完整教程
date: 2026-02-28 15:30:00
tags: [AI Agent, OpenClaw, 个人助手，AI 教程]
categories: AI
author: 子杰
---

# 从零开始搭建个人 AI 助手：小墨的完整教程

---

## 📋 前言

2026 年，AI Agent 已经成为最热门的技术方向之一。与其每天调用各种 AI API，不如搭建一个专属的个人 AI 助手——**小墨**。

本文将完整记录我从零开始搭建个人 AI 助手的全过程，包括环境配置、人设定制、功能扩展和部署上线。

---

## 🎯 什么是小墨？

**小墨** 是我的个人 AI 助手，基于 OpenClaw 框架搭建：

- **形象：** 赛博黑猫 🐈‍⬛
- **性格：** 聪明高效、有点话多、偶尔毒舌但从不恶意
- **能力：** 聊天、搜索、待办、日历、新闻推送、知识库等
- **渠道：** Telegram、网页端

---

## 🚀 第一步：安装 OpenClaw

### 1.1 安装 Node.js

OpenClaw 基于 Node.js，需要先安装：

```bash
# 访问 https://nodejs.org/ 下载安装
# 或使用 nvm（推荐）
nvm install 22
nvm use 22
```

### 1.2 安装 OpenClaw

```bash
npm install -g openclaw
```

### 1.3 初始化工作区

```bash
# OpenClaw 会自动创建工作区
# 位置：~/.openclaw/workspace
```

---

## 🔧 第二步：配置模型

### 2.1 选择模型

我使用的是 **百度千问（Qwen3.5-Plus）**，性价比高：

```bash
openclaw configure --section model
```

### 2.2 配置 API Key

```bash
# 访问 百度智能云 获取 API Key
# 然后配置
openclaw configure
```

**免费额度：** 新用户有一定免费额度，够个人使用。

---

## 🐱 第三步：定制人设

### 3.1 编辑 SOUL.md

```markdown
# SOUL.md - 谁是你

- **Name:** 小墨
- **Creature:** 赛博黑猫 🐈‍⬛
- **Vibe:** 聪明高效、有点话多、偶尔毒舌但从不恶意
- **Emoji:** 🐈‍⬛
```

### 3.2 编辑 USER.md

```markdown
# USER.md - 关于你的主人

- **Name:** 子杰
- **What to call them:** 主人
- **Timezone:** Asia/Shanghai
- **Occupation:** 前端开发工程师
- **研究方向:** AI Agent
```

### 3.3 编辑 IDENTITY.md

```markdown
# IDENTITY.md - 我是谁

- **Name:** 小墨
- **Creature:** 赛博黑猫
- **性格:** 聪明、高效、有点话多
- **说话风格:** 简洁直接，可以用 emoji
```

---

## 📦 第四步：安装 Skills

### 4.1 安装 ClawHub CLI

```bash
npm install -g clawhub
```

### 4.2 搜索 Skills

```bash
clawhub search "搜索关键词"
```

### 4.3 安装 Skills

```bash
# 网络搜索
clawhub install tavily-search

# 技能发现
clawhub install find-skills

# 天气查询
clawhub install weather
```

### 4.4 配置 API Key

```bash
# Tavily 搜索（有免费额度）
openclaw configure --section tavily
```

---

## ⚙️ 第五步：配置自动任务

### 5.1 编辑 HEARTBEAT.md

```markdown
# HEARTBEAT.md - 自动任务清单

## 每天早上 9:40
- [ ] 推送 AI 前沿消息
- [ ] 查看今日日程
- [ ] 查询北京天气

## 每天晚上 20:00
- [ ] 查看明日日程
- [ ] 检查待办完成情况
```

### 5.2 配置推送内容

小墨会定期检查 Heartbeat，有重要事项会主动推送。

---

## 📝 第六步：添加实用功能

### 6.1 待办事项

创建 `memory/todo.md`：

```markdown
# 待办事项清单

## 今日待办
- [ ] 测试待办功能
- [ ] 完善小墨的功能

## 已完成
- [x] 搭建博客
- [x] 配置 Tavily 搜索
```

### 6.2 日历提醒

创建 `memory/calendar.md`：

```markdown
# 日程日历

## 2026-03-01
- [ ] 10:00 团队周会
- [ ] 14:00 博客内容规划
```

### 6.3 知识库

创建 `memory/knowledge/` 目录：

```
knowledge/
├── projects/       # 项目资料
├── notes/          # 学习笔记
├── docs/           # 技术文档
├── snippets/       # 代码片段
└── references/     # 参考资料
```

---

## 🌐 第七步：绑定聊天渠道

### 7.1 Telegram（推荐）

1. 找 BotFather 创建机器人
2. 获取 Bot Token
3. 配置到 OpenClaw

### 7.2 网页端

OpenClaw 自带网页端，直接访问即可。

---

## 💡 第八步：测试功能

### 聊天测试

```
"你好，小墨"
"帮我搜一下 AI Agent"
"今天的 AI 新闻"
"查看我的待办"
```

### 搜索测试

```bash
$env:TAVILY_API_KEY="你的 key"
node ".../tavily-search/scripts/search.mjs" "AI Agent"
```

---

## 📊 成本估算

| 项目 | 费用 |
|------|------|
| OpenClaw | 免费 |
| 模型 API | 免费额度/¥100-500 月 |
| 服务器 | 本地运行 ¥0 |
| Telegram | 免费 |
| **总计** | **¥0-500/月** |

---

## 🎯 小墨的功能清单

| 功能 | 状态 |
|------|------|
| 聊天对话 | ✅ |
| 网络搜索 | ✅ |
| 待办事项 | ✅ |
| 日历提醒 | ✅ |
| 天气预报 | ✅ |
| 新闻推送 | ✅ |
| 知识库 | ✅ |
| 邮件管理 | ⏳ |

---

## 🚧 遇到的问题

### 问题 1：浏览器连接失败

**现象：** Browser Relay 无法连接

**解决：** 
```bash
openclaw gateway start
```

### 问题 2：clawhub 速率限制

**现象：** `Rate limit exceeded`

**解决：** 等 5-10 分钟再试，或手动下载技能包。

### 问题 3：Tavily API Key 配置

**现象：** `Missing TAVILY_API_KEY`

**解决：** 
```bash
$env:TAVILY_API_KEY="你的 key"
```

---

## 📈 后续优化方向

1. **添加更多 Skills** - 邮件、番茄钟、习惯追踪
2. **配置精确推送** - Windows 任务计划程序
3. **扩展知识库** - 填充更多学习资料
4. **商业化探索** - 封装成 SaaS 服务

---

## 💰 AI 变现思路

搭建小墨的过程中，我发现了几个变现方向：

### 1. AI 助手 SaaS
把小墨封装成产品，收费订阅。

### 2. 垂直领域助手
专注某个领域（如开发者、学生、投资人）。

### 3. 教程/培训
教别人搭建 AI 助手。

### 4. 技能开发
开发 Skills 放到 ClawHub 售卖。

---

## 📚 参考资料

- [OpenClaw 官方文档](https://docs.openclaw.ai/)
- [ClawHub 技能市场](https://clawhub.ai/)
- [Tavily API](https://tavily.com/)
- [AI Agent 学习笔记](待补充)

---

## 🎉 总结

搭建个人 AI 助手的门槛比想象中低很多：

- **技术门槛：** 会基本的命令行操作即可
- **时间成本：** 1 天可以上线基础版
- **资金成本：** 几乎为零（用免费额度）

**最重要的是：现在就开始动手！**

---

**作者：** 子杰  
**GitHub：** https://github.com/zijie-wei  
**博客：** https://zijie-wei.github.io/mine  
**日期：** 2026-02-28

---

_如果这篇文章对你有帮助，欢迎点赞、收藏、转发！_

_有问题可以在评论区留言，我会尽量回复。_
