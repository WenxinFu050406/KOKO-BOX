# Koko Box

Koko Box is a WeChat Mini Program built with uni-app, Vue 3, TypeScript, and WeChat CloudBase. It features an AI virtual pet companion, task and schedule management, chat and voice interaction, mini games, coin rewards, cloud sync, and optional BLE hardware linkage for daily care and productivity.

[English](#english) | [中文](#zh)

## Contents

- [English](#english)
  - [Overview](#en-overview)
  - [Project Status](#en-project-status)
  - [Core Features](#en-core-features)
  - [Tech Stack](#en-tech-stack)
  - [Project Structure](#en-project-structure)
  - [Quick Start](#en-quick-start)
  - [WeChat CloudBase Setup](#en-wechat-cloudbase-setup)
  - [Hardware Linkage](#en-hardware-linkage)
  - [Available Scripts](#en-available-scripts)
  - [Development Notes](#en-development-notes)
- [中文](#zh)
  - [项目简介](#zh-overview)
  - [项目状态](#zh-project-status)
  - [核心功能](#zh-core-features)
  - [技术栈](#zh-tech-stack)
  - [项目结构](#zh-project-structure)
  - [快速开始](#zh-quick-start)
  - [微信云开发配置](#zh-wechat-cloudbase-setup)
  - [硬件联动](#zh-hardware-linkage)
  - [可用脚本](#zh-available-scripts)
  - [开发说明](#zh-development-notes)

<a id="english"></a>

## English

<a id="en-overview"></a>

### Overview

Koko Box is an AI companion Mini Program centered on a virtual pet named Koko. The project explores how pet care, emotional support, daily planning, course scheduling, lightweight games, reward loops, and hardware interaction can be combined into one gentle productivity experience.

The app is designed for WeChat Mini Program delivery. It uses local storage and mock states for development, and WeChat CloudBase cloud functions for login, AI dialogue, task synchronization, course schedule synchronization, companion economy data, feedback, and town community features.

[Back to top](#koko-box)

<a id="en-project-status"></a>

### Project Status

- Target platform: WeChat Mini Program.
- Current phase: functional prototype with cloud integration scaffolding.
- Frontend: uni-app, Vue 3, TypeScript, Vite.
- Backend: WeChat CloudBase cloud functions and database collections.
- AI mode: cloud-function-only. Client code should not store DashScope or model API keys.
- Offline/demo behavior: local state and mock sessions keep the Mini Program usable when cloud services are not configured or unavailable.

[Back to top](#koko-box)

<a id="en-core-features"></a>

### Core Features

- AI virtual pet companion with health, mood, hunger, energy, cleanliness, intimacy, growth stage, home scene mood, and facing direction state.
- Text chat, voice chat, quick pet replies, chat history loading/clearing, and persona prompt customization through the `pet-dialogue` cloud function.
- Task and DDL planner with categories, priority, repeat type, subtasks, completion status, coin rewards, and cloud synchronization.
- Course schedule import and cross-device schedule synchronization.
- Pet care actions, digest cooldowns, starter resources, inventory, shop purchases, and coin ledger tracking.
- Mini games including Fetch Ball, Bubble Pop, Jump Rope, and Hide and Seek, with score-based pet stat and coin rewards.
- Pet Town community state, presence heartbeat, invite creation, invite joining, and local fallback behavior.
- Statistics, archive, coin log, profile, settings, feedback, and device pages.
- Optional BLE device connection for M5StickS3-like companion hardware through WeChat Mini Program Bluetooth APIs.
- Bilingual UI state support for English and Chinese.

[Back to top](#koko-box)

<a id="en-tech-stack"></a>

### Tech Stack

| Area | Technology |
| --- | --- |
| App framework | uni-app, Vue 3 |
| Language | TypeScript, JavaScript |
| Build tooling | Vite, `@dcloudio/uni-*` packages |
| Mini Program target | WeChat Mini Program |
| Cloud backend | WeChat CloudBase cloud functions and database |
| AI provider | DashScope Qwen through server-side cloud functions |
| Animation/assets | `lottie-miniprogram`, static WebP/JPG/PNG assets |
| Hardware | WeChat BLE APIs, Nordic UART-style BLE service |

[Back to top](#koko-box)

<a id="en-project-structure"></a>

### Project Structure

```text
.
├── App.vue                         # Root Mini Program lifecycle and cloud initialization
├── main.ts                         # App bootstrap
├── pages.json                      # Mini Program pages and tab bar configuration
├── manifest.json                   # uni-app manifest
├── src/
│   ├── App.vue
│   ├── components/                 # Shared Vue UI components
│   ├── composables/                # Auth, language, state, schedule importer, sharing logic
│   ├── config/                     # Cloud, AI, and mini-game configuration
│   ├── pages/                      # Vue page implementations
│   ├── services/                   # Cloud function and BLE service wrappers
│   ├── styles/                     # Global styles
│   └── types/                      # Shared Koko domain types
├── pages/                          # WeChat Mini Program page entry files
├── components/                     # Native Mini Program component files
├── cloudfunctions/
│   ├── login/
│   ├── pet-dialogue/
│   ├── schedule-sync/
│   ├── task-sync/
│   ├── companion-sync/
│   ├── feedback-sync/
│   └── town-community/
├── static/                         # Tab icons, home scene, town map, and other assets
└── scripts/                        # Local build and asset helper scripts
```

[Back to top](#koko-box)

<a id="en-quick-start"></a>

### Quick Start

Prerequisites:

- Node.js and npm.
- WeChat DevTools.
- A WeChat CloudBase environment for cloud-backed features.
- A DashScope API key if AI chat, schedule recognition, ASR, or TTS should call real models.
- BLE-compatible hardware only if testing the device page.

Install dependencies:

```bash
npm install
```

Start the WeChat Mini Program development build:

```bash
npm run dev
```

Create a production build:

```bash
npm run build
```

Open the project in WeChat DevTools:

1. Open this repository root.
2. Ensure `app.json`, `pages.json`, and `manifest.json` are present in the root.
3. Use `unpackage/dist/mp-weixin/` as the Mini Program output directory.
4. If the output directory does not exist, run `npm run dev` once first.

[Back to top](#koko-box)

<a id="en-wechat-cloudbase-setup"></a>

### WeChat CloudBase Setup

Backend features use WeChat CloudBase so identity-sensitive logic and API keys stay server-side.

1. Create a CloudBase environment in WeChat DevTools.
2. Copy the real environment ID.
3. Set it in `src/config/cloud.ts`:

```ts
export const WECHAT_CLOUD_ENV_ID = 'your-real-env-id'
```

Create these database collections:

- `users`
- `pets`
- `user_settings`
- `pet_dialogue_histories`
- `course_schedules`
- `user_tasks`
- `user_companion_state`
- `feedback_records`

Deploy these cloud functions from WeChat DevTools:

| Function | Purpose |
| --- | --- |
| `login` | WeChat login, user/pet/settings bootstrap, profile sync, and town fallback entry |
| `pet-dialogue` | AI chat, quick replies, voice turns, chat history, and schedule image recognition |
| `schedule-sync` | Course schedule load/save/clear |
| `task-sync` | Task and DDL load/save/clear |
| `companion-sync` | Pet state, economy, inventory, purchases, and coin ledger sync |
| `feedback-sync` | User feedback submission and admin feedback handling |
| `town-community` | Pet Town presence, heartbeat, invite creation, and invite joining |

Recommended cloud function environment variables:

| Variable | Used by | Notes |
| --- | --- | --- |
| `QWEN_API_KEY` | `pet-dialogue` | Required for real AI responses |
| `QWEN_MODEL` | `pet-dialogue` | Optional, defaults to `qwen-plus` |
| `QWEN_VL_MODEL` | `pet-dialogue` | Optional, used for schedule image recognition |
| `DASHSCOPE_ASR_MODEL` | `pet-dialogue` | Optional voice recognition model override |
| `DASHSCOPE_TTS_MODEL` | `pet-dialogue` | Optional speech synthesis model override |
| `DASHSCOPE_TTS_VOICE` | `pet-dialogue` | Optional voice override |
| `FEEDBACK_ADMIN_OPENIDS` | `feedback-sync` | Optional comma-separated admin OpenIDs |

Security notes:

- Do not place DashScope or other model API keys in client-side code.
- Keep user-owned records protected by `_openid`-based permissions.
- If a secret was ever committed to client code, rotate it immediately.
- Confirm current CloudBase quota, billing, and permission rules before release.

[Back to top](#koko-box)

<a id="en-hardware-linkage"></a>

### Hardware Linkage

The BLE service wrapper is implemented in `src/services/corgiBle.ts`. It is designed for WeChat Mini Program BLE APIs and companion hardware such as M5StickS3 devices.

Supported discovery hints:

- Device prefix: `group 6`
- Aliases: `M5-CORGI-POMO`, `M5StickS3`, `M5Stick-S3`, `M5Stack`
- BLE service: Nordic UART-style service `6E400001-B5A3-F393-E0A9-E50E24DCCA9E`

The device page can scan, bind, reconnect, disconnect, and send newline-terminated text commands to the connected device. BLE features must be tested in a real Mini Program environment with Bluetooth permissions enabled.

[Back to top](#koko-box)

<a id="en-available-scripts"></a>

### Available Scripts

| Command | Description |
| --- | --- |
| `npm run dev` | Start local WeChat Mini Program development build |
| `npm run build` | Build production WeChat Mini Program output |
| `npm run dev:mp-weixin` | Run raw uni-app WeChat dev command |
| `npm run build:mp-weixin` | Run raw uni-app WeChat build command and copy tab icons |
| `npm run prepare:mp-weixin-assets` | Copy tab bar icon assets into the Mini Program output |
| `npm run fix:uni-cli-shared-alias` | Apply the local uni CLI shared alias fix |

[Back to top](#koko-box)

<a id="en-development-notes"></a>

### Development Notes

- `src/composables/useKokoState.ts` is the central state module for pet state, tasks, schedule, chat, devices, rewards, archive, and metrics.
- `src/services/*` wraps cloud function calls and BLE behavior so page components stay focused on UI.
- `pages.json` defines the route list and the tab bar for Home, Tasks, Town, and Me.
- `static/` contains runtime image assets, tab icons, and scene backgrounds.
- `dist/`, `unpackage/`, `node_modules/`, `.tools/`, and `.npm-cache/` are ignored build or local dependency outputs.
- License information is not specified in this repository.

Suggested GitHub repository description:

```text
Koko Box is a WeChat Mini Program built with uni-app, Vue 3, TypeScript, and WeChat CloudBase. It features an AI virtual pet companion, task and schedule management, chat and voice interaction, mini games, coin rewards, cloud sync, and optional BLE hardware linkage for daily care and productivity.
```

[Back to top](#koko-box)

<a id="zh"></a>

## 中文

<a id="zh-overview"></a>

### 项目简介

Koko Box 是一个围绕虚拟宠物 Koko 展开的 AI 陪伴型微信小程序原型。项目尝试把宠物养成、情绪陪伴、日常计划、课程表、轻量小游戏、奖励机制、宠物小镇社区和硬件互动整合成一个温和的学习与生活辅助体验。

项目面向微信小程序运行环境。开发阶段可以通过本地存储和模拟会话继续调试；登录、AI 对话、任务同步、课程同步、宠物经济系统、反馈和小镇社区等能力通过微信云开发云函数承载。

[返回顶部](#koko-box)

<a id="zh-project-status"></a>

### 项目状态

- 目标平台：微信小程序。
- 当前阶段：带有云开发接入骨架的功能原型。
- 前端技术：uni-app、Vue 3、TypeScript、Vite。
- 后端技术：微信云开发云函数与数据库集合。
- AI 调用模式：仅通过云函数调用，客户端不应保存 DashScope 或模型 API Key。
- 离线/演示行为：当云环境未配置或暂不可用时，应用会尽量使用本地状态和模拟会话继续运行。

[返回顶部](#koko-box)

<a id="zh-core-features"></a>

### 核心功能

- AI 虚拟宠物陪伴，包含健康、心情、饥饿、体力、清洁、亲密度、成长阶段、主页场景心情和朝向状态。
- 文本聊天、语音聊天、宠物快速回复、聊天历史加载/清空，以及通过 `pet-dialogue` 云函数进行宠物人格提示词配置。
- 任务和 DDL 计划管理，支持分类、优先级、重复类型、子任务、完成状态、金币奖励和云端同步。
- 课程表导入与跨设备课程表同步。
- 宠物照顾动作、进食消化冷却、初始资源、背包、商店购买和金币流水记录。
- 小游戏模块，包括接球、戳泡泡、跳绳、捉迷藏，并根据分数奖励宠物属性和金币。
- 宠物小镇社区状态、在线心跳、邀请创建、邀请加入和本地降级逻辑。
- 统计、档案、金币明细、个人资料、设置、反馈和设备页面。
- 可选 BLE 设备连接，支持通过微信小程序蓝牙能力与 M5StickS3 类陪伴硬件联动。
- 中英文 UI 状态支持。

[返回顶部](#koko-box)

<a id="zh-tech-stack"></a>

### 技术栈

| 领域 | 技术 |
| --- | --- |
| 应用框架 | uni-app、Vue 3 |
| 开发语言 | TypeScript、JavaScript |
| 构建工具 | Vite、`@dcloudio/uni-*` 相关包 |
| 小程序目标 | 微信小程序 |
| 云后端 | 微信云开发云函数与数据库 |
| AI 服务 | 通过服务端云函数调用 DashScope Qwen |
| 动画与资源 | `lottie-miniprogram`、静态 WebP/JPG/PNG 资源 |
| 硬件 | 微信 BLE API、Nordic UART 风格蓝牙服务 |

[返回顶部](#koko-box)

<a id="zh-project-structure"></a>

### 项目结构

```text
.
├── App.vue                         # 小程序生命周期与云开发初始化
├── main.ts                         # 应用入口
├── pages.json                      # 页面路由与 tabBar 配置
├── manifest.json                   # uni-app 配置
├── src/
│   ├── App.vue
│   ├── components/                 # 通用 Vue UI 组件
│   ├── composables/                # 登录、语言、状态、课程导入、分享等组合逻辑
│   ├── config/                     # 云环境、AI、小游戏配置
│   ├── pages/                      # Vue 页面实现
│   ├── services/                   # 云函数与 BLE 服务封装
│   ├── styles/                     # 全局样式
│   └── types/                      # Koko 业务类型定义
├── pages/                          # 微信小程序页面入口文件
├── components/                     # 原生小程序组件文件
├── cloudfunctions/
│   ├── login/
│   ├── pet-dialogue/
│   ├── schedule-sync/
│   ├── task-sync/
│   ├── companion-sync/
│   ├── feedback-sync/
│   └── town-community/
├── static/                         # tab 图标、主页场景、小镇地图等资源
└── scripts/                        # 本地构建和资源辅助脚本
```

[返回顶部](#koko-box)

<a id="zh-quick-start"></a>

### 快速开始

准备条件：

- Node.js 与 npm。
- 微信开发者工具。
- 如需云端能力，需要创建微信云开发环境。
- 如需真实 AI 对话、课程识别、语音识别或语音合成，需要 DashScope API Key。
- 只有测试设备页面时才需要 BLE 兼容硬件。

安装依赖：

```bash
npm install
```

启动微信小程序开发构建：

```bash
npm run dev
```

生成生产构建：

```bash
npm run build
```

在微信开发者工具中打开项目：

1. 打开本仓库根目录。
2. 确认根目录存在 `app.json`、`pages.json` 和 `manifest.json`。
3. 小程序输出目录使用 `unpackage/dist/mp-weixin/`。
4. 如果输出目录还不存在，先运行一次 `npm run dev`。

[返回顶部](#koko-box)

<a id="zh-wechat-cloudbase-setup"></a>

### 微信云开发配置

后端能力使用微信云开发承载，避免把身份敏感逻辑和 API Key 放在客户端。

1. 在微信开发者工具中创建云开发环境。
2. 复制真实环境 ID。
3. 在 `src/config/cloud.ts` 中配置：

```ts
export const WECHAT_CLOUD_ENV_ID = 'your-real-env-id'
```

创建以下数据库集合：

- `users`
- `pets`
- `user_settings`
- `pet_dialogue_histories`
- `course_schedules`
- `user_tasks`
- `user_companion_state`
- `feedback_records`

在微信开发者工具中部署以下云函数：

| 云函数 | 作用 |
| --- | --- |
| `login` | 微信登录、用户/宠物/设置初始化、资料同步，以及小镇降级入口 |
| `pet-dialogue` | AI 聊天、快速回复、语音轮次、聊天历史和课程表图片识别 |
| `schedule-sync` | 课程表加载、保存和清空 |
| `task-sync` | 任务和 DDL 加载、保存和清空 |
| `companion-sync` | 宠物状态、经济系统、背包、购买记录和金币流水同步 |
| `feedback-sync` | 用户反馈提交与管理员反馈处理 |
| `town-community` | 宠物小镇在线状态、心跳、邀请创建和邀请加入 |

推荐配置的云函数环境变量：

| 变量 | 使用云函数 | 说明 |
| --- | --- | --- |
| `QWEN_API_KEY` | `pet-dialogue` | 真实 AI 回复必需 |
| `QWEN_MODEL` | `pet-dialogue` | 可选，默认 `qwen-plus` |
| `QWEN_VL_MODEL` | `pet-dialogue` | 可选，用于课程表图片识别 |
| `DASHSCOPE_ASR_MODEL` | `pet-dialogue` | 可选，语音识别模型覆盖 |
| `DASHSCOPE_TTS_MODEL` | `pet-dialogue` | 可选，语音合成模型覆盖 |
| `DASHSCOPE_TTS_VOICE` | `pet-dialogue` | 可选，语音音色覆盖 |
| `FEEDBACK_ADMIN_OPENIDS` | `feedback-sync` | 可选，英文逗号分隔的管理员 OpenID |

安全说明：

- 不要把 DashScope 或其他模型 API Key 放进客户端代码。
- 用户私有记录建议通过 `_openid` 权限隔离。
- 如果密钥曾经被提交到客户端代码，请立刻轮换密钥。
- 发布前需要确认当前微信云开发额度、计费和数据库权限规则。

[返回顶部](#koko-box)

<a id="zh-hardware-linkage"></a>

### 硬件联动

BLE 服务封装位于 `src/services/corgiBle.ts`。该模块面向微信小程序蓝牙 API，适配 M5StickS3 一类的陪伴硬件。

支持的发现线索：

- 设备名前缀：`group 6`
- 别名：`M5-CORGI-POMO`、`M5StickS3`、`M5Stick-S3`、`M5Stack`
- BLE 服务：Nordic UART 风格服务 `6E400001-B5A3-F393-E0A9-E50E24DCCA9E`

设备页面支持扫描、绑定、重连、断开连接，以及向已连接设备发送以换行结尾的文本命令。BLE 功能需要在真实微信小程序环境中开启蓝牙权限后测试。

[返回顶部](#koko-box)

<a id="zh-available-scripts"></a>

### 可用脚本

| 命令 | 说明 |
| --- | --- |
| `npm run dev` | 启动本地微信小程序开发构建 |
| `npm run build` | 生成微信小程序生产构建 |
| `npm run dev:mp-weixin` | 运行原始 uni-app 微信开发命令 |
| `npm run build:mp-weixin` | 运行原始 uni-app 微信构建命令，并复制 tab 图标 |
| `npm run prepare:mp-weixin-assets` | 将 tabBar 图标资源复制到小程序输出目录 |
| `npm run fix:uni-cli-shared-alias` | 应用本地 uni CLI shared alias 修复 |

[返回顶部](#koko-box)

<a id="zh-development-notes"></a>

### 开发说明

- `src/composables/useKokoState.ts` 是核心状态模块，管理宠物、任务、课程表、聊天、设备、奖励、档案和统计数据。
- `src/services/*` 封装云函数调用和 BLE 行为，让页面组件更专注于界面呈现。
- `pages.json` 定义页面路由，以及 Home、Tasks、Town、Me 四个 tab。
- `static/` 保存运行时图片资源、tab 图标和场景背景。
- `dist/`、`unpackage/`、`node_modules/`、`.tools/` 和 `.npm-cache/` 是构建产物或本地依赖目录，已在 `.gitignore` 中忽略。
- 当前仓库未声明 License。

建议使用的 GitHub 仓库描述：

```text
Koko Box is a WeChat Mini Program built with uni-app, Vue 3, TypeScript, and WeChat CloudBase. It features an AI virtual pet companion, task and schedule management, chat and voice interaction, mini games, coin rewards, cloud sync, and optional BLE hardware linkage for daily care and productivity.
```

[返回顶部](#koko-box)
