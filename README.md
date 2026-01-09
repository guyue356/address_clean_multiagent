# 科技计量数据地理信息清洗 Agent（异步 LangGraph 实现）
- 原型设计链接：https://guyueaddressclean.netlify.app/（基于Gemmi3 bUILD未接入API版）
- 简要说明：本仓库包含一个基于规则 + LLM 的地址/机构省份识别异步清洗流程，主要实现见笔记本 [词表清洗智能体异步调用.ipynb](词表清洗智能体异步调用.ipynb)。请将 DeepSeek / Tavily 等 API Key 填入 [.env](.env)。

主要文件

- [.env](.env) — 存放 API Key（示例已在仓库中）。
- [词表清洗智能体异步调用.ipynb](词表清洗智能体异步调用.ipynb) — 实现与说明的核心代码与示例 Notebook。
- [README.md](README.md) — 本文件。

快速上手

1. 在 [.env](.env) 中填写 API Key：
   - DEEPSEEK_API_KEY
   - Tavily_API_Key
2. 准备省份词表文件：province_keywords.json（Notebook 中使用）。
3. 安装依赖（示例）：

```sh
pip install python-dotenv pandas tqdm nest_asyncio openpyxl langchain-deepseek langgraph
```


4. 打开并运行 Notebook [词表清洗智能体异步调用.ipynb](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)。最终运行单元会调用：
   * 异步批处理入口：[`run_batch_cleaning`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
   * 单行处理任务：[`process_row_task`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)

核心组件（Notebook 中定义）

* 状态类型：[`AgentState`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
* 规则匹配智能体：[`rule_matcher_agent`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
* AI 推理智能体（异步）：[`ai_reasoner_agent`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
* 校验智能体：[`validator_agent`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
* LangGraph 工作流构建：[`StateGraph`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)、[`END`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
* 已创建的工作流变量：[`workflow`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)、[`app`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
* 规则词表引用：[`PROVINCE_RULES`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
* LLM 客户端示例：[`llm_reasoning`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)、[`ChatDeepSeek`](vscode-file://vscode-app/d:/vscode_/Microsoft%20VS%20Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)

运行说明（Notebook 中示例）

* 并发限制通过 Semaphore 控制（示例 n=20）。
* 使用 DeepSeek 的异步调用 `ainvoke` 以便并发处理（Notebook 中示例）。
* 读取与输出示例使用 Excel（pandas + openpyxl）。

注意事项

* 确保 `province_keywords.json` 存在且格式正确（省份 -> 别名列表）。
* 填写正确的 API Key 后再运行涉及 LLM 的单元。
* Notebook 可直接逐单元运行，或将核心逻辑提取为脚本以供生产环境调用。
