# 整体架构
•	表现层 (WPF/UI)：负责聊天界面的展示，包括各种消息的渲染（用户气泡、助手气泡）、正在输入的加载动画等。建议使用 MVVM 模式。
•	网关/业务逻辑层 (ViewModel & Services)：处理对话历史记录存储，维护上下文队列，并将用户的自然语言输入封装发送给下层。
•	MCP (Model Context Protocol) 层：
•	MCP Client：集成在你的 WPF 客户端中。
•	MCP Server (本地或远程)：将这套工控软件的使用手册、状态查询 API、故障代码对照表等作为“Resources (资源)”和“Tools (工具)”暴露给大模型。
•	大模型服务层 (LLM API)：通过 HTTP 或 SDK 接入云端或本地大模型（如 OpenAI、Anthropic、通义千问、DeepSeek 等），利用大模型的逻辑推理能力解析用户意图，并在必要时请求 MCP 提取专业业务数据。

# 技术栈
- MVVM+Microsoft.SemanticKernel 或原生的OpenAI SDK。
	- 使用 Semantic Kernel 的好处是，未来如果需要引入外部工具(类似于 MCP 的扩展链)，它的兼容性和扩展性更好。
- 

## 用LLM输入，回答问题的业务逻辑
### 仅仅调用LLM
- 逻辑:
	- 需要搭建MCP Server,和MCP Client，将一个MCP Client设置为大模型，通过MCP Server调用MCP Client大模型，向数据库和前端返回回答。
- 过程
	- 安装NuGet包 `dotnet add package Microsoft.SemanticKernel`和`OpenAI`
		- ![[Pasted image 20260901193254.png|223]]
	- 创建LLM服务类
		- 思路:
		- 定义接口与模拟数据Mock，最小可行性产品MVP
			- 不依赖外部API,前端与后端调用链路；实现Mock服务模拟网络延迟和假数据返回
			- 

![[Pasted image 20260901192053.png]]
1.	用户提问：用户在 WPF 聊天框输入“怎么校准测试线传感器？”。
2.	调用 LLM API：WPF 后台将系统提示词 (System Prompt)、上下文聊天记录连同最新问题发送给大模型，并附带 MCP 声明的可用“工具列表”。
3.	大模型决策与 MCP 介入：大模型发现自己不知道对应的具体工控机步骤，但看到你的 MCP 协议暴露了一个名为 SearchFactoryManual 的工具，于是大模型返回并挂起，请求调用该工具。
4.	读取业务数据：WPF 客户端（作为 MCP 客户端）执行该工具，从 MCP 服务端（或是本地数据库/文本知识库）中抓取到校准传感器的具体业务手册文本。
5.	生成最终回复：将抓取到的内容通过 API 再次提交给大模型，大模型整理成人类易懂的语言返回（最好使用 流式输出 Streaming）。
6.	WPF 渲染：ViewModel 接收到流式文本，绑定到前台，实现逐字显现的聊天气泡效果。

### 用MCP工具
