<div align="center">

# 节点工作台 · 精简版 (NodeWorkbench Lite)

**面向 Windows 用户的 Clash Verge & Clash Meta 节点筛选、流媒体/IP 风险检测与可视化订阅管理工作台**

[![Platform](https://img.shields.io/badge/Platform-Windows%20x64-0078D6?style=flat-square&logo=windows)](#快速开始)
[![Mihomo](https://img.shields.io/badge/Core-Mihomo%20v1.19.34-10b981?style=flat-square)](https://github.com/MetaCubeX/mihomo)
[![Clients](https://img.shields.io/badge/Clients-Clash%20Verge%20%7C%20Clash%20Meta-38bdf8?style=flat-square)](#订阅接口一览)
[![Version](https://img.shields.io/badge/Version-v3.3.13--lite-6366f1?style=flat-square)](#快速开始)

</div>

---

## 简介

**节点工作台 精简版 (NodeWorkbench Lite)** 是一款本地运行的 Windows 桌面工具与订阅管理服务。它基于内置的 **Mihomo** 内核，对你配置的公开订阅源或自定义订阅进行自动去重、端口预筛、HTTPS 测活、双站稳定性复测、流媒体/AI 平台解锁与 IP 风险评分，最终输出经 `mihomo -t` 严格校验的 Clash 配置文件，并通过本机及局域网 HTTP 订阅接口供 **Clash Verge**（电脑端）与 **Clash Meta for Android**（移动端）直接拉取。

> [!NOTE]
> 本仓库/发行版为**精简通用版（Lite）**：仅使用用户配置的预设源与自定义订阅，不包含自动网络爬虫搜源及特定 IDE 专用探测组件；默认独立运行于 `18302` 端口与独立数据目录，开箱即用。

---

## 核心特性

### 1. 六阶段节点筛选流水线与精细中断控制
- **全流程实测**：`订阅抓取去重` → `全池 TCP/QUIC 端口预筛` → `Mihomo 全量 HTTPS 测活` → `Google + Cloudflare 双站三轮稳定性复测` → `流媒体/AI/IP 风险与下载测速` → `Mihomo -t 校验并发布`。
- **同机房去重限流**：自动按同服务器 IP 与同 IPv4 `/24` 网段限制重复马甲节点数量（可在应用设置中自由调节上限），避免脏机房节点刷屏。
- **可控中断机制**：
  - **分阶段中断（`⏭ 中断当前→下一步`）**：任意阶段可随时截断并保留已完成验证的节点进入下一阶段；尚未完成平台探测的节点会自动标记为 `未探(仅HTTPS)` 并归入末尾隔离分组，不污染高质量分组。
  - **一键终止（`⏹ 停止全部`）**：立即终止整轮筛选与测速子进程，无损保留当前正在生效的订阅。

### 2. 多平台解锁、IP 纯净度与紧凑节点命名
- **自动检测项**：出口国家/地区、IP 欺诈风险分（IPPure / Scamalytics）、IP 类型（住宅/机房 · 原生/广播）、1 MiB 实测下载速率，以及常见流媒体与 AI 服务可达性。
- **紧凑英文简称标签（≤3 字母）**：节点名自动标注 `NF`（Netflix 全片库）、`NFO`（Netflix 自制剧）、`DP`（Disney+）、`YT`（YouTube Premium）、`GPT`（OpenAI）、`GM`（Gemini）、`CL`（Claude）、`TK`（TikTok）、`SP`（Spotify），在客户端列表中一目了然。
- **规范代理组排序**：`🛡️ 低风险纯净节点` 位于 `🚀 手动切换` 之前，自动探测能力组与 `url-test` 测速组统一置于末尾，支持一键同步修正所有历史配置。

### 3. 桌面客户端 + Web 可视化大屏双界面
- **原生桌面端**：深浅色自适应主题（支持跟随 Windows / 强制深色 / 强制浅色）、交互式 Canvas 可视化图表（IP 风险纯净度占比、点击式 Top 5 地区分布、HTTPS 延迟直方图）、系统托盘驻留。
- **内置 Web 大屏**：浏览器访问 `http://127.0.0.1:18302/` 即可使用卡片/表格双视图、SVG 环形图与全功能管理面板。
- **高性能低占用**：HTTP/1.1 长连接 + 轻量增量轮询 + 桌面端全异步线程与 O(1) 表格缓存，后台筛选进程自动降为 `BELOW_NORMAL` 优先级，测速期间窗口与系统依然流畅不卡顿。

### 4. 可选节点导入、选项记忆与历史版本回滚
- **自定义勾选导入**：默认全部导入，也可在节点列表自由勾选/取消勾选子集并实时更新生效订阅；完整原始节点始终保留于 `provider.full.yaml`，随时可无损恢复。
- **多选筛选与选项记忆**：`剔除高风险(≥70%)`、`剔除未探测`、`低风险≤35%`、`≥1M`、`已勾导入` 支持高亮多选叠加，状态自动持久化并在重启及后续新一轮筛选发布时自动延续。
- **历史订阅切换与全量同步**：支持随时切换到任意历史已验证版本，或一键按最新规则与代理组顺序同步更新全部历史配置。

### 5. 独立功能区：「🗂 外来梳理归纳」
- **完全隔离于主体筛选流程**，专门用于整理你自己的外部 `.yaml` 配置文件或第三方订阅链接：
  - 支持本地 YAML 文件导入、订阅 URL 拉取或粘贴文本；
  - 代理组可视化新建、编辑、删除、上移/下移/置顶/置底、一键规范排序、一键套用工作台分流规则与能力分组；
  - 支持对外来节点发起独立 HTTPS 测活与流媒体/IP 风险探测、按检测结果自动重命名、剔除不可用或高风险节点；
  - 经 `mihomo -t` 校验后可导出本地 `.yaml`，同时生成独立的本机与手机端 Clash Meta 订阅地址。

### 6. 移动端 Clash Meta 规范兼容与零干扰响应
- 采用无 `?key=` 查询参数的规范路径令牌地址 `/sub/<token>/clash-meta.yaml` 与 `clashmeta://install-config` 一键导入链接。
- 面向移动端自动兼容处理（剥离不兼容字段、转换 `REJECT-DROP`、剔除 Windows `.exe` 进程规则），支持跨网段私网 LAN 访问（RFC1918 / CGNAT）。
- 不返回虚假 `subscription-userinfo` 响应头，订阅卡片界面干净无异常流量条。

### 7. 常用应用设置与系统集成
- **Windows 开机自动启动**（写入当前用户 `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`）与 **`--minimized` 开机静默最小化到托盘**；
- 关闭按钮行为（最小化到托盘 / 直接退出）、启动后是否自动跑筛选、定时自动更新间隔（关闭 / 30m / 1h / 2h / 4h / 6h）；
- 局域网订阅访问总开关、局域网密钥一键重置、历史版本自动保留份数、测速临时缓存与旧日志一键清理。

---

## 工作流程

```mermaid
flowchart LR
    A["订阅源抓取与去重<br/>预设源 + 用户源 + 自定义源"] --> B["全池端口预筛<br/>TCP / QUIC"]
    B --> C["Mihomo 全量<br/>HTTPS 测活"]
    C --> D["双站稳定性复测<br/>Google + Cloudflare"]
    D --> E["同 IP / 24网段<br/>限流去重"]
    E --> F["流媒体 / AI / IP风险<br/>与下载测速"]
    F --> G["Mihomo -t 校验<br/>生成 clash.yaml"]
    G --> H["Clash Verge 本机订阅<br/>& Clash Meta 局域网订阅"]
```

---

## 快速开始

### 方式一：直接使用 Release 单文件（推荐）
1. 前往本仓库 **Releases** 页面下载 `NodeWorkbench_ClashVerge_Lite_v3.3.13.exe`。
2. 双击运行，首次启动选择数据存储目录（默认优先推荐非 C 盘目录，如 `D:\NodeWorkbenchLiteData`，不污染系统盘）。
3. 点击顶部 **「⚡ 一键导入 Clash Verge」**，或在移动端 Clash Meta 中复制局域网订阅地址导入即可使用。
4. 随时可通过顶部按钮或访问 `http://127.0.0.1:18302/` 打开网页可视化大屏。

### 支持的启动参数

| 参数 | 说明 |
| --- | --- |
| `--minimized` | 静默启动并直接最小化到系统托盘（开机自启默认附带此参数） |
| `--choose-dir` | 启动时重新打开数据存储目录选择向导 |
| `--service` | 仅启动后台 HTTP 订阅与筛选服务（无桌面窗口） |
| `--prepare` | 基于最近一次筛选结果重新生成并校验配置 |

---

## 订阅接口一览

默认监听端口为 `18302`（可通过环境变量 `CLASH_NODE_PORT` 修改）：

| 接口路径 | 访问范围 | 用途 |
| --- | --- | --- |
| `http://127.0.0.1:18302/` | 仅本机 | 网页可视化大屏（Web UI） |
| `http://127.0.0.1:18302/subscription/clash-verge.yaml` | 仅本机 | Clash Verge 桌面端完整配置订阅 |
| `http://<LAN_IP>:18302/sub/<token>/clash-meta.yaml` | 局域网（需 Token） | Clash Meta for Android 移动端兼容订阅 |
| `http://127.0.0.1:18302/external/clash.yaml` | 仅本机 | 「外来梳理归纳」独立导出订阅 |
| `http://<LAN_IP>:18302/sub/<token>/external-meta.yaml` | 局域网（需 Token） | 「外来梳理归纳」移动端兼容订阅 |

---

## 目录结构

```text
.
├── VERSION                      # 版本号定义
├── README.md                    # 项目说明文档
├── rules/
│   └── RULES .txt               # 默认分流规则与基础代理组模板
├── src/
│   ├── Launcher.cs              # Windows 单文件 C# 启动器与数据目录向导
│   ├── app.py                   # 本地 HTTP/1.1 订阅与控制服务、发布与同步逻辑
│   ├── app_settings.py          # 应用设置持久化、开机自启注册表与缓存清理
│   ├── capability.py            # 流媒体 / AI / IP 纯净度 / 下载测速探测模块
│   ├── dashboard.py             # Tkinter 原生桌面客户端与系统托盘
│   ├── diagnostics.py           # Clash Verge 状态与本地代理端口检测
│   ├── external_studio.py       # 「外来梳理归纳」独立工作区后端
│   ├── lan.py                   # 局域网私网校验与异步网卡 IP 发现
│   ├── package_payload.py       # 运行时资源与 Mihomo 内核打包脚本
│   ├── pipeline.py              # 多阶段并发筛选、复测、中断与 SQLite 历史统计
│   ├── web_ui.py                # 嵌入式单页 Web 可视化大屏
│   ├── sources.json             # 预设公开订阅源配置
│   └── user-sources.txt         # 用户附加订阅源列表
└── build/
    ├── NodeWorkbenchRuntime.spec # PyInstaller 打包配置
    └── build_all.ps1            # 一键语法检查 + PyInstaller + 资源打包 + C# 编译脚本
```

---

## 从源码构建

### 环境要求
- Windows 10 / 11 (x64)
- Python 3.11+（含 `pyinstaller`、`pyyaml`、`requests`、`pystray`、`pillow`）
- .NET Framework 4.x `csc.exe`（Windows 系统自带）
- 上游依赖组件：Mihomo Windows x64 内核、`Country.mmdb` 以及节点解析依赖

### 构建步骤
1. 根据本机路径调整 `build/build_all.ps1`、`build/NodeWorkbenchRuntime.spec` 与 `src/package_payload.py` 中的 Python 虚拟环境及上游组件路径。
2. 在 PowerShell 中执行：
   ```powershell
   .\build\build_all.ps1
   ```
3. 构建完成后将在 `outputs/` 目录生成单文件可执行程序 `NodeWorkbench_ClashVerge_Lite_v<VERSION>.exe`。

---

## 致谢与上游组件

本项目集成了以下优秀开源项目的核心组件或解析能力：
- [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo) — 高性能多协议代理内核
- [Free-Nodes](https://github.com/735754647/Free-Nodes) & [aggregator](https://github.com/wzdnzd/aggregator) — 代理节点解析与测速基础库

---

## 免责声明

本项目仅供学习、研究与个人网络质量测试使用，软件本身不提供任何代理服务器或节点服务。请严格遵守所在国家或地区的相关法律法规，因不当使用本软件产生的一切后果由使用者自行承担。
