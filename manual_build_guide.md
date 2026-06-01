# manual_build_guide.md

## 第 1 步：Manual Trigger
添加 `Manual Trigger` 节点，用于手动启动评测。

## 第 2 步：Generate Scenario Test Cases
添加 `Code` 节点，命名为 `Generate Scenario Test Cases`。从 `workflow.json` 复制同名节点代码。它会生成 36 条测试任务。

## 第 3 步：Split In Batches
添加 `Split In Batches`，Batch Size 设置为 `1`。连接：`Generate Scenario Test Cases -> Split In Batches`。

## 第 4 步：6 个模型节点
添加 6 个 `HTTP Request` 节点：`Call DeepSeek`、`Call Qwen`、`Call Kimi`、`Call Doubao`、`Call Zhipu GLM`、`Call Baidu Qianfan`。全部连接自 `Split In Batches`。

通用配置：Method 为 POST，URL 为 `{{$env.XXX_BASE_URL + '/chat/completions'}}`，Header 为 `Authorization: Bearer {{$env.XXX_API_KEY}}` 和 `Content-Type: application/json`，Body 使用 OpenAI-compatible Chat Completions。开启 Continue On Fail。

## 第 5 步：Normalize Model Answers
添加 Code 节点，统一输出 provider、model_id、answer、latency_ms、api_status、api_error、estimated_input_tokens、estimated_output_tokens、estimated_cost。若平台返回结构不同，修改回答字段路径。

## 第 6 步：Objective Rule Checks
添加 Code 节点，检查 JSON、表格、字数、禁用词、必填字段、空回答和 API 失败。

## 第 7 步：LLM-as-Judge
添加 HTTP Request 节点调用评分模型。要求评分模型只输出 JSON，包含 7 个评分维度、total_score、error_type、risk_level、judge_reason、product_recommendation。

## 第 8 步：Parse Judge Result
添加 Code 节点安全解析评分 JSON。解析失败时 total_score 置 0，error_type 为“评分JSON解析失败”。

## 第 9 步：Aggregate Results
添加 Code 节点统计模型总体平均分、分场景平均分、第一名次数、API 失败率、平均响应时间、高风险占比和常见错误。

## 第 10 步：Generate Product Evaluation Report
添加 Code 节点生成 Markdown 报告。

## 单模型测试
先只连一个模型，运行 1 条 case，确认返回能被 Normalize 节点解析。

## 全模型测试
确认单模型跑通后，再打开 6 个模型，先跑 2 条样本，最后跑全量 36 条。

## 人工抽检
抽检 20% 样本，覆盖高分、低分、争议样本和关键场景。客服、安全、求职承诺类场景优先复核。
