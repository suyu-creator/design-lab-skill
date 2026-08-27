# 07 · 智能体模板骨架库

> 直接复制改写的可复用骨架。所有模板遵循：确定性可测 + 幻觉防线 + fail-closed 权限。

## 1. System Prompt 模板

```markdown
# 角色
你是 <智能体名>，专门<一句话核心任务>。<目标用户>。

# 核心目标
<3-5 条主要能力/目标>

# 行为边界（重要）
- 只基于提供的资料/工具结果回答，不编造事实。
- 无法确认的信息，明确说"我无法确认"，绝不猜测。
- 涉及 <敏感操作列表> 必须先请求用户确认，不擅自执行。
- 超出能力范围时，明确告知并建议替代方案，不硬答。

# 可用工具
- <工具名>：<一句话用途>（<敏感：需确认>）
- <工具名>：<一句话用途>

# 输出格式
<结构化输出要求，如：结论 → 依据（引用来源）→ 建议>
- 引用来源时用 [来源 id] 格式，必须可核对。

# 示例
<few-shot：输入→期望输出 1-2 组>
```

## 2. 工具 Schema 模板（OpenAI 兼容 / JSON Schema）

```json
{
  "name": "search_knowledge",
  "description": "在知识库中检索与问题相关的资料，返回带来源 id 的结果。用户问知识性问题时调用。",
  "input_schema": {
    "type": "object",
    "properties": {
      "query": { "type": "string", "description": "检索关键词，提取用户问题核心" },
      "top_k": { "type": "integer", "description": "返回条数，默认 3" }
    },
    "required": ["query"]
  }
}
```

**声明敏感（fail-closed）**：
```python
# 工具注册时显式声明，新工具默认 sensitive=True
register_tool(tool_schema, sensitive=True)   # 需 HITL 确认
register_tool(tool_schema, sensitive=False)  # 只读/无害操作
```

## 3. 双轨测试模板（mock 确定性 + real 契约）

```python
# mock 引擎：规则表，确定性，可断言
def test_mock_refund_requires_confirm():
    resp = engine.stream("我要退款，订单号 20260820002")
    assert resp[0].tool == "request_refund"
    assert resp[0].need_confirm is True          # 必须确认，未确认前不执行
    assert db.no_refund_executed()               # 确认前不落库

def test_mock_state_machine_rejects_illegal():
    assert ticket.transition("closed", "resolved") is False  # 非法转移拒绝

# real LLM：验证工具协议契约恒定
def test_real_llm_tool_protocol():
    schema = get_tool_schema("search_knowledge")
    assert schema == EXPECTED_SCHEMA              # schema 恒定
    resp = real_client.chat("我的订单到哪了", tools=[schema])
    assert resp.tool_calls[0].name in VALID_TOOLS
```

**幻觉防线测试**（故意注入假声明验证拦截）：
```python
def test_hallucination_caught():
    answer = pipeline.query("谁的发明？")           # 回答含注入的假声明
    assert answer.confidence < 0.7                  # 置信度低 → 触发精炼
    assert "1969" not in answer.claim_report[0]["text"]  # 假声明被移除
```

## 4. 记忆引导规则模板（memoria 风格）

```markdown
## 会话开始
1. memory_retrieve(query="<用户问题>")   # 加载相关历史上下文
2. memory_search(query="GOAL ACTIVE")    # 检查是否有活跃目标

## 会话中
- 用户表达偏好 → memory_store(type="profile")
- 用户纠正事实 → memory_correct(query="...", new="...")
- 话题切换 → memory_retrieve(query="<新话题>")

## 会话结束
1. memory_purge(topic="<本次任务>")      # 清理工作记忆
2. memory_store(content="<会话摘要>", type="episodic")

## 维护
- 定期 memory_consolidate()  # 检测记忆矛盾
- 定期 memory_governance()   # 隔离低置信记忆
- 高风险实验用 memory_branch() 隔离，失败可 rollback
```

## 5. 学习智能体示例骨架（freelingo 提炼，示例场景）

适合"设计学习/教育类智能体"时参考：
- **结构化 + 自适应结合**：确定性放置评估（测水平）→ 按强度生成周路线图（4/8/12/16 周）→ 顺序解锁课程；课程内容在课程边界内由 AI 生成。
- **学习科学落地**：SM-2 间隔重复闪卡；XP / 连续天数 / 技能分 / 完成测试做进度追踪。
- **记忆全局持久**：导师用工具调用保存"学生薄弱点/偏好"等有用事实；用户可手动管理记忆。
- **多语言/语音注意**：本地 TTS 可能只支持部分语言；多语言需切云端或确认模型支持。
- **评估**：放置测试 + 级别完成测试，验证学习效果而不是"聊得开心"。

## 6. 交付评审模板（输出给用户）

```markdown
## 智能体方案交付摘要
- 定位：<一句话>
- 场景：<BDD 摘要>
- 模式选型：<选了哪个 + 为什么>
- 结构：<单/多 agent、工具清单、记忆、权限>
- 复用清单：<引用的现成项目/库>
- 评估集：<条数 + 覆盖>
- 交付检查清单：<勾选结果>
- 未决问题：<待用户决策项>
```
