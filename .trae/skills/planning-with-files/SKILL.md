# Planning with Files

> Work like Manus — the AI agent company Meta acquired for **$2 billion**.

## 技能描述

实现 Manus 风格的基于文件的规划系统，用于复杂任务管理。创建 `task_plan.md`、`findings.md` 和 `progress.md` 三个持久化文件，解决 AI 代理的上下文丢失问题。

**适用场景**：
- 多步骤任务（3+步骤）
- 研究任务
- 构建/创建项目
- 跨多个工具调用的任务

**不适用场景**：
- 简单问题
- 单文件编辑
- 快速查询

---

## 核心原理

```
Context Window = RAM (易失，有限)
Filesystem = Disk (持久，无限)

→ 任何重要信息都写入磁盘
```

## 三文件模式

| 文件 | 用途 | 更新时机 |
|------|------|----------|
| `task_plan.md` | 阶段跟踪、进度、决策 | 每个阶段完成后 |
| `findings.md` | 研究、发现、知识存储 | 任何发现后立即更新 |
| `progress.md` | 会话日志、测试结果 | 整个会话过程中 |

---

## 使用方法

### 方式一：手动触发

告诉 AI："使用 planning-with-files 技能开始规划任务"

### 方式二：自动激活

当任务满足以下条件时，AI 会自动使用此技能：
- 任务需要 3 个以上步骤
- 任务涉及研究或探索
- 任务需要跨多个文件操作

---

## 快速开始

### 第一步：创建规划文件

在项目根目录创建三个文件：

```bash
# 使用脚本初始化（推荐）
./.trae/skills/planning-with-files/scripts/init-session.sh [项目名称]
```

或手动创建：
- [task_plan.md](templates/task_plan.md) — 任务计划模板
- [findings.md](templates/findings.md) — 发现记录模板
- [progress.md](templates/progress.md) — 进度日志模板

### 第二步：填写任务计划

在 `task_plan.md` 中：
1. 写清楚目标（Goal）
2. 将任务分解为 3-7 个阶段
3. 每个阶段设置状态：`pending` → `in_progress` → `complete`

### 第三步：执行并更新

- **开始阶段前**：读取 `task_plan.md` 刷新目标
- **发现新信息时**：立即写入 `findings.md`
- **完成阶段后**：更新状态，记录到 `progress.md`
- **遇到错误时**：记录到 `task_plan.md` 的错误表格

---

## 核心规则

### 1. 先创建计划

永远不要在没有 `task_plan.md` 的情况下开始复杂任务。

### 2. 2操作规则

> 每进行 2 次查看/浏览/搜索操作后，立即将关键发现保存到文件。

这防止视觉/多模态信息在上下文重置时丢失。

### 3. 决策前读取

在做出重大决策前，读取计划文件。这会将目标保持在注意力窗口中。

### 4. 行动后更新

完成任何阶段后：
- 更新阶段状态：`in_progress` → `complete`
- 记录遇到的错误
- 记录创建/修改的文件

### 5. 记录所有错误

每个错误都记录在计划文件中。这建立知识库并防止重复。

```markdown
## Errors Encountered
| Error | Attempt | Resolution |
|-------|---------|------------|
| FileNotFoundError | 1 | 创建默认配置 |
| API timeout | 2 | 添加重试逻辑 |
```

### 6. 永不重复失败

```
if action_failed:
    next_action != same_action
```

跟踪尝试过的操作，改变方法。

---

## 3次错误协议

```
尝试 1：诊断并修复
  → 仔细阅读错误
  → 识别根本原因
  → 应用针对性修复

尝试 2：替代方案
  → 同样错误？尝试不同方法
  → 不同工具？不同库？
  → 永远不要重复完全相同的失败操作

尝试 3：更广泛的重新思考
  → 质疑假设
  → 搜索解决方案
  → 考虑更新计划

3次失败后：升级给用户
  → 解释尝试了什么
  → 分享具体错误
  → 请求指导
```

---

## 5问题重启测试

如果能回答这些问题，说明上下文管理良好：

| 问题 | 答案来源 |
|------|----------|
| 我在哪里？ | task_plan.md 中的当前阶段 |
| 我要去哪里？ | 剩余阶段 |
| 目标是什么？ | 计划中的目标陈述 |
| 我学到了什么？ | findings.md |
| 我做了什么？ | progress.md |

---

## 读写决策矩阵

| 情况 | 操作 | 原因 |
|------|------|------|
| 刚写入文件 | 不读取 | 内容仍在上下文中 |
| 查看图片/PDF | 立即写入发现 | 多模态→文本，防止丢失 |
| 浏览器返回数据 | 写入文件 | 截图不持久 |
| 开始新阶段 | 读取计划/发现 | 上下文可能过时 |
| 发生错误 | 读取相关文件 | 需要当前状态来修复 |
| 间隔后恢复 | 读取所有规划文件 | 恢复状态 |

---

## 反模式

| 不要 | 应该 |
|------|------|
| 用 TodoWrite 持久化 | 创建 task_plan.md 文件 |
| 陈述目标后忘记 | 决策前重新读取计划 |
| 隐藏错误并静默重试 | 将错误记录到计划文件 |
| 把所有内容塞入上下文 | 将大内容存储到文件 |
| 立即开始执行 | 首先创建计划文件 |
| 重复失败的操作 | 跟踪尝试，改变方法 |
| 在技能目录创建文件 | 在项目目录创建文件 |

---

## 文件位置说明

| 位置 | 内容 |
|------|------|
| 技能目录 (`.trae/skills/planning-with-files/`) | 模板、脚本、参考文档 |
| 你的项目目录 | `task_plan.md`、`findings.md`、`progress.md` |

---

## 模板

- [templates/task_plan.md](templates/task_plan.md) — 阶段跟踪
- [templates/findings.md](templates/findings.md) — 研究存储
- [templates/progress.md](templates/progress.md) — 会话日志

## 脚本

- `scripts/init-session.sh` — 初始化所有规划文件
- `scripts/check-complete.sh` — 验证所有阶段是否完成

## 参考资料

- [examples.md](examples.md) — 真实使用示例
- [reference.md](reference.md) — Manus 原则详解

---

## 版本信息

- **版本**: 2.15.0
- **基于**: [Planning with Files](https://github.com/OthmanAdi/planning-with-files)
- **灵感来源**: [Manus AI Context Engineering](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)
