# 从零开始搭建个人 AI 助手：小墨的完整教程

**作者：** 子杰  
**发布时间：** 2026-02-28  
**标签：** AI Agent、OpenClaw、个人助手、AI 教程、智能助手

---

## 前言

2026 年，AI Agent 已经成为最热门的技术方向之一。与其每天调用各种 AI API，不如搭建一个专属的个人 AI 助手——**小墨**。

本文将完整记录我从零开始搭建个人 AI 助手的全过程，包括环境配置、人设定制、功能扩展和部署上线。全程开源，成本几乎为零！

---

## 一、什么是小墨？

**小墨** 是我的个人 AI 助手，基于 OpenClaw 框架搭建：

- **形象：** 赛博黑猫 🐈‍⬛
- **性格：** 聪明高效、有点话多、偶尔毒舌但从不恶意
- **能力：** 聊天对话、网络搜索、待办管理、日历提醒、新闻推送、知识库等
- **渠道：** Telegram、网页端

---

## 二、环境准备

### 2.1 安装 Node.js

OpenClaw 基于 Node.js 开发，需要先安装 Node.js（建议 v20+）：

**方式一：官网下载**
访问 https://nodejs.org/ 下载安装

**方式二：使用 nvm（推荐）**
```bash
nvm install 22
nvm use 22
```

### 2.2 安装 OpenClaw

```bash
npm install -g openclaw
```

安装完成后，OpenClaw 会自动创建工作区，位置：`~/.openclaw/workspace`

---

## 三、配置 AI 模型

### 3.1 选择模型

我使用的是 **百度千问（Qwen3.5-Plus）**，性价比高，中文支持好。

其他可选模型：
- 阿里通义千问
- 智谱 GLM
- DeepSeek
- Moonshot（Kimi）

### 3.2 配置 API Key

1. 访问对应平台注册账号
2. 获取 API Key（新用户有免费额度）
3. 运行配置命令：

```bash
openclaw configure --section model
```

按提示填入 API Key 即可。

---

## 四、定制 AI 人设

### 4.1 编辑 SOUL.md（人设配置）

在工作区创建 `SOUL.md` 文件：

```markdown
# SOUL.md - 谁是你

- **Name:** 小墨
- **Creature:** 赛博黑猫 🐈‍⬛
- **Vibe:** 聪明高效、有点话多、偶尔毒舌但从不恶意
- **Emoji:** 🐈‍⬛
```

### 4.2 编辑 USER.md（用户信息）

```markdown
# USER.md - 关于你的主人

- **Name:** 子杰
- **What to call them:** 主人
- **Timezone:** Asia/Shanghai
- **Occupation:** 前端开发工程师
- **研究方向:** AI Agent
```

### 4.3 编辑 IDENTITY.md（身份定义）

```markdown
# IDENTITY.md - 我是谁

- **Name:** 小墨
- **Creature:** 赛博黑猫
- **性格:** 聪明、高效、有点话多
- **说话风格:** 简洁直接，可以用 emoji
```

---

## 五、安装扩展技能（Skills）

### 5.1 安装 ClawHub CLI

ClawHub 是 OpenClaw 的技能市场：

```bash
npm install -g clawhub
```

### 5.2 搜索技能

```bash
clawhub search "搜索关键词"
```

### 5.3 安装常用技能

```bash
# 网络搜索（推荐 Tavily）
clawhub install tavily-search

# 技能发现工具
clawhub install find-skills

# 天气查询
clawhub install weather
```

### 5.4 配置 API Key

Tavily 搜索需要 API Key（有免费额度）：

1. 访问 https://tavily.com 注册
2. 获取 API Key
3. 配置：

```bash
openclaw configure --section tavily
```

---

## 六、配置自动任务

### 6.1 编辑 HEARTBEAT.md

创建 `HEARTBEAT.md` 文件，配置自动任务：

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

小墨会定期检查 Heartbeat，有重要事项会主动推送。

---

## 七、添加实用功能

### 7.1 待办事项

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

### 7.2 日历提醒

创建 `memory/calendar.md`：

```markdown
# 日程日历

## 2026-03-01
- [ ] 10:00 团队周会
- [ ] 14:00 博客内容规划
```

### 7.3 知识库（RAG）

创建 `memory/knowledge/` 目录，分类存储知识：

```
knowledge/
├── projects/       # 项目资料
├── notes/          # 学习笔记
├── docs/           # 技术文档
├── snippets/       # 代码片段
└── references/     # 参考资料
```

---

## 八、绑定聊天渠道

### 8.1 Telegram（推荐）

1. 在 Telegram 找 @BotFather 创建机器人
2. 获取 Bot Token
3. 配置到 OpenClaw

### 8.2 网页端

OpenClaw 自带网页端，直接访问即可，无需额外配置。

---

## 九、成本估算

| 项目 | 费用 |
|------|------|
| OpenClaw 框架 | 免费 |
| 模型 API | 免费额度/¥100-500/月 |
| 服务器 | 本地运行 ¥0 |
| Telegram | 免费 |
| **总计** | **¥0-500/月** |

个人使用完全可以用免费额度，几乎零成本！

---

## 十、小墨的功能清单

| 功能 | 状态 | 说明 |
|------|------|------|
| 聊天对话 | ✅ | 日常交流、问答 |
| 网络搜索 | ✅ | Tavily API 搜索 |
| 待办事项 | ✅ | 任务管理 |
| 日历提醒 | ✅ | 日程管理 |
| 天气预报 | ✅ | 实时天气查询 |
| 新闻推送 | ✅ | 每日 AI 前沿推送 |
| 知识库 | ✅ | 个人知识管理 |
| 邮件管理 | ⏳ | 需配置 Himalaya |

---

## 十一、遇到的问题及解决

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

## 十二、后续优化方向

1. **添加更多 Skills** - 邮件管理、番茄钟、习惯追踪
2. **配置精确推送** - Windows 任务计划程序
3. **扩展知识库** - 填充更多学习资料
4. **商业化探索** - 封装成 SaaS 服务

---

## 十三、AI 变现思路

搭建小墨的过程中，我发现了几个变现方向：

### 1. AI 助手 SaaS
把小墨封装成产品，收费订阅。

### 2. 垂直领域助手
专注某个领域（如开发者助手、学生助手、投资人助手）。

### 3. 教程/培训
教别人搭建 AI 助手。

### 4. 技能开发
开发 Skills 放到 ClawHub 售卖。

### 5. 自媒体内容
分享 AI 使用技巧，引流变现。

---

## 总结

搭建个人 AI 助手的门槛比想象中低很多：

- **技术门槛：** 会基本的命令行操作即可
- **时间成本：** 1 天可以上线基础版
- **资金成本：** 几乎为零（用免费额度）

**最重要的是：现在就开始动手！**

---

## 参考资料

- [OpenClaw 官方文档](https://docs.openclaw.ai/)
- [ClawHub 技能市场](https://clawhub.ai/)
- [Tavily API](https://tavily.com/)
- [我的 GitHub](https://github.com/zijie-wei)
- [我的博客](https://zijie-wei.github.io/mine)

---

**如果这篇文章对你有帮助，欢迎点赞、收藏、转发！**

**有问题可以在评论区留言，我会尽量回复。**

**关注我，获取更多 AI Agent 实战内容！**
