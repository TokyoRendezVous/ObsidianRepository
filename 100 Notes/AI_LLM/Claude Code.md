---
description: Claude Code官方实践指南的笔记
tags:
  - AI
  - Agent
  - Claude
---
## 1. Claude Code的工作原理

![[Pasted image 20260323215932.png]]

代理循环（agentic loop）由两个关键组件驱动：**模型（models）** 和 **工具（tools）**。

Claude Code运行在终端中，任何能通过命令行做的事，都可以让Claude Code完成。

Claude Code serves as the **agentic harness** around Claude: it provides the tools, context management, and execution environment that turn a language model into a capable coding agent. 

内置工具是基础，Claude Code的功能可以扩展：
- Skills：
- MCP：连接外部工具
- hooks：自动化工作流
- subagents：委托任务

### 1.1 Sessions

**Sessions are independent.**

新session -> 干净的上下文

通过 auto memory 和 CLAUDE.md 持久记忆

Claude Code 目前**以目录路径为键**来存储 Session。Claude Code 推荐使用 git worktrees 来实现真正的隔离：每个 worktree 拥有独立的工作目录和 branch，同时共享同一份 repository 历史和远程连接。这样多个 Session 可以并行工作，互不干扰。

`claude --continue`, `claude --resume`：恢复session，session ID 相同。

`claude --continue --fork-session`：创建一个新session id 再branch off，原始session保持不变。

在多个终端使用相同的session会让对话混乱，使用`--fork-session`

### 1.2 Context Window

上下文窗口包含以下内容：对话历史，文件内容，命令输出，CLAUDE.md，auto memory，已加载的技能和系统指令。

Claude Code在触及上下文窗口上限时，会先清除旧的工具输出，然后在需要的时候总结对话。
持久性的规则放在CLAUDE.md中。

控制压缩保留的内容：在CLAUDE.md中添加压缩指令，或 `\compact` focus on xx

Skills和subagents也可以控制加载到上下文的内容。CC会在会话开始时看到Skills的描述，但完整内容仅在被使用时才加载。子代理有全新上下文，与主session独立。

### 1.3 Checkpoint 

每次文件编辑均可逆。连按两次`Esc`回到先前状态。
Checkpoint仅限于session，影响远程系统的操作不可逆。

###  1.4 Work effectively with Claude Code

Claude Code can teach you how to use it. 
不懂就问。

`/init` ：为项目创建CLAUDE.md
`/agents`：配置自定义subagents
`/doctor`：检查安装中常见的故障

发现问题及时中断并重新引导。

初始提示词越精确，返工越少。例如引用具体的文件，提及限制以及给出示例模式。

让CC可以验证自己的工作：测试用例，预期UI的截图，定义预期输出。

## 2. Extend Claude Code

ttps://code.claude.com/docs/en/how-claude-code-works#claude-md-vs-skill



### 2.1 CLAUDE.md

| Feature         | What it does                                               | When to use it                                                                  | Example                                                                         |
| --------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **CLAUDE.md**   | **Persistent** context loaded every conversation           | Project conventions, “always do X” rules                                        | ”Use pnpm, not npm. Run tests before committing.”                               |
| **Skill**       | Instructions, knowledge, and workflows Claude can use      | **Reusable** content, reference docs, repeatable tasks                          | `/deploy` runs your deployment checklist; API docs skill with endpoint patterns |
| **Subagent**    | Isolated execution context that returns summarized results | Context **isolation**, parallel tasks, specialized workers                      | Research task that reads many files but returns only key findings               |
| **Agent teams** | Coordinate multiple independent Claude Code sessions       | Parallel research, new feature development, debugging with competing hypotheses | Spawn reviewers to check security, performance, and tests simultaneously        |
| **MCP**         | Connect to **external** services                           | External data or actions                                                        | Query your database, post to Slack, control a browser                           |
| **Hook**        | Deterministic script that runs on events                   | Predictable automation, no LLM involved                                         | Run ESLint after every file edit                                                |
![[Pasted image 20260324205253.png]]

