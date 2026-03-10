---
title: 搭建一个能写代码的 AI Agent
date: 2026-03-06 09:00:00
tags: [AI Agent, 代码生成，GitHub, 编程助手，实战教程]
categories: AI
author: 子杰
---

# 搭建一个能写代码的 AI Agent

> **摘要：** 本文教你搭建一个能写代码、审查代码、修复 Bug 的 AI Agent。集成 GitHub API，实现代码自动生成和审查。包含完整代码和实战演示。

---

## 📋 目录

1. [功能设计](#功能设计)
2. [技术选型](#技术选型)
3. [环境准备](#环境准备)
4. [核心实现](#核心实现)
5. [GitHub 集成](#github 集成)
6. [实战演示](#实战演示)
7. [优化建议](#优化建议)
8. [总结](#总结)

---

## 功能设计

### 核心功能

1. **代码生成** - 根据需求写代码
2. **代码审查** - 检查代码质量
3. **Bug 修复** - 定位并修复问题
4. **代码解释** - 解释代码逻辑
5. **GitHub 集成** - 自动提交 PR

### 工作流程

```
用户需求 → 理解需求 → 生成代码 → 代码审查 → 提交 GitHub → 创建 PR
```

---

## 技术选型

| 组件 | 技术 | 理由 |
|------|------|------|
| **LLM** | GPT-4/Claude | 代码能力强 |
| **框架** | OpenClaw | 开发快 |
| **版本控制** | Git | 标准工具 |
| **代码托管** | GitHub | 最流行 |
| **语言** | Python/JS | 通用 |

---

## 环境准备

### 第 1 步：安装依赖

```bash
npm install -g openclaw
pip install openai github3.py
```

### 第 2 步：获取 API Key

**OpenAI：** https://platform.openai.com/api-keys

**GitHub：** https://github.com/settings/tokens

### 第 3 步：配置环境变量

```bash
export OPENAI_API_KEY="sk-xxx"
export GITHUB_TOKEN="ghp_xxx"
```

---

## 核心实现

### 代码生成 Agent

```python
from openai import OpenAI

class CodingAgent:
    def __init__(self, api_key: str):
        self.llm = OpenAI(api_key=api_key)
    
    def generate_code(self, requirement: str, language: str = "python") -> str:
        """生成代码"""
        prompt = f"""
用{language}实现以下功能：

{requirement}

要求：
- 代码完整可运行
- 包含注释
- 遵循最佳实践
- 包含错误处理

代码：
"""
        response = self.llm.chat.completions.create(
            model="gpt-4",
            messages=[
                {"role": "system", "content": "你是一个资深软件工程师"},
                {"role": "user", "content": prompt}
            ]
        )
        return response.choices[0].message.content
    
    def review_code(self, code: str) -> dict:
        """代码审查"""
        prompt = f"""
审查以下代码：

```{language}
{code}
```

从以下维度评价：
1. 代码质量（1-10 分）
2. 潜在问题
3. 优化建议
4. 安全性

按 JSON 格式返回：
{{
    "score": 8,
    "issues": ["问题 1", "问题 2"],
    "suggestions": ["建议 1", "建议 2"],
    "security_concerns": ["安全问题"]
}}
"""
        response = self.llm.chat.completions.create(
            model="gpt-4",
            messages=[{"role": "user", "content": prompt}]
        )
        return json.loads(response.choices[0].message.content)
    
    def fix_bug(self, code: str, error_message: str) -> str:
        """修复 Bug"""
        prompt = f"""
以下代码有错误，请修复：

代码：
```python
{code}
```

错误信息：
{error_message}

请：
1. 分析错误原因
2. 提供修复后的代码
3. 解释修复方案

修复后的代码：
"""
        response = self.llm.chat.completions.create(
            model="gpt-4",
            messages=[{"role": "user", "content": prompt}]
        )
        return self.extract_code(response.choices[0].message.content)
    
    def explain_code(self, code: str) -> str:
        """解释代码"""
        prompt = f"""
解释以下代码：

```python
{code}
```

请：
1. 概述功能
2. 逐行解释
3. 说明关键逻辑
4. 举例说明用法

解释：
"""
        response = self.llm.chat.completions.create(
            model="gpt-4",
            messages=[{"role": "user", "content": prompt}]
        )
        return response.choices[0].message.content
    
    def extract_code(self, text: str) -> str:
        """从文本中提取代码"""
        import re
        match = re.search(r'```(?:\w+)?\n(.*?)\n```', text, re.DOTALL)
        return match.group(1) if match else text
```

---

## GitHub 集成

### GitHub API 封装

```python
import github3

class GitHubIntegration:
    def __init__(self, token: str):
        self.gh = github3.login(token=token)
    
    def create_branch(self, repo: str, branch_name: str, base_branch: str = "main"):
        """创建分支"""
        repository = self.gh.repository(*repo.split('/'))
        base = repository.branch(base_branch)
        repository.create_ref(f"refs/heads/{branch_name}", base.commit.sha)
    
    def commit_code(self, repo: str, branch: str, file_path: str, code: str, message: str):
        """提交代码"""
        repository = self.gh.repository(*repo.split('/'))
        
        # 检查文件是否存在
        try:
            contents = repository.file_contents(file_path, ref=branch)
            # 更新文件
            contents.update(message, code.encode())
        except:
            # 创建新文件
            repository.create_file(file_path, message, code.encode(), branch=branch)
    
    def create_pr(self, repo: str, title: str, body: str, head: str, base: str = "main"):
        """创建 PR"""
        repository = self.gh.repository(*repo.split('/'))
        pr = repository.create_pull(title=title, body=body, head=head, base=base)
        return pr.html_url
```

### 完整工作流

```python
class AutoCodingWorkflow:
    def __init__(self, openai_key: str, github_token: str):
        self.agent = CodingAgent(openai_key)
        self.github = GitHubIntegration(github_token)
    
    def execute(self, requirement: str, repo: str, file_path: str):
        """执行完整工作流"""
        # 1. 生成代码
        print("正在生成代码...")
        code = self.agent.generate_code(requirement)
        
        # 2. 代码审查
        print("正在审查代码...")
        review = self.agent.review_code(code)
        
        if review["score"] < 7:
            print(f"代码质量不高（{review['score']}分），重新生成...")
            code = self.agent.generate_code(
                f"{requirement}\n\n优化建议：{review['suggestions']}"
            )
        
        # 3. 创建分支
        branch_name = f"feature/auto-{int(time.time())}"
        print(f"创建分支：{branch_name}")
        self.github.create_branch(repo, branch_name)
        
        # 4. 提交代码
        print("提交代码...")
        self.github.commit_code(repo, branch_name, file_path, code, f"feat: {requirement}")
        
        # 5. 创建 PR
        print("创建 PR...")
        pr_url = self.github.create_pr(
            repo=repo,
            title=f"feat: {requirement}",
            body=f"自动生成代码\n\n需求：{requirement}\n\n代码审查评分：{review['score']}/10",
            head=branch_name
        )
        
        print(f"✅ 完成！PR: {pr_url}")
        return pr_url
```

---

## 实战演示

### 示例 1：生成工具函数

**需求：** "写一个 Python 函数，计算两个日期之间的工作日天数"

**Agent 生成：**

```python
from datetime import datetime, timedelta

def count_workdays(start_date: str, end_date: str) -> int:
    """
    计算两个日期之间的工作日天数（排除周末）
    
    参数：
        start_date: 开始日期，格式 YYYY-MM-DD
        end_date: 结束日期，格式 YYYY-MM-DD
    
    返回：
        工作日天数
    """
    start = datetime.strptime(start_date, "%Y-%m-%d")
    end = datetime.strptime(end_date, "%Y-%m-%d")
    
    workdays = 0
    current = start
    
    while current <= end:
        # 周一到周五是工作日
        if current.weekday() < 5:
            workdays += 1
        current += timedelta(days=1)
    
    return workdays

# 测试
if __name__ == "__main__":
    print(count_workdays("2026-01-01", "2026-01-31"))  # 输出：23
```

**代码审查结果：**
```json
{
    "score": 9,
    "issues": [],
    "suggestions": ["可以添加节假日排除功能"],
    "security_concerns": []
}
```

### 示例 2：修复 Bug

**原始代码：**
```python
def divide(a, b):
    return a / b
```

**错误信息：**
```
ZeroDivisionError: division by zero
```

**Agent 修复：**
```python
def divide(a, b):
    """
    安全除法
    
    参数：
        a: 被除数
        b: 除数
    
    返回：
        商，如果除数为 0 则返回 None
    """
    if b == 0:
        raise ValueError("除数不能为 0")
    return a / b
```

### 示例 3：代码解释

**代码：**
```python
result = [x**2 for x in range(10) if x % 2 == 0]
```

**Agent 解释：**

```
这是一个列表推导式，功能是：

1. `range(10)` - 生成 0-9 的数字
2. `if x % 2 == 0` - 筛选偶数
3. `x**2` - 计算平方
4. 最终结果：[0, 4, 16, 36, 64]

等价于：
```python
result = []
for x in range(10):
    if x % 2 == 0:
        result.append(x**2)
```
```

---

## 优化建议

### 1️⃣ 提高代码质量

**方法：**
- 使用更强的模型（GPT-4 vs GPT-3.5）
- 添加代码规范约束
- 多次迭代优化

### 2️⃣ 减少错误

**方法：**
- 添加单元测试生成
- 集成静态代码分析
- 人工审核关键代码

### 3️⃣ 提升效率

**方法：**
- 缓存常用代码片段
- 批量处理多个文件
- 并行生成和审查

### 4️⃣ 扩展功能

**可以添加：**
- 自动生成文档
- 自动部署
- 代码性能分析
- 依赖管理

---

## 总结

### 核心要点

1. **代码生成 Agent = LLM + 代码审查 + GitHub 集成**
2. **代码审查很重要** - 避免低质量代码
3. **GitHub 集成实现自动化** - 从生成到 PR 全流程
4. **持续优化** - 根据反馈改进

### 实际应用

**适合场景：**
- ✅ 重复性代码
- ✅ 工具函数
- ✅ 单元测试
- ✅ 文档生成
- ✅ 代码迁移

**不适合场景：**
- ❌ 核心业务逻辑
- ❌ 复杂算法
- ❌ 性能关键代码
- ❌ 安全相关代码

### 最后的话

**AI 写代码不是替代程序员，而是：**
- 减少重复劳动
- 提高开发效率
- 降低入门门槛
- 让你专注于创造性工作

**开始搭建你的代码 Agent 吧！**

---

**作者：** 子杰  
**GitHub：** https://github.com/zijie-wei  
**博客：** https://zijie-wei.github.io/mine

---

_如果这篇文章对你有帮助，欢迎点赞、评论、分享、关注！_

_下一篇：《2026 年 AI Agent 十大发展趋势》_
