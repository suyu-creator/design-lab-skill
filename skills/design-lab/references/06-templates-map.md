# 06 · 模板地图（31 个真实系统架构模板）

> 设计时**先查这张表**：找到最接近场景的系统，把它的架构图当起点——照「关键决策与权衡」「常见误区」逐条问自己，而不是从零硬想。
> 每个模板最浓缩的部分 = **关键决策与权衡** + **演进路线**（何时升级、何时过度设计）。
> 来源：awesome-architecture templates/（31 个），每个模板附真实开源项目/工程文档链接，可顺藤摸瓜读源码。

## 一、经典 / 通用系统（16）

| 模板 | 代表产品 | 核心架构看点 |
|---|---|---|
| 普通网站 | 企业官网、SaaS 后台 | 经典三层、缓存、读写分离的"够用就好" |
| 移动 App | 大多数 iOS/Android | 离线优先、数据同步、客户端状态、推送 |
| 电商平台 | Amazon、Shopify、淘宝 | 库存、订单、支付、**超卖**、大促洪峰 |
| 社交信息流 | Twitter/X、Instagram | Feed 拉/推、关注关系、**热点扩散**、收件箱 |
| 视频流媒体 | Netflix、YouTube | 转码（管道）、CDN、自适应码率、推荐 |
| 实时通讯 | WhatsApp、Slack、微信 | 长连接、**消息时序**、离线投递、群扩散 |
| 短链接服务 | Bitly、TinyURL、t.co | 读多写少、缓存、301/302、分布式唯一 ID |
| 支付系统 | Stripe、支付宝、PayPal | **幂等**、复式记账、对账、状态机 |
| 搜索引擎 | Google、Elasticsearch | 倒排索引、相关性排序、召回+精排、分片 |
| 网约车 / 出行 | Uber、滴滴 | 地理空间索引、实时位置、供需匹配、动态定价 |
| 实时协同文档 | Google Docs、Figma | OT/CRDT、单 writer 串行、操作日志、离线同步 |
| 云存储 / 网盘 | Dropbox、iCloud | 文件分块、内容寻址去重、增量同步、断点续传 |
| 通知 / 推送系统 | Novu、FCM/APNs | 多渠道扇出、**去重限频**、异步重试、优先级 |
| 在线票务 / 抢票 | Ticketmaster、大麦、12306 | 虚拟等候室、**原子扣减防超卖**、锁座超时 |
| 浏览器插件 | Honey、Grammarly | 内容脚本/后台分离、页面注入、隐私边界 |
| AI 对话产品 | Claude、ChatGPT | LLM 推理、流式输出（SSE）、上下文管理、RAG、成本控制 |

## 二、AI 原生系统（5）

| 模板 | 代表产品 | 核心架构看点 |
|---|---|---|
| AI 网关 / 中转站 | One API、LiteLLM、Portkey | 统一接口、计费限流、负载均衡、故障转移、缓存 |
| RAG 知识库 | RAGFlow、LlamaIndex、Dify | 切块、向量检索、**混合检索+重排**、引用溯源 |
| AI Agent / 工作流平台 | Dify、Coze、LangGraph | **行动循环、工具沙箱、记忆、可控兜底**（步数/成本上限） |
| 模型推理服务 | vLLM、SGLang、Triton | 连续批处理、分页 KV 缓存、量化、多副本、排队降载 |
| 向量数据库 | Milvus、Qdrant、pgvector | ANN 近似最近邻、HNSW/IVF、召回-延迟权衡 |

## 三、AI 编码 / 自治 Agent（4）

| 模板 | 代表产品 | 核心架构看点 |
|---|---|---|
| Claude Code | Claude Code | 本地优先编码 agent、子代理/钩子/技能/MCP、**双层权限 + OS 沙箱**、上下文压缩 |
| OpenAI Codex | Codex CLI + Cloud | 本地 CLI 与云端异步沙箱双形态、**沙箱 × 审批双轴**、默认断网防注入、自动开 PR |
| OpenClaw | OpenClaw | 自托管 Gateway、聊天软件即 UI、心跳/cron、可插拔 harness、记忆即纯文本 |
| Hermes | Hermes | 常驻自我成长、FTS5 持久记忆、自动沉淀技能、cron、多渠道/多 provider |

## 四、系统提示词架构（1）

| 模板 | 代表产品 | 核心架构看点 |
|---|---|---|
| 系统提示词架构 | Claude、ChatGPT、Gemini、Cursor | **分层 Agent OS**、决策树路由、Skills 外化、版权/记忆合规、运行时注入 |

## 五、工业 / 嵌入式系统（5）

| 模板 | 代表产品 | 核心架构看点 |
|---|---|---|
| 嵌入式设备固件 | 智能门锁、手环 | HAL 分层、状态机、看门狗、**A/B 双分区 OTA 防变砖** |
| IoT 设备接入平台 | AWS IoT Core、ThingsBoard | 百万长连接、**一机一密**、设备影子、灰度 OTA |
| 工业边缘网关 / SCADA | EdgeX、OPC UA | 协议归一、边缘自治、**OT/IT 隔离（Purdue）**、受控反向控制 |
| 车载电子电气（E/E） | 特斯拉、openpilot | **ASIL 安全隔离**、CAN+车载以太网两张网、整车 OTA、影子模式 |
| 机器人系统 | 扫地机、AGV、无人机 | 感知→规划→控制**频率分层**、独立安全旁路、录制回放、车队管理 |

## 深挖索引（需要深入某个主题时回原文）

| 主题 | 原文章节 |
|---|---|
| 一致性工程（Saga/Outbox/幂等/事件溯源/CQRS） | awesome-architecture tutorial/11 |
| 规模化力学（一致性哈希/热点/多活/尾延迟/扇出放大） | tutorial/13 |
| 演进拆分（绞杀者/并行运行/零停机迁移/DDD 限界上下文） | tutorial/14 |
| 组织即架构（康威/逆康威/团队拓扑/平台工程） | tutorial/15 |
| 安全多租户（威胁建模/零信任/爆炸半径/租户隔离） | tutorial/16 |
| 大模型时代判断（vibe coding/非确定性/上下文工程） | tutorial/17 |
| 完整案例推演（从 0 推到上线再推到真实压力） | cases/（抢票/工单 SaaS/RAG/实时协同/Feed/编码 Agent 平台） |
| 技术选型（语言/数据库/缓存MQ/API/云原生/可观测/AI 基建） | tutorial/27-34 |
