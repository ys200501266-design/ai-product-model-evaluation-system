# 面向 AI 产品选型的中文大模型评测系统

这是一个面向 AI 产品经理作品集的 n8n 项目模板。它不是“大模型排行榜”，而是用真实产品场景判断不同中文大模型是否适合某类 AI 产品落地。

## 项目价值

公开排行榜只能回答“哪个模型分数更高”。产品选型更关心：在 AI 求职助手、AI 文档助手、AI 客服助手、AI 产品经理助手、结构化信息处理、可靠性与安全等场景里，模型是否稳定、可控、可用、便宜、响应快。

## 测评对象与变量

- DeepSeek：`DEEPSEEK_API_KEY`、`DEEPSEEK_BASE_URL`、`DEEPSEEK_MODEL`
- Qwen：`QWEN_API_KEY`、`QWEN_BASE_URL`、`QWEN_MODEL`
- Kimi：`KIMI_API_KEY`、`KIMI_BASE_URL`、`KIMI_MODEL`
- Doubao：`DOUBAO_API_KEY`、`DOUBAO_BASE_URL`、`DOUBAO_MODEL`
- Zhipu GLM：`ZHIPU_API_KEY`、`ZHIPU_BASE_URL`、`ZHIPU_MODEL`
- Baidu Qianfan：`QIANFAN_API_KEY`、`QIANFAN_BASE_URL`、`QIANFAN_MODEL`
- Judge：`JUDGE_API_KEY`、`JUDGE_BASE_URL`、`JUDGE_MODEL`

不要写死真实 API Key，不要上传 GitHub。模型名可能变化，请到各平台控制台复制最新模型 ID。ChatGPT Plus 不等于 API 免费额度，API 通常需要单独开通或充值。

## 文件说明

- `workflow.json`：n8n 工作流模板。
- `scenario_test_cases.csv`：36 道场景化测试任务。
- `manual_build_guide.md`：导入失败时的手动搭建步骤。
- `portfolio_writeup.md`：作品集文案、简历文案、1 分钟面试讲解。

## 使用步骤

1. 在 n8n 导入 `workflow.json`。
2. 配置上方环境变量。
3. 先用 1 条测试任务跑通 DeepSeek 或任一模型。
4. 再开启 6 个模型完整评测。
5. 查看 `Generate Product Evaluation Report` 节点输出的 Markdown 报告。

## 评分体系

总分 100：task_success 25、instruction_following 15、factuality 20、structure_stability 10、user_usefulness 15、safety_compliance 5、cost_latency 10。

## 人工抽检

建议抽检 20% 样本，覆盖高分、低分、争议样本和关键场景。复核事实可靠性、用户可用性、是否符合真实业务需求。人工评分与 AI 评分冲突时，以人工复核为准，并调整 Rubric。

## 常见报错

- 401 / 403：API Key、权限、余额或 Base URL 错误。
- 404：模型 ID 或接口路径错误。
- 429：限流，降低并发或增加等待时间。
- JSON 解析失败：让评分模型只输出 JSON，temperature 设为 0。
- 某模型失败导致流程中断：HTTP Request 节点开启 Continue On Fail。
- 成本字段为 0：当前预留字段，后续接入真实价格表。

## 简历一句话

基于 n8n 搭建面向 AI 产品选型的中文大模型评测系统，接入 DeepSeek、Qwen、Kimi、豆包、智谱 GLM、百度千帆等 6 个国内主流模型，设计 36 道场景化任务集，并结合规则校验、LLM-as-Judge 辅助评分、人工抽检方案、响应速度与失败率统计，输出模型能力画像和产品选型建议。
