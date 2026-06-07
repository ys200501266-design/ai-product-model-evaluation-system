# n8n 手动搭建与运行指南

## 1. 如何导入 workflow/n8n_workflow.json

1. 打开 n8n。
2. 进入 Workflows。
3. 选择 Import from File。
4. 选择 `workflow/n8n_workflow.json`。
5. 导入后先不要直接跑全量任务，先检查环境变量和 HTTP Request 节点配置。

## 2. 如果导入失败，如何手动新建节点

按下面顺序手动创建节点，并把 `workflow/n8n_workflow.json` 中对应 Code 节点的代码复制进去。

## 3. 每个节点的作用

- Generate Scenario Test Cases：生成 36 道场景化测试任务。
- Split In Batches：按批次处理任务，便于控制并发和排错。
- Call DeepSeek：调用 DeepSeek API。
- Call Qwen：调用 Qwen API。
- Call Kimi：调用 Kimi / Moonshot API。
- Call Doubao：调用豆包 / 火山方舟 API。
- Call Zhipu GLM：调用智谱 GLM API。
- Call Baidu Qianfan：调用百度千帆 API。
- Normalize Model Answers：统一模型回答字段。
- Objective Rule Checks：做 JSON、表格、字段、禁用词、API 状态等客观校验。
- LLM-as-Judge：调用评分模型做辅助评分。
- Parse Judge Result：安全解析评分 JSON，失败时兜底。
- Aggregate Results：聚合模型表现、失败率、延迟和错误类型。
- Generate Product Evaluation Report：生成 Markdown 报告。

## 4. 如何先测试 1 条任务

建议先临时把 Generate Scenario Test Cases 里的 cases 截取为 1 条，确认一个模型 API 可以正常返回，再接入全部模型。

## 5. 如何再测试完整 36 条任务

确认单条任务跑通后，恢复 36 条任务，并检查 Split In Batches 的循环连接是否符合当前 n8n 版本。完整评测理论上应得到：36 道题 × 6 个模型 = 216 条模型回答。

## 6. 如何判断是否真的跑完

检查最终聚合前的原始记录数。如果少于 216 条，需要检查 Split In Batches 循环、某些模型节点是否失败、是否有节点没有连接到 Normalize。

## 7. 常见错误处理

- 401/403：API Key、权限、余额或 Base URL 错误。
- 404：模型 ID 或接口路径错误。
- 429：触发限流，降低并发或增加等待时间。
- JSON 解析失败：降低 Judge temperature，提示词强调只输出 JSON。
- 某模型失败导致流程中断：HTTP Request 节点开启 Continue On Fail。
- Split In Batches 只跑一条：检查循环连接，或根据 n8n 版本调整批处理逻辑。

## 8. 如何导出最终报告

运行后查看 `Generate Product Evaluation Report` 节点的 `report_markdown` 字段，复制到 `report/model_evaluation_report.md`。

## 9. 如何把结果填入 report/model_evaluation_report.md

不要手动编造分数。只把真实运行后生成的平均分、失败率、延迟、风险占比和选型建议填入报告表格。

## 10. 上传 GitHub 前如何检查 API Key

搜索以下关键词：`sk-`、`api_key`、`API_KEY`、`Authorization`、`Bearer`。确认没有真实密钥，只有环境变量或占位符。