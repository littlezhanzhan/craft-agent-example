# SSH Skill for Craft Agent

这是一个适合 **Craft Agent / Claude Code 项目级 skill** 的共享版 SSH skill。

它基于上游项目 [badseal/ssh-skill](https://github.com/badseal/ssh-skill) 做了适配，目标是：

- 适合放进 Git 仓库复用
- 不绑定某一台 Windows 机器的绝对路径
- 对 **macOS** 和 **Windows** 更友好
- 默认文档更偏向 **key / ssh-agent** 的安全用法

## 目录位置

项目级 skill 推荐放在：

- `.agents/skills/ssh-skill/`

Craft Agent 在项目目录中可以直接发现这个 skill。

## 包含内容

- `SKILL.md`：skill 指令
- `icon.svg`：图标
- `scripts/`：SSH 执行、上传下载、隧道、批量执行、配置读取等脚本

## 依赖

### 必需
- Python 3.8+
- `paramiko`
- 系统 OpenSSH 客户端（`ssh`）
- 本机 `~/.ssh/config` 中已有可用 alias

### 安装依赖

```bash
python -m pip install paramiko
```

## 推荐前置条件

- 优先使用 **密钥认证**
- 如果密钥有 passphrase，提前配置 **ssh-agent**
- 先确保本机命令行能通过 alias 正常 SSH 登录

例如：

```bash
ssh my-server
```

## macOS 使用说明

通常不需要额外环境变量，直接使用：

```bash
python ".agents/skills/ssh-skill/scripts/ssh_execute.py" my-server "hostname"
```

上传示例：

```bash
python ".agents/skills/ssh-skill/scripts/ssh_upload.py" my-server "./app.tar.gz" "/tmp/app.tar.gz"
```

## Windows 使用说明

执行命令示例：

```bash
python ".agents/skills/ssh-skill/scripts/ssh_execute.py" my-server "hostname"
```

如果你在 **Git Bash / MSYS** 下做上传、下载、服务器间传输，并且远程路径是 `/tmp/...` 这种 Unix 路径，建议加：

```bash
MSYS_NO_PATHCONV=1
```

例如：

```bash
MSYS_NO_PATHCONV=1 python ".agents/skills/ssh-skill/scripts/ssh_upload.py" my-server "./app.tar.gz" "/tmp/app.tar.gz"
```

## 当前共享版的默认边界

这个共享版默认**不鼓励**：

- 把明文密码写进 `~/.ssh/config`
- 自动改写 SSH config
- 未确认就执行破坏性批量操作

如果你要扩展成你自己的内部版本，可以在此基础上再加规则。

## 来源与改造说明

上游项目：
- [badseal/ssh-skill](https://github.com/badseal/ssh-skill)

本共享版主要做了这些改造：
- 去掉当前机器专属的绝对路径说明
- 增加项目级 skill 使用方式
- 补充 macOS / Windows 安装说明
- 默认文档收敛到更安全的使用建议
