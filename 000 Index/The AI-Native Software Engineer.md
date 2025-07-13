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

## 在整个软件开发生命周期中应用AI
### 1. 需求与构想

AI可以作为头脑风暴伙伴和需求分析师。

可以帮助非技术相关者，例如借助AI生成一份草案产品需求文档（PRD）
### 2. 系统设计与架构

将你构想的高层架构描述给AI，通过AI验证你的想法和检查忽视的地方。

AI 可以回答具体的设计问题。

可以用AI生成伪图表来可视化。

现阶段AI的一个强大用途是通过让它考虑失败的情况来验证假设。
### 3. 实现（Coding）

自动生成boilerplate configs

功能实现（函数），代码复用和引用
	_I need to remove duplicates from a list of objects in Python, treating objects with same id as duplicates. How to do that efficiently?_

保持一致性和模式：在大型项目中通常会遵循某种统一的模式，AI可以模仿这种风格和结构，后续新代码的生成也会遵从相似的模式。

生成测试代码、辅助debugging、性能调优与重构。

### 4. 测试与质量保证

生成单元测试、边界用例
...


## 最佳实践与关键原则

1. 清晰、具有上下文的提示词：
	询问代码相关的问题时，最好给出相关的代码片段和函数的目的描述。指定期望的输出格式。
	Good tips：将任务分解成步骤或示例代码。

2. 始终检查和验证AI的输出：永远不要盲目相信AI。使用AI 不代表放弃思考。
3. 控制范围：不要让AI直接自动化生成整个项目。将大任务分解给更小的AI辅助部分。
4. 持续学习并保持更新。持续学习AI相关的新技术与工具，关注AI相关社区。
5. 团队协作与共识：共享提示词、使用规范、经验教训
6. 负责任地使用AI。避免将敏感代码传递到外部AI。