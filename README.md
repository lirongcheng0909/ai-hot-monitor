# AI 热点监控工具

> 一款自动聚合热点、利用 AI 识别真假与相关性、并通过 WebSocket 实时推送和邮件通知的监控工具。

## 项目简介

输入要监控的关键词，系统会自动从 Twitter、Bing、HackerNews、搜狗、B 站等 **8+** 个信息源聚合抓取内容，利用 AI 进行查询扩展、真假识别、相关性分析和智能摘要，并通过 WebSocket 实时推送和邮件通知用户。此外，还将热点监控能力封装为 **Agent Skills 技能包**，让 Cursor、VS Code Copilot、Claude Code 等 AI 编程工具也能直接使用。

## 技术栈

基于 **Express 5 + React 19 + OpenRouter + Socket.io** 的全栈 TypeScript 项目。

| 层级 | 技术 |
|------|------|
| 前端 | React 19 · Vite 7 · TailwindCSS 4 · Framer Motion |
| 后端 | Node.js · Express 5 · TypeScript (tsx) |
| 数据库 | SQLite · Prisma ORM |
| AI 服务 | OpenRouter（真假识别 / 相关性分析 / 智能摘要 / 查询扩展） |
| 实时通信 | Socket.io（WebSocket 推送） |
| 定时任务 | node-cron |
| 邮件通知 | Nodemailer |

## 核心功能

1. **关键词监控**：配置监控关键词，支持激活 / 暂停，系统每 30 分钟自动检查一次。
2. **多源聚合抓取**：从 Twitter、Bing、HackerNews、搜狗、B 站等 8+ 数据源自动聚合内容。
3. **AI 智能分析**：查询扩展（Query Expansion）、真假识别、相关性打分、智能摘要。
4. **多维筛选与排序**：按来源、重要性、时间范围筛选；按热度、相关性、时间排序。
5. **全网搜索**：输入关键词从多个数据源聚合搜索。
6. **实时通知**：WebSocket 实时推送 + 邮件通知。
7. **Agent Skills 技能包**：封装为标准 Skills，可在 Cursor、Copilot、Claude Code 中直接使用。

## 目录结构

```
hot-monitor/
├── server/              # 后端服务（Express + Prisma）
│   ├── src/
│   │   ├── routes/      # API 路由
│   │   ├── services/    # 搜索 / AI / Twitter / 邮件等业务逻辑
│   │   ├── jobs/        # 定时任务
│   │   └── utils/       # 工具函数
│   └── prisma/          # 数据库 Schema 与迁移
├── client/              # 前端应用（React + Vite）
│   └── src/
│       ├── components/  # UI 组件
│       └── services/    # API 与 WebSocket 封装
├── skills/              # Agent Skills 技能包
└── docs/                # 项目文档
```

## 快速开始

### 前置条件

- Node.js ≥ 18（推荐 20 LTS）
- 一个 [OpenRouter API Key](https://openrouter.ai/settings/keys)（必需，用于 AI 分析）

### 1. 安装依赖

```bash
# 后端
cd server
npm install

# 前端
cd ../client
npm install
```

### 2. 配置环境变量

```bash
cp server/.env.example server/.env
```

编辑 `server/.env`，至少填入 OpenRouter API Key：

```env
OPENROUTER_API_KEY=sk-or-v1-你的key
# Twitter API Key（可选）
TWITTER_API_KEY=你的key
```

### 3. 初始化数据库

```bash
cd server
npx prisma generate
npx prisma db push
```

### 4. 启动服务（两个终端）

```bash
# 终端 1：启动后端（端口 3001）
cd server && npm run dev

# 终端 2：启动前端（端口 5173）
cd client && npm run dev
```

访问 **http://localhost:5173**，输入关键词即可开始监控热点。

| 服务 | 地址 |
|------|------|
| 前端页面 | http://localhost:5173 |
| 后端 API | http://localhost:3001 |
| 数据库管理 | `cd server && npx prisma studio`（可选） |

## 环境变量说明

| 变量 | 必填 | 说明 |
|------|------|------|
| `DATABASE_URL` | 否 | SQLite 数据库路径，默认 `file:./dev.db` |
| `PORT` | 否 | 后端端口，默认 3001 |
| `CLIENT_URL` | 否 | 前端地址，用于 CORS 和 WebSocket |
| `OPENROUTER_API_KEY` | 是 | OpenRouter API Key，用于 AI 分析 |
| `TWITTER_API_KEY` | 否 | Twitter API Key（twitterapi.io），不填则不抓取 Twitter |
| `SMTP_*` / `NOTIFY_EMAIL` | 否 | 邮件通知配置，不填则不发送邮件 |

## 更多文档

- [本地运行指南](docs/LOCAL_SETUP.md)
- [API 集成文档](docs/API_INTEGRATION.md)
- [需求文档](docs/REQUIREMENTS.md)
