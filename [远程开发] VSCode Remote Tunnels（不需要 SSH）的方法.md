# VSCode Remote Tunnels（不需要 SSH）的方法

它是微软提供的一种简洁、安全的远程开发方式，适合你这种在**Windows 本地运行环境 + 在 macOS 上远程开发**的场景，不需要配置 SSH 服务器或开放端口。

---

## 🎯 什么是 VSCode Remote Tunnels？

**VSCode Remote Tunnels** 允许你在一台电脑（如 Windows 桌面）上运行一个后台 VSCode Server，然后你可以从另一台电脑（如 macOS）通过 VSCode GUI **远程连接和开发**，**不需要配置 SSH**，也不需要公网 IP。

它通过微软的账户进行身份验证，并通过云中转连接你的两台设备。

---

## 🧰 一次性配置：在 Windows 电脑上启动 Remote Tunnel

### ✅ Step 1: 安装 VSCode 和登录账户

1. 安装 VSCode：
   [https://code.visualstudio.com/](https://code.visualstudio.com/)

2. 打开 VSCode，**登录 Microsoft 账户**（点击左下角头像登录）

### ✅ Step 2: 安装 Remote Tunnel 插件（如果没安装）

在 VSCode 中安装插件：

* 搜索并安装：`Remote - Tunnels`

或在终端运行：

```bash
code --install-extension ms-vscode.remote-server
```

---

### ✅ Step 3: 启动 Remote Tunnel（后台运行 VSCode Server）

打开 Windows 的 **终端（Command Prompt、PowerShell、Windows Terminal 均可）**：

运行：

```bash
code tunnel --accept-server-license-terms
```

此命令会：

* 启动一个 VSCode 后台 Server
* 自动连接你的 Microsoft 账户
* 将当前电脑注册为 Tunnel 服务
* 显示如下内容：

  ```
  ✅ Tunnel connection established
  Connected as: your.email@example.com
  Device name: MY-PC
  ```

---

## 📲 在 Mac 上连接 Remote Tunnel

### ✅ Step 1: 安装 VSCode 并登录同一个账户

1. macOS 上安装 VSCode
2. 登录与你 Windows 上相同的 Microsoft 账户

---

### ✅ Step 2: 打开 Remote Tunnel 连接

1. 打开 **左侧菜单栏中的 Remote Explorer**
2. 点击 **"Tunnels"**
3. 会看到你 Windows 电脑的设备名（如：`MY-PC`）
4. 点击设备 → 自动连接 → 进入远程开发环境！

你就像在本地一样使用 Windows 上的所有文件、Conda 环境、终端、Jupyter、Python 脚本等。

---

## ✅ 支持的能力

* 浏览远程 Windows 文件系统
* 打开终端（使用远程 Conda 环境）
* 安装/使用 VSCode 插件（自动部署到远程）
* 使用远程 Git、Jupyter 等工具
* 快速测试模型训练、分析脚本等
* 自动同步 Conda 环境解释器（可选手动选择）

---

## 🔐 安全性说明

* Tunnel 使用微软账户 + 加密连接 + 云代理中转
* 不需要公网地址 / 端口开放
* 所有通信默认加密
* 支持组织账户身份验证

---

## 🔁 每次使用时如何启动？

你只需在 Windows 上打开终端并运行：

```bash
code tunnel
```

或使用以下方式让它“后台自动运行”：

### ✅ 方法 1：Windows 启动项中添加：

在 `C:\Users\<YourName>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup` 中创建 `.bat` 文件：

```bat
@echo off
code tunnel --accept-server-license-terms
```

### ✅ 方法 2：注册为 Windows 后台服务（可选）

你也可以用 Node 或 NSSM 设置成 Windows 后台服务，但不建议太复杂，手动运行或开机启动更轻量。

---

## 💡 总结对比：Tunnel vs SSH

| 项目      | Tunnel 模式     | SSH 模式          |
| ------- | ------------- | --------------- |
| 需要公网 IP | ❌ 否           | ✅ 是（或 VPN）      |
| 设置难度    | ⭐ 极低          | ⚠️ 中等           |
| 安全性     | ✅ 高（微软账户认证）   | ✅ 高（取决于 SSH 配置） |
| 启动方式    | `code tunnel` | SSH 登录 VSCode   |
| 适合谁     | 新手、轻量开发、跨平台   | 专业、控制更多、服务器环境   |

---

## 🚀 实战建议

* **适合你这种情况**：在家里 Windows 跑 Conda 环境，外面用 Mac 远程开发
* 推荐结合 Conda + VSCode Tunnel 打造“个人 AI 开发服务器”


