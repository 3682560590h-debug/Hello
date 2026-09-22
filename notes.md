# Linux 与 Git 学习笔记

> 记录于 2026-09-22，配合 WSL (Ubuntu 24.04) 学习使用

## 一、Linux 基本指令

### 1. 文件和目录
| 命令 | 作用 | 示例 |
|------|------|------|
| pwd | 显示当前所在目录 | pwd |
| ls | 列出目录内容 | ls 或 ls -la |
| cd | 切换目录 | cd ~/hello |
| mkdir | 新建目录 | mkdir 项目名 |
| touch | 新建空文件 | touch a.txt |
| cp | 复制文件 | cp a.txt b.txt |
| mv | 移动 / 重命名 | mv a.txt b.txt |
| rm | 删除文件 | rm a.txt（删目录用 rm -r 目录） |
| cat | 查看文件内容 | cat a.txt |

### 2. 文件编辑
| 命令 | 作用 |
|------|------|
| nano | 简单文本编辑器（新手推荐） |
| vim | 强大但学习曲线陡的编辑器 |

### 3. 权限和系统
| 命令 | 作用 |
|------|------|
| sudo | 以管理员身份执行（装软件常用） |
| chmod | 修改文件权限 |
| whoami | 显示当前用户名 |
| uname -a | 显示系统信息 |
| df -h | 查看磁盘空间 |
| free -h | 查看内存 |
| top | 查看运行中的进程 |

### 4. 网络
| 命令 | 作用 |
|------|------|
| ping | 测试网络是否通 |
| curl | 请求网址 / 下载 |
| wget | 下载文件 |

### 5. 查找
| 命令 | 作用 |
|------|------|
| grep | 在文件里搜索内容 |
| find | 查找文件 |
| which | 查找某个命令在哪 |

### 6. 其他常用
| 命令 | 作用 |
|------|------|
| echo | 输出文字 |
| clear | 清屏 |
| history | 查看敲过的命令 |
| man 命令 | 查看命令的手册说明 |

---

## 二、Git 基本指令及作用

### 核心三步（每次改完代码做这三步）
```bash
git add .              # 1. 暂存所有改动
git commit -m "说明"   # 2. 提交（给改动拍快照）
git push               # 3. 推送到 GitHub
```

### 完整指令表
| 命令 | 作用 |
|------|------|
| git init | 把当前目录变成 git 仓库 |
| git clone 地址 | 下载远程仓库到本地 |
| git status | 查看改了什么（最常用，随时看） |
| git add 文件 | 把改动加入暂存区 |
| git commit -m "说明" | 提交暂存区的内容 |
| git push | 推送到远程（GitHub） |
| git pull | 从远程拉取最新代码 |
| git log --oneline | 查看提交历史 |
| git diff | 查看具体改了什么 |
| git remote -v | 查看远程仓库地址 |
| git branch | 查看 / 创建分支 |
| git checkout 分支 | 切换分支 |
| git merge 分支 | 合并分支 |

---

## 三、WSL 使用代理（走 Windows 的 Clash）

### 原理
Windows 上开着 Clash（127.0.0.1:7897），但 WSL 是独立虚拟机，
访问不到 Windows 的 127.0.0.1。需要两步：
1. Clash 开启「允许局域网连接」(Allow LAN)
2. WSL 里设置代理指向 Windows 的 IP

### 已帮你配好的部分
~/.profile 里已加了自动配置：动态获取 Windows 的 IP，并把
http_proxy / https_proxy 指向 7897 端口。

### 你需要做的：开启 Clash 的 Allow LAN
1. 打开 Clash 客户端（Verge / for Windows 等）
2. 找到「允许局域网连接」/「Allow LAN」开关，打开

### 验证代理是否生效
```bash
curl -I https://www.google.com
```
返回 200 就说明代理通了，codex 就能正常连 OpenAI 了。

---

## 四、项目操作流程回顾

### 首次创建并上传项目
```bash
mkdir 项目名 && cd 项目名     # 建目录并进入
git init                       # 初始化 git
# ... 写代码 ...
# GitHub 网页上新建同名空仓库（不勾 README）
git remote add origin git@github.com:用户名/仓库名.git
git branch -M main
git add .
git commit -m "first commit"
git push -u origin main
```

### 日常改代码后
```bash
cd ~/项目名
git add .
git commit -m "改了什么"
git push
```
