# AHLLMProxy

> 统一多厂商大模型代理网关（桌面端）· 当前版本 **v0.5.4**

一个跑在本地的「模型网关 + 管理控制台」：把 DeepSeek、阿里百炼、腾讯混元、火山方舟等多家上游，以及任意 OpenAI 兼容端点，统一成一套标准出口对外提供服务。桌面图形界面里完成模型商接入、代理模型编排、Token 统计、日志追踪与成本核算，所有数据保存在本地，开箱即用、可整目录迁移。

## 特性

- **统一兼容出口**：对外提供 OpenAI 兼容的 `/v1/chat/completions`（含 SSE 流式）、`/v1/responses`、`/v1/embeddings`、`/v1/models`，以及 Anthropic 兼容的 `/v1/messages`
- **多模态透传**：生图（`/v1/images/generations`、`edits`、`variations`）、TTS（`/v1/audio/speech`）、STT（`/v1/audio/transcriptions`、`translations`）、视频（`/v1/videos/*`）等非对话端点直接透传
- **多模型商接入**：内置多家常见厂商预设，也支持任意 OpenAI 兼容 Base URL；密钥本地加密存储
- **回退链与自动选择**：代理模型可配置多个上游组成回退链（Fallback），失败自动切换；Sticky Session 保持会话粘滞；支持按能力（chat / 多模态 / 图像）过滤，避免把生图模型送进对话链路
- **缓存亲和**：识别上游 prompt 缓存命中，统计缓存 Token 与节省成本
- **全量日志与用量**：记录每次请求的请求体 / 响应体 / 流式帧 / Token / 耗时 / 状态，支持分类筛选、详情查看与复制
- **成本核算**：内置模型价格库（可维护），按实际用量估算成本
- **内置对话与文件管理**：控制台自带对话页（多模态、Markdown 渲染、附件）与文件页（对话上传附件集中管理，橱窗 / 列表双视图，预览、下载、批量删除）
- **配置导入导出**：代理模型 + 模型池 + 模型商配置可整包加密导出 / 复制分享，对方导入免密自动还原；重复配置可选跳过或覆盖
- **多客户端 API 接入**：为不同客户端签发独立 Token，便于区分来源与回收
- **数据本地、零外部依赖**：SQLite（WASM）落库，全部数据位于 `data/` 目录，绿色可迁移；第三方资源本地化，无外部 CDN

<p align="center">
    <a href="https://github.com/rexyang-ai/AHLLMProxyRelease/releases/latest"><img alt="Latest Release" src="https://img.shields.io/github/v/release/rexyang-ai/AHLLMProxyRelease?label=%E6%9C%80%E6%96%B0%E7%89%88%E6%9C%AC&amp;color=blue"></a>
  <a href="https://github.com/rexyang-ai/AHLLMProxyRelease/releases"><img alt="Downloads" src="https://img.shields.io/github/downloads/rexyang-ai/AHLLMProxyRelease/total?label=%E7%B4%AF%E8%AE%A1%E4%B8%8B%E8%BD%BD&amp;color=success"></a>
  <img alt="Platform" src="https://img.shields.io/badge/平台-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey">
  <img alt="License" src="https://img.shields.io/badge/License-MIT-green">
</p>

---

## 它解决什么问题？

- 🎯 **只对付一份密钥、一份配置**：不管你后面接了 DeepSeek、通义、混元、方舟、Kimi 还是自建 OpenAI 兼容端点，客户端只认一个 `http://127.0.0.1:11435/v1`
- 🔀 **自动挑最快的模型**：多个上游模型定义为"代理模型"，按最快 / 最便宜 / 轮询 / 加权 / 手动优先级自动选择，失败自动回退到下一个
- 💰 **看清每一分钱**：Token 统计精确到 prompt / completion，按模型分组；日志翻页、错误分类饼图、实时请求流一览无余
- 🔒 **数据永远在你自己电脑上**：API Key 用 AES-256-GCM 本地加密，SQLite 数据库、日志、备份全部在应用目录下；无外部 CDN、无遥测
- 🧩 **一键生成客户端配置**：为 Codex CLI、Cursor、Trae、OpenAI SDK 提供开箱即用的配置片段，复制粘贴就能用

---

## 工具预览
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/4aae8dbf-4120-435a-8ea6-a7ec23e3f14b" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/718153df-2f6f-4030-a0d6-e73623fb7467" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/bd25b2b1-7fb8-448d-a120-e1b80b64136a" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/1ad16768-fe35-49e8-826c-a177dbc5bd6d" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/cfa014a0-ab68-427a-95fd-41b7d328e72c" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/c8e4d357-9cd4-4ed0-a11f-891e7f3c739f" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/b79ce72b-aad3-45bc-ba08-0505a6150953" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/4b744321-1b6f-4728-a8d0-245e45e0c86f" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/e4e0476f-3ea7-4d19-b386-dff866e96154" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/2e3d0218-f8da-4d5b-a6b8-9819050a5275" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/db0c3cd1-7c3b-4546-947c-9738babd95a4" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/66bb5a19-6627-46a4-9bd3-45d3c307f96e" />
<img width="1166" height="754" alt="image" src="https://github.com/user-attachments/assets/1e21fc90-00d8-4813-bbf9-4e5809ca1fcb" />


## 主要功能

### 供应商与模型
- 内置 **18+ 主流供应商模板**：DeepSeek / 阿里百炼 / 腾讯混元 / 字节方舟 / 智谱 / MiniMax / 月之暗面 / 硅基流动 / 零一万物 / 百川 / 阶跃星辰 / Groq / Together / OpenRouter / OpenAI / Anthropic / Google Gemini / Ollama
- 一键从上游同步模型列表，批量启用/禁用/删除
- **连通性测试**：单击验证凭据、Base URL、网络可达性
- **健康巡检 + 熔断**：后台每 5 分钟自动检测，连续失败自动暂停 10 分钟；恢复时自动解除；系统通知提醒

### 代理路由
- **代理模型（Proxy Model）**：给一批物理模型起个别名（如 `smart`），客户端请求 `smart` 时按策略路由
- 5 种路由策略：`manual` 手动优先 / `fastest` 最快 / `cheapest` 最便宜 / `round-robin` 轮询 / `weighted` 加权
- **自动回退链**：首选失败（429 / 5xx / 网络异常）时自动尝试下一个
- **Sticky Session**：同一 `user` 字段在 30 分钟内粘住同一模型，避免上下文断裂

### 对外接口（OpenAI 兼容 + Anthropic 兼容）
- `POST /v1/chat/completions` — OpenAI Chat（含 SSE 流式）
- `POST /v1/messages` — Anthropic Claude 格式
- `POST /v1/responses` — OpenAI Responses（Codex CLI 使用）
- `POST /v1/embeddings`
- `GET  /v1/models`
- 请求响应带 `x-ahp-hit-model` / `x-ahp-attempts` 便于观测

### 安全与鉴权
- **全局 Bearer Token**：开启后所有请求必须携带
- **多客户端 Token**：为 Codex / Cursor / Trae 分别生成独立 Token，可随时撤销，日志按客户端分类
- **供应商密钥脱敏**：列表只显示 `sk-****abcd` 预览
- **一键脱敏导出**：导出配置分享给他人时自动隐藏所有密钥

### 观测与统计
- **概览页**：4 大 KPI（今日请求 / Token / 错误率 / 平均耗时）+ 24 小时请求趋势 Sparkline + 错误分类饼图 + 实时请求流（最近 50 条 IPC 推送）
- **模型健康 Top 8**：按成功率 + 平均延迟综合排名
- **服务端分页日志**：支持按模型/客户端/错误类型筛选，行级抽屉展示完整错误堆栈

### 客户端配置生成器
在设置页可为下列客户端一键生成配置（含 Base URL / Token / 模型名占位符）：
- OpenAI SDK（Python / Node.js）
- Cursor（settings.json）
- Codex CLI（`~/.codex/config.toml`）
- Trae IDE（自定义 OpenAI 兼容 Provider）
- curl 冒烟示例

### 桌面体验
- 系统托盘常驻、开机自启（隐藏到托盘）、单实例锁
- 深色 / 浅色 / 跟随系统主题；简体中文 / English 双语
- 全局快捷键：`Ctrl+,` 设置 / `Ctrl+1..6` 快速切换页面 / `Ctrl+Alt+S` 启停服务
- **升级不丢数据**：NSIS 覆盖安装通过临时中转区保留 SQLite / 加密密钥 / 日志 / 备份
- 内建 electron-updater 自动升级：启动 30s 后静默检查，或手动检查

---

## 下载与安装

### Windows
👉 **[下载最新安装版](https://github.com/rexyang-ai/AHLLMProxyRelease/releases/latest)** → `AHLLMProxy-x.y.z-setup.exe`

- 双击安装，可选择任意目录（如放到 U 盘就是便携版）
- 首次启动 30 秒后自动检查更新，也可在「设置 → 更新」手动检查
- **数据保留**：升级安装自动保留你的所有配置、日志与备份

### macOS / Linux
从 [Releases](https://github.com/rexyang-ai/AHLLMProxyRelease/releases/latest) 页面下载：
- `AHLLMProxy-x.y.z-x64.dmg` / `AHLLMProxy-x.y.z-arm64.dmg`（macOS Intel / Apple Silicon）
- `AHLLMProxy-x.y.z-x64.AppImage`（Linux）

---

## 快速上手

### 1. 添加供应商
「供应商」页 → **新建** → 从模板选择 → 填写你的 API Key → 保存 → 点击 ⚡ 测试连通

### 2. 同步模型
「模型」页 → **同步模型** → 选择供应商 → 一键拉取上游模型列表

### 3. 创建代理模型（可选）
「路由」页 → **新建代理模型** → 起个别名（如 `smart`）→ 选择路由策略 → 添加多个后端模型

### 4. 启动服务
右上角开关 → **服务运行中**

### 5. 客户端接入
```bash
# curl 示例
curl -X POST http://127.0.0.1:11435/v1/chat/completions \
  -H "Authorization: Bearer <你的 Token>" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "smart",
    "messages": [{"role":"user","content":"你好"}],
    "stream": true
  }'
```

或者到「设置 → 客户端配置生成器」一键复制适配 Cursor / Codex / Trae 的配置。

---

## 使用场景

| 场景 | 收益 |
|---|---|
| 同时用多个供应商（DeepSeek + 通义 + Moonshot） | 一份配置管所有客户端，避免每个 IDE 都重新填 Key |
| 想省钱又不想手动切模型 | `cheapest` 策略 + 回退链自动挑最便宜可用的 |
| 团队/多设备共用配置 | 一键脱敏导出 JSON，`authKey` 自动打码后分享 |
| 需要多个 IDE 独立限流/追踪 | 多客户端 Token，日志按 `X-Client-Id` 分组 |
| 上游经常抽风（429/5xx） | 熔断 + 回退，故障自动隔离 10 分钟不影响业务 |
| 自建 Ollama / vLLM / LM Studio | 直接选「OpenAI 兼容」模板加进去即可 |

---

## 常见问题

**Q：数据存在哪里？会不会上传到云端？**
A：全部存在应用安装目录下的 `data/` 文件夹（SQLite + 加密密钥 + 日志）。所有 API 请求都是 IPC 直连上游，不经过任何第三方服务器。

**Q：更新会不会丢配置？**
A：不会。安装程序在覆盖前会把 `data/` 复制到临时中转区，安装完再写回。主动卸载时也**不会**删除 `data/`，需要清理时手动删除安装目录即可。

**Q：为什么不发布便携版？**
A：便携版无法自动更新（技术限制），容易长期停留在有漏洞的旧版本。安装版可以放到任意位置（包括 U 盘），效果与便携版一致但支持自动升级。

**Q：支持流式响应吗？**
A：完整支持 SSE 流式；SSE 中最后一个 chunk 携带 usage 统计，因此 Token 计数在流式模式下依然准确。

**Q：为什么我的中文供应商名调用会报错？**
A：v0.2.4 之前存在这个 bug，请升级到最新版本（HTTP header 现在会自动 ASCII 化）。

---

## 许可与反馈

- **License**: MIT
- **作者**: 辉哥
- **反馈**: [提交 Issue](https://github.com/rexyang-ai/AHLLMProxyRelease/issues) 
- **源码仓库**: [rexyang-ai/AHLLMProxy](https://github.com/rexyang-ai/AHLLMProxy)（暂未开放）
- **发布仓库**: [rexyang-ai/AHLLMProxyRelease](https://github.com/rexyang-ai/AHLLMProxyRelease)（Issue / PR 请发这里，现时存放发行版产物）

---

<p align="center">
  <sub>Made with ♥ using Electron · Vue 3 · Naive UI · Fastify</sub>
</p>

