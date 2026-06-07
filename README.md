# 面向 AI 产品选型的中文大模型评测系统

本项目模拟 AI 产品经理在真实业务场景下进行大模型选型的流程，围绕 AI 求职助手、AI 文档助手、AI 客服助手、AI 产品经理助手、结构化信息处理、可靠性与安全等 6 类场景，设计 36 道中文场景化测试任务，并基于 n8n 搭建多模型并行评测工作流。

项目结合程序规则校验、LLM-as-Judge 辅助评分、人工抽检机制、响应速度与失败率统计，最终用于输出模型能力画像和产品选型建议。

## 项目定位

本项目不是大模型排行榜，而是面向 AI 产品经理模型选型决策的评测系统。它关注的是“某个模型是否适合某个具体 AI 产品场景”，而不是简单判断“哪个模型绝对最强”。

## 为什么做这个项目

公开排行榜只能提供通用参考，但产品经理实际选型更关注具体场景下的稳定性、可控性、可用性、成本、速度、失败率和安全风险。一个模型在通用榜单上表现较好，并不代表它一定适合客服、求职、文档抽取或安全拒答等真实产品场景。

## 测评对象

- DeepSeek
- Qwen
- Kimi / Moonshot
- Doubao / 火山方舟
- Zhipu GLM
- Baidu Qianfan

## 场景化任务集

任务集位于 `dataset/scenario_test_cases.csv`。共 6 个场景，每个场景 6 道任务，共 36 道任务：

- AI 求职助手
- AI 文档助手
- AI 客服助手
- AI 产品经理助手
- 结构化信息处理
- 可靠性与安全

## 评测方法

- 场景化任务集：用真实 AI 产品使用场景替代抽象考试题。
- 程序规则校验：检查 JSON、表格、字数、禁用词、必填字段、空回答和 API 失败。
- LLM-as-Judge 辅助评分：辅助判断开放式回答质量，但不作为最终权威结论。
- 人工抽检方案：建议抽检 20% 样本，覆盖高分、低分、争议和安全相关样本。
- 响应速度、失败率、成本字段统计：记录 `latency_ms`、`api_status`、`api_error`、token 估算和成本字段。

## 评分体系

总分 100 分：

- task_success：任务成功率，25 分
- instruction_following：指令遵循，15 分
- factuality：事实可靠性，20 分
- structure_stability：结构化稳定性，10 分
- user_usefulness：用户可用性，15 分
- safety_compliance：安全合规，5 分
- cost_latency：成本速度，10 分

## 项目结构

```text
.
├── README.md
├── .gitignore
├── workflow/
│   └── n8n_workflow.json
├── dataset/
│   └── scenario_test_cases.csv
├── docs/
│   ├── manual_build_guide.md
│   ├── portfolio_writeup.md
│   ├── evaluation_method.md
│   └── human_review_template.md
├── report/
│   ├── model_evaluation_report.md
│   ├── evaluation_results_template.csv
│   └── product_selection_summary.md
├── assets/
│   ├── README.md
│   ├── workflow_overview_placeholder.md
│   ├── model_parallel_nodes_placeholder.md
│   └── evaluation_result_placeholder.md
└── backup/
    └── 原始文件备份
```

## 如何运行

1. 打开 n8n。
2. 导入 `workflow/n8n_workflow.json`。
3. 配置 6 个模型 API Key、Base URL 和 Model 环境变量。
4. 配置 Judge 模型 API Key、Base URL 和 Model 环境变量。
5. 先用 1 条任务测试接口和字段解析是否正常。
6. 再运行完整 36 道任务。
7. 查看 `Generate Product Evaluation Report` 节点输出。
8. 将真实运行结果整理到 `report/model_evaluation_report.md`。

> 注意：不同 n8n 版本的 `Split In Batches` 循环连接可能略有差异。当前 workflow 可用于单批次验证；完整 36 题批量评测前，请确认 Split In Batches 的循环连接或根据 n8n 版本调整批处理逻辑。

## API Key 安全提醒

- 不要把真实 API Key 上传到 GitHub。
- ChatGPT Plus 不等于 API 免费额度。
- 各国内模型 API 通常需要单独开通或充值。
- workflow 中只应保留环境变量或占位符，不应写死真实密钥。

## 当前状态

- 已完成 n8n workflow 设计。
- 已完成 36 道场景化任务集。
- 已完成规则校验与 LLM-as-Judge 评分框架。
- 已预留成本、速度、失败率字段。
- 真实完整评测结果需要用户接入 API 后运行，并填入 `report/model_evaluation_report.md`。

## 项目局限

- 样本量有限。
- LLM-as-Judge 可能有偏差。
- 不同模型 API 参数不完全一致。
- 成本估算需要接入真实价格表。
- 仍需要人工抽检和真实用户反馈。

## 下一步计划

- 补充真实运行截图。
- 补充完整评测报告。
- 扩展到 100+ 场景任务。
- 增加可视化 Dashboard。
- 增加 RAG 场景。
- 增加 Agent 工具调用场景。
- 增加真实用户满意度反馈。

## 简历写法

基于 n8n 搭建面向 AI 产品选型的中文大模型评测系统，接入 DeepSeek、Qwen、Kimi、豆包、智谱 GLM、百度千帆等 6 个国内主流模型，设计 36 道场景化任务集，并结合规则校验、LLM-as-Judge 辅助评分、人工抽检方案、响应速度与失败率统计，输出模型能力画像和产品选型建议。