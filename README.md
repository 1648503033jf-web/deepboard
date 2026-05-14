# DeepFlow Dashboard

<p align="center">
  <img src="https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white" alt="React 19" />
  <img src="https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/Vite-7-646CFF?logo=vite&logoColor=white" alt="Vite 7" />
  <img src="https://img.shields.io/badge/Ant%20Design-6-0170FE?logo=antdesign&logoColor=white" alt="Ant Design" />
  <img src="https://img.shields.io/badge/ECharts-6-AA344D?logo=apacheecharts&logoColor=white" alt="ECharts" />
  <img src="https://img.shields.io/badge/Express-4-000000?logo=express&logoColor=white" alt="Express" />
  <img src="https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/LangGraph-0.2-FF6F00?logo=langchain&logoColor=white" alt="LangGraph" />
  <img src="https://img.shields.io/badge/MCP-Protocol-8B5CF6?logo=data:image/svg+xml;base64,&logoColor=white" alt="MCP" />
</p>

<p align="center">
  <strong>云原生智能可观测平台</strong><br>
  基于 eBPF 零侵入采集，融合网络流量、调用链、服务拓扑与 AI 分析<br>
  让可观测性从"看指标"升级为"懂系统"
</p>

---

## 为什么需要 DeepDashboard

微服务时代面临四大核心挑战：

| 挑战 | 痛点 | DeepFlow 解法 |
|------|------|---------------|
| **看不清** | 服务数量爆炸，调用关系复杂，人工维护拓扑图永远滞后 | Universal Map 自动发现，实时流量拓扑 |
| **查不到** | 传统 APM 需要埋点，未覆盖的服务成为盲区 | eBPF 内核采集，部署即覆盖全部服务 |
| **定不准** | 告警只告诉"哪里红了"，根因需要跨系统拼凑 | AI 自动关联证据，输出诊断结论 |
| **改不动** | 接入可观测需要改代码加 SDK，业务团队抵触 | 零侵入，不改一行业务代码 |

DeepDashboard构建了完整的智能运维平台，特别加入了 DeepFlow 原生不提供的 AI 诊断、RAG 知识库、SLO 管理、工作流编排等能力。

---

## 产品能力全景

```
┌─────────────────────────────────────────────────────────────────────┐
│                        DeepDashboard                           │
├─────────────┬─────────────┬──────────────┬─────────────────────────┤
│  可观测层    │  诊断层      │  智能层       │  管理层                  │
├─────────────┼─────────────┼──────────────┼─────────────────────────┤
│ Universal   │ 故障诊断     │ DeerFlow     │ SLO 管理                │
│ Map 拓扑    │ 工作台       │ 多Agent引擎   │ 告警收敛                │
│             │             │              │                         │
│ 全栈路径    │ 引导式诊断   │ RAG 知识库    │ 工作流编排               │
│ 追踪        │             │              │                         │
│             │ 网络路径     │ MCP 工具协议  │ RBAC 权限               │
│ 调用链分析  │ 诊断         │              │                         │
│             │             │ A2A 跨Agent   │ 自定义大盘               │
│ 流量日志    │ 变更关联     │ 协作          │                         │
│             │ 分析         │              │ 报告管理                 │
│ DNS 分析    │             │ AI Copilot   │                         │
│             │ 异常分析     │              │ 业务线管理               │
│ 协议统计    │             │ 自然语言查询  │                         │
│             │ 慢请求分析   │              │ CMDB 资产                │
│ Profiling   │             │              │                         │
│ 性能分析    │ 数据库分析   │              │ 品牌定制                 │
└─────────────┴─────────────┴──────────────┴─────────────────────────┘
```

---

## 系统架构

```
                    ┌──────────────────────────┐
                    │      用户浏览器           │
                    └────────────┬─────────────┘
                                 │
                    ┌────────────▼─────────────┐
                    │   Frontend (React 19)     │
                    │   Nginx / NodePort:30301  │
                    └────────────┬─────────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                   │
   ┌──────────▼────────┐ ┌──────▼───────┐ ┌────────▼────────┐
   │  Backend (Express) │ │  MCP Server  │ │ DeerFlow Agent  │
   │  NodePort:3001     │ │  Port:3002   │ │ (Python/FastAPI) │
   └──────┬───────┬─────┘ └──────────────┘ └────────┬────────┘
          │       │                                   │
   ┌──────▼──┐ ┌──▼──────────┐              ┌────────▼────────┐
   │  MySQL  │ │    Redis     │              │   LangGraph     │
   │         │ │   (缓存)     │              │   工作流引擎     │
   └─────────┘ └─────────────┘              └────────┬────────┘
                                                      │
                    ┌─────────────────────────────────┼────────┐
                    │                                  │        │
          ┌────────▼────────┐  ┌──────────────┐  ┌───▼──────┐
          │  DeepFlow Server │  │  Prometheus  │  │  LLM API │
          │  (eBPF 数据源)   │  │  / 夜莺      │  │ MiniMax  │
          └─────────────────┘  └──────────────┘  └──────────┘
```

---

## 核心特性

### 🔭 全栈可观测

- **Universal Map** — 基于 eBPF 自动发现服务拓扑，实时展示流量关系
- **全栈路径追踪** — 从 L2-L7 网络到应用调用链的端到端追踪
- **流量日志 & 协议分析** — HTTP/gRPC/DNS/MySQL/Redis 等协议深度解析
- **Profiling 性能分析** — CPU/内存火焰图，定位代码级热点
- **拓扑变更检测** — 自动发现服务依赖变化，关联故障时间线

### 🤖 AI 智能诊断

- **DeerFlow 多 Agent 引擎** — Planner → Researcher → Executor → Verifier 四阶段协作
- **引导式诊断** — 交互式问答逐步缩小故障范围
- **因果推理引擎** — 自动关联告警、指标、日志、拓扑变更，推导根因
- **RAG 知识库** — 历史故障案例 + 运维文档，辅助诊断决策
- **AI Copilot** — 自然语言查询可观测数据，对话式运维

### 🔗 MCP & A2A 协议

- **MCP 工具服务** — 以 Model Context Protocol 暴露平台能力，供外部 AI Agent 调用
- **A2A 跨 Agent 协作** — Agent-to-Agent 通信，支持多系统联合诊断
- **Skill 注册中心** — 动态注册/发现 Agent 技能，按需编排

### 📊 运维管理

- **SLO 管理** — 定义服务等级目标，自动计算错误预算与燃烧率
- **告警收敛** — 智能去重、分组、静默，减少告警风暴
- **工作流编排** — DAG 可视化编排诊断/修复流程，支持 n8n 集成
- **自定义大盘** — 拖拽式仪表盘，支持多数据源混合展示
- **报告生成** — 自动生成诊断报告 / 巡检报告，支持 PDF 导出
- **RBAC 权限** — 角色 + 项目级细粒度权限控制

---

## 技术栈

| 层级 | 技术选型 |
|------|----------|
| **前端** | React 19 + TypeScript 5.9 + Vite 7 + Ant Design 6 + ECharts 6 + AntV G6 |
| **状态管理** | Zustand + React Query (TanStack Query) |
| **后端** | Express 4 + TypeScript + MySQL + Redis + WebSocket |
| **AI Agent** | Python 3.11 + FastAPI + LangGraph + LangChain |
| **LLM** | MiniMax-M2.7（可切换其他 OpenAI 兼容模型） |
| **协议** | MCP (Model Context Protocol) + A2A (Agent-to-Agent) |
| **数据源** | DeepFlow Server (eBPF) + Prometheus/夜莺 + 腾讯云 CLS |
| **部署** | Docker + Kubernetes + Nginx |
| **国际化** | i18next（中/英双语） |

---

## 快速开始

### 环境要求

- Node.js >= 20
- Python >= 3.11
- Docker & Kubernetes（生产部署）
- MySQL 8.0+
- Redis 6+

### 本地开发

```bash
# 1. 克隆项目
git clone <repo-url> deepflow-dashboard
cd deepflow-dashboard

# 2. 安装前端依赖
npm install

# 3. 安装后端依赖
cd server && npm install && cd ..

# 4. 安装 DeerFlow Agent 依赖
cd deerflow && pip install -e ".[dev]" && cd ..

# 5. 配置环境变量
cp .env.development .env.local
cp server/.env.example server/.env
# 编辑 .env.local 和 server/.env，填入实际的数据库/API 地址

# 6. 启动后端
cd server && npm run dev

# 7. 启动前端（新终端）
npm run dev

# 8. 启动 DeerFlow Agent（新终端，可选）
cd deerflow && uvicorn src.main:app --reload --port 8080
```

前端访问：http://localhost:3000  
后端 API：http://localhost:3001  
MCP Server：http://localhost:3002

### 生产部署

```bash
# 一键构建并推送镜像
bash deploy/build.sh v3.2.1.14

# 部署到 Kubernetes
kubectl apply -f deploy/k8s.yaml

# 滚动更新
kubectl rollout restart deployment dashboard-frontend dashboard-backend deerflow-orchestrator -n deepflow
```

---

## 项目结构

```
deepflow-dashboard/
├── src/                    # 前端源码
│   ├── pages/              # 页面组件（70+ 页面）
│   ├── components/         # 业务组件（30+ 模块）
│   ├── api/                # API 请求层
│   ├── stores/             # Zustand 状态管理
│   ├── hooks/              # 自定义 Hooks
│   ├── i18n/               # 国际化资源
│   ├── router/             # 路由配置
│   └── utils/              # 工具函数
├── server/                 # 后端服务
│   ├── src/
│   │   ├── routes/         # API 路由（80+ 接口）
│   │   ├── services/       # 业务逻辑（140+ 服务）
│   │   ├── mcp/            # MCP Server 实现
│   │   ├── middleware/     # 中间件（认证/限流/日志）
│   │   └── config/         # 配置管理
│   └── prompts/            # AI Prompt 模板
├── deerflow/               # AI Agent 引擎 (Python)
│   └── src/
│       ├── agents/         # Agent 定义（Planner/Researcher/Executor/Verifier）
│       ├── orchestrator/   # 编排引擎
│       ├── skills/         # 技能注册
│       ├── memory/         # 对话记忆
│       ├── execution/      # 执行引擎
│       ├── monitoring/     # 监控指标
│       └── scheduler/      # 调度器
├── deploy/                 # 部署配置
│   ├── backend/            # 后端 Dockerfile
│   ├── frontend/           # 前端 Dockerfile
│   ├── deerflow/           # Agent Dockerfile
│   ├── k8s.yaml            # Kubernetes 部署清单
│   └── build.sh            # 一键构建脚本
├── public/                 # 静态资源
└── docs/                   # 项目文档
```

---

## DeerFlow 多 Agent 架构

```
用户输入 / 告警触发
        │
        ▼
┌───────────────┐
│  Intent Router │  ← 意图识别，路由到对应工作流
└───────┬───────┘
        │
        ▼
┌───────────────┐
│    Planner    │  ← 分解任务，制定诊断计划
└───────┬───────┘
        │
        ▼
┌───────────────┐
│  Researcher   │  ← 采集数据：指标/日志/拓扑/调用链
└───────┬───────┘
        │
        ▼
┌───────────────┐
│   Executor    │  ← 执行分析：因果推理/异常检测/关联
└───────┬───────┘
        │
        ▼
┌───────────────┐
│   Verifier    │  ← 验证结论，评估置信度
└───────┬───────┘
        │
        ▼
  诊断报告 / 修复建议
```

核心能力：
- **LangGraph 状态机** — 基于图的工作流编排，支持条件分支和循环
- **Human-in-the-Loop** — 关键决策节点支持人工审批
- **Memory 持久化** — Redis 存储对话上下文，支持多轮诊断
- **Prometheus 指标** — Agent 执行耗时、成功率、Token 消耗全链路监控

---

## MCP 工具协议

平台通过 MCP (Model Context Protocol) 将运维能力暴露为标准化工具接口，支持外部 AI Agent 直接调用：

```
MCP Client (Cursor/Claude/Dify)
        │
        ▼ Streamable HTTP / SSE
┌───────────────────────┐
│   MCP Server (:3002)  │
├───────────────────────┤
│  Tools:               │
│  - query_metrics      │  ← 查询 DeepFlow 指标
│  - search_logs        │  ← 搜索日志
│  - get_topology       │  ← 获取服务拓扑
│  - run_diagnosis      │  ← 执行故障诊断
│  - list_alerts        │  ← 查询告警
│  - trace_request      │  ← 追踪请求链路
└───────────────────────┘
```

---

## 开发命令

```bash
# 前端
npm run dev          # 启动开发服务器
npm run build        # 生产构建
npm run lint         # ESLint 检查
npm run test         # 运行测试
npm run test:ui      # 测试 UI 界面

# 后端
cd server
npm run dev          # 启动开发服务器（热重载）
npm run build        # TypeScript 编译
npm run test         # 运行测试
npm run mcp          # 启动 MCP Server (stdio)
npm run mcp:sse      # 启动 MCP Server (SSE)
npm run mcp:http     # 启动 MCP Server (Streamable HTTP)

# DeerFlow Agent
cd deerflow
uvicorn src.main:app --reload --port 8080   # 启动 Agent 服务
pytest                                       # 运行测试
```

---

## 环境变量说明

| 变量 | 说明 | 示例 |
|------|------|------|
| `VITE_DEEPFLOW_SERVER` | DeepFlow Server 地址 | `http://10.236.251.11:30796` |
| `DEEPFLOW_SERVER` | 后端连接的 DeepFlow 地址 | `http://deepflow-server.deepflow:20416` |
| `AI_API_BASE_URL` | LLM API 地址 | `http://mini-gateway.dc.aaa.com/v1` |
| `AI_MODEL` | 默认 LLM 模型 | `MiniMax-M2.7` |
| `MYSQL_HOST` | MySQL 数据库地址 | `10.35.2.110` |
| `REDIS_HOST` | Redis 缓存地址 | `localhost` |
| `PROMETHEUS_URL` | Prometheus/夜莺地址 | `http://n9e.dc.aaa.com/api/n9e/proxy/1` |
| `DEERFLOW_ORCHESTRATOR_URL` | DeerFlow Agent 地址 | `http://deer-orchestrator:8080` |

---

## 浏览器支持

| 浏览器 | 版本 |
|--------|------|
| Chrome | >= 90 |
| Firefox | >= 90 |
| Safari | >= 15 |
| Edge | >= 90 |

---

## License

MIT
