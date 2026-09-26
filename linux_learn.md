# 需求文档：从 WSL 起步的 Linux 与工程能力学习计划

| 项目 | 内容 |
|---|---|
| 文档版本 | v1.0 |
| 编写日期 | 2026-09-25 |
| 适用对象 | 物联网工程专业大三学生，Linux/云 0 基础 |
| 文档性质 | 需求文档 + 可执行学习路线（两者合一，直接照着做即可） |
| 状态 | 待执行 |

> **怎么用这份文档**：第 1–4 章是"为什么这么排"，用来对齐预期；第 5–8 章是"具体做什么"，照着执行；第 10 章是验收标准，每一阶段结束回来对一遍；第 12 章是进度表，学完一行勾一行。

---

## 1. 背景与现状

### 1.1 个人背景

- 物联网工程专业大三在读，Linux、云计算、服务器方向零基础。
- 学习动机：想理解 Linux 系统的工作方式，掌握软件工程/个人开发的工程流程，并接触嵌入式方向，为求职/实习做准备。
- 起点：已具备 Windows 日常使用能力，能用 git 做基础提交推送，但缺乏命令行与系统层认知。

### 1.2 现有环境（已具备，无需重装）

| 项目 | 现状 |
|---|---|
| 硬件 | 笔记本 i7-14650HX / RTX 4060 / 16GB DDR5 / 双 SSD（932GB + 954GB） |
| 系统 | Windows 11 Pro |
| Linux 环境 | WSL2 + Ubuntu 24.04 |
| Node.js | v24.18.1，手动装在 `~/nodejs`，npm 已配国内镜像 |
| Git | SSH key 已生成并绑定 GitHub（账号 `3682560590h-debug`），本地 → 远程推送链路已打通 |
| 编辑器 | Windows 版 VS Code + Remote-WSL 扩展，已连通到 Ubuntu-24.04 |
| 代理 | Clash Verge，端口 7897，已开 Allow LAN，WSL 内可走代理 |
| AI 辅助 | codex CLI 已装在 `~/nodejs/bin/codex` |

**结论：环境准备阶段已经完成，可以直接进入学习阶段。** 这是很多新手卡最久的地方，你已经跳过了。

### 1.3 问题陈述

需要一份**有明确阶段划分、有可验收产出物、资源经过实测可用**的学习计划，解决以下问题：

1. 不知道从哪开始，容易陷入"收藏了一堆教程但没动手"。
2. 不清楚 WSL 的能力边界，可能在错误的工具上浪费时间（尤其是嵌入式）。
3. 没有产出物目标，学完无法证明自己会了，求职时拿不出东西。

---

## 2. 目标

### 2.1 总目标

在 **6 个月内**（主线 16 周 + 嵌入式延伸 6 周），建立起"能用 Linux 完成真实开发与部署工作"的能力，并产出可展示的作品集，支撑大三下的暑期实习与大四上的秋招。

### 2.2 可验收的分项目标

| 编号 | 目标 | 对应阶段 |
|---|---|---|
| G1 | 不查资料完成文件操作、权限管理、进程管理、文本处理与管道组合 | Phase 1 |
| G2 | 独立写出 50 行以上、带参数与错误处理的 Shell 脚本，并能设置定时任务 | Phase 2 |
| G3 | 独立完成分支开发 → 推送 → PR → 合并的完整协作流程，能解决冲突 | Phase 3 |
| G4 | 做出一个带依赖管理、测试、README 的 CLI 小工具，他人可按文档一次跑通 | Phase 4 |
| G5 | 在公网云服务器上部署一个可访问的服务（含反向代理与 HTTPS） | Phase 5 |
| G6 | 完成"传感器 → 单片机 → MQTT → 可视化"端到端物联网小项目 | Phase 6 |
| G7 | GitHub 上保持 3–5 个有完整 README 的作品仓库 | 贯穿全程 |

### 2.3 明确不做的（Non-goals）

避免范围蔓延，以下内容**本计划不覆盖**，不要在这 6 个月里分心：

- 内核源码分析、驱动开发（需要真机/虚拟机环境，且求职性价比在本科阶段偏低）。
- Kubernetes、微服务架构等进阶运维内容（先把 Docker 和单机部署吃透）。
- 同时学多门语言。**只保留 Python（或你已会的语言）+ C 语言基础**，不要 Python/Go/Rust 并行。
- 图形界面 Linux 桌面折腾。WSL 里专注命令行。

---

## 3. 约束条件

| 约束 | 具体内容 | 对计划的影响 |
|---|---|---|
| 时间 | 每周 5–8 小时 | 主线 16 周（约 4 个月）+ 嵌入式 6 周，总计约 5.5–6 个月。**不建议加速**，5–8 小时/周已是可持续上限 |
| 硬件 | 无嵌入式开发板 | Phase 6 需要先采购（见第 8 章，预算 ¥100 以内可起步） |
| 环境 | WSL2，非真机 Linux | 有明确能力短板，见第 4 章 |
| 网络 | 国内网络 | GitHub / Docker Hub 等需走已有代理；apt/pip 建议换国内镜像 |
| 目标 | 求职/实习导向 | 侧重工程化能力与可展示产出，弱化纯理论 |

---

## 4. 环境定位：WSL 能做什么、不能做什么

**这一章是本计划最重要的部分之一。** WSL2 是运行在轻量虚拟机里的**真实 Linux 内核**，它不是模拟器，但也不是完整的独立 Linux 机器。搞错边界会在错误的地方浪费大量时间。

| 能力 | WSL2 支持情况 | 说明与替代方案 |
|---|---|---|
| 命令行、文件系统、权限 | ✅ 完整可用 | 学习主体，够用 |
| `apt` 包管理、开发工具链 | ✅ 完整可用 | |
| Git、SSH、脚本、cron | ✅ 完整可用 | |
| Docker | ✅ 可用 | 装 Docker Desktop 或 WSL 内原生 Docker |
| 网络服务、监听端口 | ✅ 基本可用 | 端口转发到 Windows 基本透明 |
| systemd 服务管理 | ⚠️ 需手动开启 | 需在 `C:\Users\chunlinYm\.wslconfig` 加 `[wsl2]` + `systemd=true`，否则 `systemctl` 不可用。**这是学服务管理的前提，建议 Phase 5 前开启** |
| 真实启动流程 / GRUB / 内核模块 | ❌ 不可用 | 用云服务器补齐，或装本地虚拟机 |
| USB / 串口设备直连 | ❌ 默认不行 | 需 `usbipd-win` 转发（有教程但坑多），**实践中建议直接在 Windows 侧做** |
| JTAG / ST-Link 硬件调试 | ❌ 不可用 | 嵌入式硬件实验一律在 Windows 侧做 |
| 图形界面 | ⚠️ WSLg 可用 | 能用，但不要依赖，专注命令行 |
| `/mnt/c` 下的文件权限 | ⚠️ 语义不同 | DrvFs 的权限是模拟的，别用它学 `chmod`/`chown`，要在 `~/` 下的 ext4 文件系统里练 |

**核心结论（三条）：**

1. **WSL 学"壳"——足够。** 用户空间、命令行、开发工具链、脚本、Git、Docker，这些是求职面试和日常开发的实际内容，WSL 完全够用。
2. **WSL 学"系统层"——不够。** 启动流程、systemd、内核模块、服务编排的真实感，需要一台云服务器（推荐路线，你原计划就是这样）或本地虚拟机。
3. **WSL 做"硬件"——别硬刚。** 嵌入式一律在 Windows 侧用 Keil / STM32CubeIDE / VS Code + PlatformIO 做，不要试图让 WSL 直连串口。

这个边界正好对应你的三件事：**WSL 学命令行与开发流程 → 云服务器补系统运维 → Windows 侧做嵌入式。**

---

## 5. 学习路线总览

| 阶段 | 周次 | 主题 | 核心产出物 |
|---|---|---|---|
| Phase 1 | W1–W4 | Linux 命令行与文件系统基础 | 通关 Bandit 15 关；能把日常工作搬进命令行 |
| Phase 2 | W5–W6 | Shell 脚本与自动化 | 3 个脚本，其中 1 个 50 行以上并发布到 GitHub |
| Phase 3 | W7–W8 | Git 分支与协作工作流 | 1 个含多 commit、走过 PR 流程的仓库 |
| Phase 4 | W9–W11 | 开发工程化与调试 | 一个带测试和 README 的 CLI 工具 |
| Phase 5 | W12–W16 | 网络、服务、Docker 与部署 | 云服务器上一个公网可访问的服务 |
| Phase 6 | W17–W22 | 嵌入式入门（需先买板） | 端到端物联网小项目 + 接线图 |
| 贯穿 | 全程 | 算法练习 / 笔记 / 作品集 | LeetCode 记录 + 每阶段一篇总结 |

**每周时间分配建议（以 6 小时为例）：**

- 3.5 小时：主线动手练习（写代码、敲命令，占大头）
- 1.5 小时：看教程/文档
- 1 小时：笔记整理 + 算法题 1–2 道

---

## 6. 分阶段详细内容

### Phase 1（W1–W4）：Linux 命令行与文件系统基础

**目标**：把"用鼠标做文件操作"的习惯，换成"用命令行做"，并理解权限与路径的本质。

**知识点清单：**

- 目录结构与 FHS 规范：`/etc` `/var` `/home` `/usr` `/tmp` 各自干什么
- 路径概念：绝对路径 vs 相对路径、`.` `..` `~` `-`
- 文件操作：`ls` `cd` `pwd` `cp` `mv` `rm` `mkdir` `touch` `ln`（软/硬链接区别）
- 查看文件：`cat` `less` `head` `tail` `wc`
- 查找与过滤：`grep` `find` `which`
- 权限：`rwx`、`u/g/o`、`chmod`（含数字表示法）、`chown`、`umask`
- 重定向与管道：`>` `>>` `<` `|` `2>` `&>`
- 通配符与引用：`*` `?` `{}` 与单引号/双引号的区别
- 环境变量与 `PATH`：`echo $PATH`、`export`、`.bashrc` 与 `.profile` 的区别（**你这台机器上已有真实案例：Node 的 PATH 配在 `.profile` 而不是 `.bashrc`，Phase 1 结束你应该能解释为什么**）
- 帮助系统：`man`、`--help`、`tldr`
- 进程基础：`ps` `top` `kill`

**动手任务：**

1. **主力任务**：通关 OverTheWire Bandit Level 0–15。这是一个纯命令行的闯关游戏，每关只给你一个 SSH 登录，必须用命令行找密码进下一关。**这是本阶段最有效的练习方式**，比看视频强得多。
2. 在 WSL 里建一个 `~/practice/` 目录，用它当"实验室"，随意折腾（别在 `/mnt/c` 里练权限）。
3. 每天用命令行完成至少一件事：整理文件、搜索内容、统计行数。
4. 第 4 周：删掉一个不需要的目录树、批量重命名 10 个文件、找出某个目录下最大的 5 个文件——全部用命令行完成。

**验收标准：**

- Bandit 通过 15 关。
- 能不看资料解释：`chmod 755 file` 是什么含义、`>` 和 `>>` 的区别、`~/.bashrc` 和 `~/.profile` 分别在什么时候生效。
- 面试常问：`ls -la` 输出里每一列是什么、软链接和硬链接的区别。

**资源：**

- [The Linux Command Line（中文版）](https://billie66.github.io/TLCL/) — 首选教材，免费，从零讲到脚本。**需代理访问**
- [MIT《缺失的学期》](https://missing.csail.mit.edu/) — 第 1–2 讲讲命令行与 Shell，视角很"工程师"，强烈推荐
- [鸟哥的 Linux 私房菜](https://linux.vbird.org/) — 第 5–9 章，中文经典，讲得更细更啰嗦，适合当参考书
- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/) — 闯关练习
- [菜鸟教程 Linux](https://www.runoob.com/linux/linux-tutorial.html) — 速查手册性质

---

### Phase 2（W5–W6）：Shell 脚本与自动化

**目标**：从"手动敲命令"升级到"写脚本让机器自己干"。

**知识点清单：**

- 脚本基本结构：shebang `#!/bin/bash`、执行权限、`bash script.sh` vs `./script.sh`
- 变量与引用、命令替换 `$(...)`、算术运算
- 条件判断：`if`、`[[ ]]`、文件测试 `-f` `-d` `-x`
- 循环：`for`、`while`、遍历文件
- 函数、参数 `$1` `$@` `$#`
- 退出码与错误处理：`$?`、`set -e`、`set -u`、`set -x`、`trap`
- 输入：`read`、here-doc `<<EOF`
- 定时任务：`cron`、`crontab -e`、cron 表达式

**动手任务（三个脚本，逐级加难）：**

1. **文件整理脚本**：把 `~/Downloads` 里的文件按扩展名分类到子目录（图片/文档/压缩包/其他），处理文件名含空格的情况。
2. **备份脚本**：把指定目录打包成带日期的 `.tar.gz`，保留最近 7 份自动删除旧的。加 `set -euo pipefail`，用 `cron` 每天跑一次。
3. **环境初始化脚本**：一键在新机器上装好你常用的工具、配置 `.bashrc`、配镜像源。**这个脚本以后每次重装环境都能用，是真实工程习惯。**

**验收标准：**

- 有一个 50 行以上、带参数解析和错误处理、能处理异常输入（比如目录不存在）的脚本。
- 能解释 `set -euo pipefail` 每个选项的作用。
- 脚本发到 GitHub，带 README 说明用法。

**资源：**

- [MIT《缺失的学期》](https://missing.csail.mit.edu/) 第 2–3 讲（Shell 工具与脚本）
- [The Linux Command Line 中文版](https://billie66.github.io/TLCL/) 第 24–36 章（脚本部分）
- [Google Shell 风格指南](https://google.github.io/styleguide/shellguide.html) — 学怎么写"像样"的脚本
- 小技巧：写脚本时用 [ShellCheck](https://www.shellcheck.net/) 检查（WSL 里 `apt install shellcheck` 即可）

---

### Phase 3（W7–W8）：Git 分支与协作工作流

**说明**：你已经打通了 `add → commit → push`，所以这两周**不是学基础命令**，而是学**分支模型和协作流程**——这是求职面试的必问项，也是团队开发的实际方式。

**知识点清单：**

- 分支的本质（指针，不是复制文件夹）、`branch` `checkout`/`switch` `merge`
- `rebase` 与 `merge` 的区别及各自适用场景
- 冲突的产生与解决（手动编辑冲突标记、`git status` 的引导）
- 远程协作：`remote`、`fetch` vs `pull`、`push`、追踪分支
- `.gitignore`、`git rm --cached`（提交了不该提交的文件怎么办）
- 撤销三件套：`restore`（改工作区）、`reset`（改暂存区/历史）、`revert`（安全地抵消已推送的提交）
- PR（Pull Request）流程、code review 概念、commit message 规范（推荐 Conventional Commits）
- `tag` 与版本号

**动手任务：**

1. 建一个练习仓库，走完整流程：开 feature 分支 → 多次 commit → push → 在 GitHub 上发 PR → 自己 review → merge。
2. **故意制造一次冲突**：两个分支改同一个文件的同一行，然后手动解决。这一步必须做，不制造冲突永远学不会。
3. 用 `git rebase` 把 3 个零碎 commit 合并成 1 个清晰的 commit。
4. 练习把误提交的大文件/密钥从历史中移除（用 `git rm --cached` + 改 `.gitignore`）。

**验收标准：**

- 能画图解释 `merge` 和 `rebase` 对提交历史的影响有什么区别。
- 能不看资料说清 `reset --soft` / `--mixed` / `--hard` 各自改动了哪一层。
- GitHub 上有 1 个走过完整 PR 流程的仓库。

**资源：**

- [Pro Git（中文版）](https://git-scm.com/book/zh/v2) 第 2–3 章 — 官方权威，免费，**可直连**
- [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN) — 可视化交互式练习，玩通关基本就懂了，**可直连**
- [Conventional Commits 规范](https://www.conventionalcommits.org/zh-hans/v1.0.0/)

---

### Phase 4（W9–W11）：开发工程化与调试

**目标**：从"能写出能跑的代码"升级到"能做出别人能用的项目"。

**知识点清单：**

- **进程管理**：`ps` `top` `htop` `jobs` `fg` `bg` `&` `nohup` `kill -9` vs `kill -15`、信号概念
- **包管理与环境隔离**：Ubuntu `apt`、Python `venv` + `pip` + `requirements.txt`、Node `npm` + `package.json`（你已装 Node）
- **调试手段**：Python `pdb` / VS Code 断点调试、`gdb` 基础（为嵌入式铺垫）、日志分级、`strace`（看系统调用，理解"程序到底在干什么"）
- **构建工具**：`make` 和 Makefile 基础（嵌入式开发必备）
- **配置文件管理**：环境变量、`.env` 文件、配置与代码分离、敏感信息不入库
- **代码质量**：格式化工具（`black`/`prettier`）、linter、基础单元测试（`pytest`）

**动手任务：**

做一个 CLI 小工具，主题自选（推荐选**和你专业或兴趣相关的**，面试时更有话说）。例如：

- 一个把某类文件信息汇总成表格的工具
- 一个简单的端口扫描/网络探测小工具（仅限你自己机器，注意授权边界）
- 一个定时抓取某项数据并生成报告的脚本（可以复用你 Phase 2 的成果）

**硬性要求：**

- 用 `venv` 隔离环境，依赖写进 `requirements.txt`
- 至少 3 个单元测试
- README 包含：项目简介、安装步骤、使用示例、截图或输出样例
- **验收方式：开一个全新的 WSL shell（或让 AI 帮你模拟一个陌生人），只按 README 操作，能一次跑通。** 跑不通就改 README。

**资源：**

- [MIT《缺失的学期》](https://missing.csail.mit.edu/) 第 4–6 讲（数据整理、命令行环境、版本控制）
- [阮一峰的网络日志](https://www.ruanyifeng.com/blog/) — 遇到具体概念（如 systemd、`.env`）时可搜
- [C 语言中文网](https://c.biancheng.net/) — 补 C 语言基础，为嵌入式铺路
- [Makefile 教程（阮一峰）](https://www.ruanyifeng.com/blog/2015/02/make.html)

---

### Phase 5（W12–W16）：网络、服务、Docker 与部署

**这是求职含金量最高的阶段。** 它把前面所有东西串起来，产出"能公网访问的服务"——面试时这是最有说服力的一段经历。

**开工前必做**：开启 WSL 的 systemd（否则 `systemctl` 用不了）。

在 `C:\Users\chunlinYm\.wslconfig` 写入：

```ini
[wsl2]
systemd=true
```

然后 PowerShell 执行 `wsl --shutdown` 重启 WSL，进去后 `systemctl --version` 有输出即成功。

**知识点清单：**

- **网络基础**：IP、端口、DNS 解析过程、TCP vs UDP、HTTP 请求/响应结构
- **网络排查工具**：`curl`（重点）、`ss`（看监听端口）、`ping` `traceroute` `dig`、`ip addr`
- **systemd**：`systemctl start/stop/status/enable`、写一个自己的 `.service` 单元文件、`journalctl` 看日志
- **反向代理**：Nginx 基础配置、反向代理与负载均衡概念
- **Docker**：镜像 vs 容器、`Dockerfile` 编写、数据卷、网络、`docker compose` 编排多容器
- **云服务器**：SSH 密钥登录、安全组/防火墙、`ufw`、域名解析、HTTPS 证书（Let's Encrypt / `certbot`）
- **消息中间件**：MQTT 协议概念、EMQX 或 Mosquitto broker 部署（**衔接你的物联网专业**）

**动手任务（按顺序）：**

1. **本地**：装 Docker，把 Phase 4 的 CLI 工具打包成镜像跑起来。
2. **本地**：用 `docker compose` 编排两个服务（例如你的工具 + 一个数据库）。
3. **云**：买一台轻量云服务器（新用户通常有很便宜的入门套餐，学生认证还有优惠）。SSH 登录，配好防火墙，装 Docker。
4. **云**：把服务部署上去，装 Nginx 做反向代理，申请域名或直接用 IP + 端口访问。
5. **云**：部署 EMQX，用 `mosquitto_pub` / Python 脚本发布订阅一条消息，验证链路通。
6. **可选加分**：配 HTTPS 证书、写一个 systemd 单元文件让服务开机自启、用 GitHub Actions 做自动部署。

**验收标准：**

- 有一个**公网可访问的 URL**（记下来，写进简历）。
- 能解释：为什么需要反向代理、容器和虚拟机的区别、`docker compose` 解决了什么问题。
- 能独立在云服务器上排查"服务起不来"的问题（看 `systemctl status` → 看 `journalctl` → 看端口占用 → 看防火墙）。

**资源：**

- [Docker 从入门到实践（中文）](https://yeasy.gitbook.io/docker_practice) — 免费在线书，中文最佳
- [Docker 官方 Get Started](https://docs.docker.com/get-started/)
- [EMQX 中文文档](https://docs.emqx.com/zh/emqx/latest/) — MQTT broker，中文文档很完整
- [阮一峰 systemd 教程（命令篇）](https://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html) 与 [实战篇](https://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)
- [清华镜像站 Ubuntu 使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/) — apt 换源，国内提速必备
- [Nginx 官方文档](https://nginx.org/en/docs/) — 英文，但配置片段可直接抄
- [WSL 官方文档（中文）](https://learn.microsoft.com/zh-cn/windows/wsl/) — 配 `.wslconfig` 时查，**需代理访问**
- [GitHub Actions 中文文档](https://docs.github.com/zh/actions) — 自动部署用

---

### Phase 6（W17–W22）：嵌入式入门

**前提**：需要先买开发板，见第 8 章。

**再次强调环境边界**：嵌入式实验**在 Windows 侧做**，不要试图让 WSL 直连串口。WSL 只用来写代码、管 Git、跑 MQTT broker。

**知识点清单：**

- 微控制器（MCU）vs 嵌入式 Linux 的区别，各自适用场景
- GPIO 概念：输入/输出、上拉/下拉、PWM
- 通信总线：UART（串口）、I2C、SPI——各自速度、线数、典型设备
- 交叉编译概念：为什么在电脑上编译、在板子上跑
- 开发框架：ESP-IDF（官方 C 框架）或 Arduino/PlatformIO（上手更快）
- RTOS 基础概念：任务、调度、看门狗（了解即可，不深究）
- 联网：WiFi 连接、MQTT 客户端发布/订阅、JSON 数据格式
- 低功耗概念（物联网关键词，面试会问）

**动手任务（逐级递进）：**

1. **点灯 + 串口打印**：让板子上的 LED 闪烁，并通过串口输出文字。跑通工具链。
2. **读传感器**：接一个温湿度传感器（DHT22 或 BME280），把读数通过串口打印出来。
3. **联网上报**：连上 WiFi，把传感器数据通过 MQTT 发布到 Phase 5 部署的 broker。
4. **端到端闭环**：broker 收到数据后存储，做一个简单可视化（网页、或 Grafana）。**这一步做完，你就有了一个完整的物联网项目，可以直接写进简历。**
5. **加分**：加一个继电器/舵机实现反向控制（云端下发指令 → 板子动作），做成双向。

**验收标准：**

- 一个端到端 demo：真实传感器数据 → 你能在网页上看到曲线。
- 仓库含：代码、接线图（照片或 Fritzing 图）、README（说明硬件清单、接线、如何复现）。
- 能解释：为什么选 MQTT 而不是 HTTP 做设备上报（提示：功耗、连接开销、QoS）。

**资源：**

- [ESP-IDF 中文文档](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/) — 乐鑫官方，中文完整
- [Random Nerd Tutorials](https://randomnerdtutorials.com/) — ESP32 实战教程最全的英文站，代码可直接抄，**可直连**
- [PlatformIO 文档](https://docs.platformio.org/en/latest/) — VS Code 里的嵌入式开发插件，比 Arduino IDE 专业
- [MQTT 官方](https://mqtt.org/) — 协议规范与概念
- [C 语言中文网](https://c.biancheng.net/) — 补 C 语言，嵌入式绕不开 C

---

## 7. 贯穿全程的三件事

这三件事不单独占阶段，但要每周固定投入，否则求职时会吃亏。

### 7.1 算法练习（每周 1–2 小时，2–3 题）

求职面试的第一道门槛。**不要一开始啃难题**，按分类刷：

- W1–W8：数组、字符串、哈希表、双指针（LeetCode 简单 + 部分中等）
- W9–W16：链表、栈队列、二叉树、排序二分
- 大四前刷到 150–200 题即可，重点是**每道题都自己写出来并且能讲清思路**，不要看题解抄一遍

资源：[代码随想录](https://programmercarl.com/)（中文题解体系完整）、[Hello 算法](https://www.hello-algo.com/)（图解数据结构，适合零基础）、[LeetCode 中国站](https://leetcode.cn/)

### 7.2 笔记与输出（每周 0.5 小时）

每个阶段结束写一篇总结，发到 GitHub 或博客。标题用"我如何解决 X 问题"这种形式。

**为什么必须做**：面试时"我遇到过 X 问题，排查过程是 Y"比"我学过 Z"有说服力得多。笔记就是把经历变成弹药的过程。

### 7.3 作品集维护

- GitHub 上保持 **3–5 个**有完整 README 的仓库（Phase 2 / 4 / 5 / 6 各一个）。
- 每个 README 必须有：项目做什么、怎么跑起来、截图或输出样例、遇到的难点。
- 简历上放的是**公网可访问的 URL**，不是"熟悉 Linux"这种形容词。

---

## 8. 嵌入式硬件选型建议

**你没有开发板，这一章解决买什么的问题。**

### 8.1 先明确一件事：走哪条路线

| 路线 | 内容 | 门槛 | 薪资水平 | 建议 |
|---|---|---|---|---|
| **A. IoT 应用向** | ESP32 + 传感器 + 云平台 | 低，会 C 基础即可 | 中等 | **推荐你先走这条** |
| **B. 嵌入式 Linux 向** | 树莓派/香橙派 + 驱动 + 交叉编译 | 高，需要扎实 C + Linux | 较高 | 大四确定方向后再上 |

理由：你的目标是求职，路线 A 可以在 **2 周内出成果**，而且和你物联网专业、已学的 MQTT/云部署直接衔接；路线 B 需要先有 C 和 Linux 底子，现在上容易挫败。**先用 A 拿到正反馈和简历素材，再考虑 B。**

### 8.2 路线 A 采购清单（总预算 ¥100 以内）

| 物品 | 说明 | 大致价格 |
|---|---|---|
| ESP32 开发板 ×1 | **首选**。自带 WiFi + 蓝牙，USB 线一插就能用，生态和教程最丰富。可选：立创 ESP32-S3 开发板、正点原子/野火 ESP32 开发板（带教程）、合宙 ESP32-C3 核心板 | ¥15–60 |
| 面包板 + 杜邦线 | 免焊接接线 | ¥10–20 |
| 温湿度传感器 | DHT22（便宜）或 BME280（更准，I2C 接口，推荐） | ¥5–25 |
| LED + 电阻若干 | 第一个实验用 | ¥5 |
| USB 数据线 | **注意要能传数据的，不是只能充电的** | 家里大概率有 |

**不建议现在买**：正点原子/野火的旗舰级套件（¥300–600），内容太多太全，容易吃灰。先用最小套装跑通 4 个实验，确定有兴趣再扩容。

### 8.3 路线 B 参考（暂不采购）

如果想做嵌入式 Linux，性价比排序：

- **香橙派（Orange Pi）Zero 3** — 全志 H618，价格通常 ¥150–250 区间，比树莓派便宜很多
- **树莓派 4B / 5** — 生态最好，但国内价格近年偏高（¥400–800），二手 3B+ 约 ¥150–250
- **二手开发板** — 闲鱼上有便宜的，但要注意是否被锁

> **价格提醒**：以上为大致区间，电商价格波动大，下单前请自行核对当前价格。我只做选型方向建议，不保证具体价格。

### 8.4 开发环境（Windows 侧）

- **入门推荐**：VS Code + **PlatformIO** 插件（比 Arduino IDE 专业，比 ESP-IDF 易上手）
- **进阶**：ESP-IDF（官方 C 框架，求职更认可）
- 你已经有 Windows 版 VS Code，直接装插件即可，**不需要在 WSL 里做**

---

## 9. 资源清单（已实测可达性）

> 探测时间：2026-09-25。标注依据是当时从你这台机器的实测结果，其中"需代理"指直连不通、走 Clash 代理可通。

### 9.1 核心教材

| 资源 | 链接 | 访问 | 用途 |
|---|---|---|---|
| MIT《缺失的学期》 | https://missing.csail.mit.edu/ | 可直连 | **最推荐的工程视角入门课** |
| 中文翻译仓库 | https://github.com/missing-semester-cn/missing-semester-cn.github.io | 需代理 | 上者的中文版源码（渲染站点本次探测不通） |
| The Linux Command Line 中文版 | https://billie66.github.io/TLCL/ | 需代理 | Linux 命令与脚本主力教材 |
| 鸟哥的 Linux 私房菜 | https://linux.vbird.org/ | 可直连 | 中文经典参考书 |
| Pro Git 中文版 | https://git-scm.com/book/zh/v2 | 可直连 | Git 权威教材 |
| Docker 从入门到实践 | https://yeasy.gitbook.io/docker_practice | 可直连 | Docker 中文最佳 |

### 9.2 练习与实战

| 资源 | 链接 | 访问 | 用途 |
|---|---|---|---|
| OverTheWire Bandit | https://overthewire.org/wargames/bandit/ | 可直连 | 命令行闯关，Phase 1 主力 |
| Learn Git Branching | https://learngitbranching.js.org/?locale=zh_CN | 可直连 | Git 可视化练习 |
| 代码随想录 | https://programmercarl.com/ | 可直连 | 算法题解体系 |
| Hello 算法 | https://www.hello-algo.com/ | 可直连 | 图解数据结构 |
| LeetCode 中国站 | https://leetcode.cn/ | 可直连 | 刷题 |
| Random Nerd Tutorials | https://randomnerdtutorials.com/ | 可直连 | ESP32 实战（英文） |

### 9.3 官方文档与工具

| 资源 | 链接 | 访问 | 用途 |
|---|---|---|---|
| Docker 官方 Get Started | https://docs.docker.com/get-started/ | 可直连 | Docker 官方入门 |
| GitHub Actions 中文文档 | https://docs.github.com/zh/actions | 可直连 | CI/CD |
| EMQX 中文文档 | https://docs.emqx.com/zh/emqx/latest/ | 可直连 | MQTT broker |
| ESP-IDF 中文文档 | https://docs.espressif.com/projects/esp-idf/zh_CN/latest/ | 可直连 | 乐鑫官方 |
| PlatformIO 文档 | https://docs.platformio.org/en/latest/ | 可直连 | 嵌入式开发插件 |
| 阮一峰 systemd 教程 | https://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html | 可直连 | 服务管理 |
| 清华镜像站帮助 | https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/ | 可直连 | apt 换源 |
| WSL 官方文档中文 | https://learn.microsoft.com/zh-cn/windows/wsl/ | 需代理 | WSL 配置 |
| usbipd-win | https://github.com/dorssel/usbipd-win | 需代理 | WSL 串口转发（备查，不推荐走这条路） |
| C 语言中文网 | https://c.biancheng.net/ | 可直连 | 补 C 语言 |
| 菜鸟教程 Linux | https://www.runoob.com/linux/linux-tutorial.html | 可直连 | 速查 |
| 阮一峰网络日志 | https://www.ruanyifeng.com/blog/ | 可直连 | 概念速查 |
| ShellCheck | https://www.shellcheck.net/ | 可直连 | Shell 脚本检查 |

### 9.4 本次探测不通（仅作记录）

| 资源 | 链接 | 情况 |
|---|---|---|
| CSAPP 官网 | https://csapp.cs.cmu.edu/ | 直连与代理均不通 |
| Linux Journey | https://linuxjourney.com/ | 返回 403（反爬），浏览器可能可开，未验证 |

---

## 10. 验收标准（Definition of Done）

整个计划完成时，你应该能拿出以下**具体证据**。这是判断"学会没学会"的唯一标准，不是"看完了多少教程"。

| 编号 | 验收项 | 证据形式 | 状态 |
|---|---|---|---|
| D1 | Linux 命令行熟练 | Bandit 通关记录；能不看资料解释权限/重定向/PATH | ☐ |
| D2 | Shell 脚本能力 | GitHub 上一个 50+ 行脚本，带 README | ☐ |
| D3 | Git 协作能力 | 一个走过的完整 PR 的仓库 | ☐ |
| D4 | 工程化能力 | 一个带测试与 README 的 CLI 工具 | ☐ |
| D5 | 部署能力 | **一个公网可访问的 URL** | ☐ |
| D6 | 容器能力 | 一个 Dockerfile + 一份 docker-compose.yml | ☐ |
| D7 | 嵌入式能力 | 端到端物联网项目（含接线图） | ☐ |
| D8 | 算法 | LeetCode 150+ 题记录 | ☐ |
| D9 | 输出能力 | 每阶段一篇总结，共 6 篇 | ☐ |
| D10 | 简历可写 | 3–5 个有 README 的 GitHub 仓库 | ☐ |

---

## 11. 风险与坑（提前知道能省很多时间）

| 风险 | 说明 | 对策 |
|---|---|---|
| **把 WSL 当完整 Linux** | 面试被问"WSL 和真机 Linux 有什么区别"会答不上来 | 用云服务器补系统层体验（Phase 5）；记住第 4 章那张表 |
| **试图在 WSL 里做嵌入式** | USB/串口不通，JTAG 更不可能，会卡死在环境配置上 | 硬件实验一律 Windows 侧做 |
| **只看不练** | 本计划最大的风险。看教程会产生"我会了"的错觉 | 每阶段必须有产出物，第 10 章的 D1–D10 就是防这个 |
| **时间碎片化** | 每周 5–8 小时如果拆成每天 40 分钟，效果远差于两个 3 小时整块 | 固定时段，例如周三晚 + 周日下午 |
| **国内网络** | GitHub / Docker Hub 拉取慢或失败 | 已有 Clash 代理；另配清华/中科大镜像加速 apt 与 pip |
| **贪多求全** | 同时学 Python+C+Go，或同时学 K8s+嵌入式，结果都半途而废 | 严格执行第 2.3 节的 Non-goals |
| **买贵设备吃灰** | 一上手买 ¥600 套件，跑完点灯就放着 | 先按第 8 章买 ¥100 以内的最小套装 |
| **虚拟化驱动冲突** | 你这台机器有过卡巴斯基 `klwtp.sys` 导致 0xD1 蓝屏的历史。若后续为了学系统层装 VirtualBox/VMware，可能触发类似驱动冲突 | 优先用**云服务器**代替本地虚拟机；必须装 VM 时先确认杀软版本，装完观察是否蓝屏 |
| **Docker Desktop 授权** | Docker Desktop 对个人/小团队免费，但企业使用有授权限制 | 个人学习完全没问题，注意即可 |

---

## 12. 进度追踪表

每周学完打个勾。**如果连续两周空着，回来重看第 2.3 节和第 11 章。**

| 周次 | 阶段 | 本周目标 | 完成 |
|---|---|---|---|
| W1 | P1 | 目录结构、路径、基础文件操作；Bandit 0–4 | ☐ |
| W2 | P1 | 查看/查找文件、grep/find；Bandit 5–9 | ☐ |
| W3 | P1 | 权限、chmod/chown、重定向与管道；Bandit 10–13 | ☐ |
| W4 | P1 | 环境变量、PATH、进程基础；Bandit 14–15；命令行实战任务 | ☐ |
| W5 | P2 | Shell 基础语法；写文件整理脚本 | ☐ |
| W6 | P2 | 错误处理、cron；写备份脚本 + 环境初始化脚本 | ☐ |
| W7 | P3 | 分支模型、merge/rebase；走一次完整 PR 流程 | ☐ |
| W8 | P3 | 冲突解决、撤销操作；故意制造并解决冲突 | ☐ |
| W9 | P4 | 进程管理、包管理、venv 环境隔离 | ☐ |
| W10 | P4 | 调试手段、make、配置管理；开始写 CLI 工具 | ☐ |
| W11 | P4 | 测试、README；陌生人视角验收 | ☐ |
| W12 | P5 | 网络基础与排查工具；开 WSL systemd | ☐ |
| W13 | P5 | systemd 服务、Nginx 反向代理；Docker 打包 | ☐ |
| W14 | P5 | docker compose；购买并配置云服务器 | ☐ |
| W15 | P5 | 云上部署 + Nginx 反代 + 公网访问 | ☐ |
| W16 | P5 | 部署 EMQX，打通 MQTT；可选 HTTPS/自动部署 | ☐ |
| W17 | P6 | 买板、搭环境、点灯 + 串口 | ☐ |
| W18 | P6 | GPIO、I2C/SPI 概念；读传感器 | ☐ |
| W19 | P6 | WiFi 连接、MQTT 客户端 | ☐ |
| W20 | P6 | 数据上报到自己的 broker | ☐ |
| W21 | P6 | 存储 + 可视化 | ☐ |
| W22 | P6 | 完善 README/接线图；可选双向控制 | ☐ |

---

## 附录 A：今天就能开始的三个动作

不用等，现在就打开 WSL 终端做这三件事：

```bash
# 1. 建一个学习实验室目录（以后所有练习都放这里，别在 /mnt/c 下练）
mkdir -p ~/practice && cd ~/practice && pwd

# 2. 看看你的 Linux 是什么样子
cat /etc/os-release          # 系统版本
uname -a                     # 内核信息
df -h                        # 磁盘使用情况
echo $PATH                   # 你的 PATH（注意里面 ~/nodejs/bin 是怎么来的）

# 3. 安装 Bandit 练习需要的 ssh 客户端，注册第一关
sudo apt update && sudo apt install -y openssh-client
# 然后打开 https://overthewire.org/wargames/bandit/bandit0.html 开始第一关
```

**关于 PATH 的一个悬案，留着当 Phase 1 的作业**：你的 Node 是手动解压到 `~/nodejs` 的，PATH 配置写在了 `~/.profile` 而不是 `~/.bashrc`。请在第 4 周之前搞清楚——为什么 `.bashrc` 里配会不生效？`.profile` 和 `.bashrc` 分别在什么场景下被加载？**能自己讲明白这一题，说明你已经理解 Linux 的 shell 启动流程了。**

---

## 附录 B：本计划的时间线假设（需你确认）

文档按以下假设排期，**如果与你的实际情况不符，请自行调整阶段顺序**：

- 你当前大三上学期（2026 年秋），预计 2028 年毕业。
- 按常规节奏：**2027 年春季（约 3 月）** 开始投递暑期实习，**2027 年秋季（约 9 月）** 进入秋招。
- 因此本计划的目标是两个节点：**约 6 个月做到"可投实习"**（能拿出 D1–D6），**约 12 个月做到"可投校招"**（D1–D10 全达标）。

**如果你的毕业年份或求职节奏不同，请把 6/12 个月的里程碑按比例平移，但不要压缩 Phase 1–3 的动手量——基础不牢后期全是坑。**

---

*本文档由 Claude Code 生成于 2026-09-25，资源链接的可达性基于当日从本机实测，网络环境变化后可能失效。*
