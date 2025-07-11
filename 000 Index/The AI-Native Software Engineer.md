---
source: https://addyo.substack.com/p/the-ai-native-software-engineer
date: 2025-07-12
tags:
  - AI
---
AI是提高生产力和创造力的工具，而不是威胁。将AI视为一个知识渊博的初级编程搭档。

这种合作可以加快开发速度和提高质量，但前提是承担监督的责任。

Every engineer is a manager now. 
工程师使用规则文件、README和结构化的代码，让AI更容易处理。

## 构建AI原生开发流程

### 第一步：任务先交给AI尝试

典型的工作流是先把任务交给AI尝试
用AI调研新技术，或者让AI生成原型方案。


> [!quote] Title
> Hallucinations can be significantly mitigated and managed with proper context engineering and agentic feedback loops.


### 第二步：选择合适的AI工具

在VSCode中安装Copilot、Cursor或Cline等插件。
使用独立工具（ChatGPT、Claude）进行问答和决策辅助

### 第三步：学习提示词工程

Prompt Engineering
《Prompting Guide 101》--Google


> [!example]  Poor prompt
> Can you write tests for my React component?


> [!example] Better prompt
>I have a LoginForm React component with an email field, password field, and submit button. It displays a success message on successful submit and an error message on failure, via an onSubmit callback. 
>**Please write a Jest test file** that: 
>(1) renders the form, 
>(2) fills in valid and invalid inputs, 
>(3) submits the form, 
>(4) asserts that onSubmit is called with the right data, and
(5) checks that success and error states render appropriately.

两个快速小Tips：明确你想要的格式，在提问中将复杂任务分解为有序的步骤。

### 第四步：用AI进行代码生成和补全

一个好的初始用例可以是生成样板代码或重复代码。
	简单函数或工具类
	配置文件
	表单校验、接口调用的重复性逻辑
	
不要盲目接受AI的初始实现，仔细阅读并进行测试。

### 第五步：将AI集成到非编码任务

编写PR描述、撰写代码文档、初步设计文档和架构说明。

用AI进行规划：描述需求让AI概述一种可能的方法。

起草产品邮件，复杂技术沟通内容

### 第六步：通过反馈迭代和完善

注意AI输出中需要修正的地方，通过反馈改进下一次提示。

记录有效的提示词格式，建立个人的提示词模板库。

### 第七步：坚持验证和测试

永远不要假设AI100%正确。

对Ai编写的代码始终应用代码审查、测试和静态分析。

在经验积累中了解到AI在哪些任务上表现较弱，在这部分仔细检查。

### 第八步：扩展到复杂用途

使用Cursor、Windsurf等工具的Agent模式自动监听代码问题并修复。

使用AI进行端到端的原型设计 （练习使用主要依靠AI辅助的方式构建一个简单应用）

逐步解锁高级用法，但坚持检查。