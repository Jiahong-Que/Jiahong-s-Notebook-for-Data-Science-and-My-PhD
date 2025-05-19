好的！下面是从零开始为你的 Git 添加 **SSH 认证** 的**完整详细教程**，适用于 GitHub 或 GitLab，可以解决你在多台电脑或 GitHub Desktop 中频繁输入账号密码的问题。

---

## 🛡️ 一、什么是 SSH 认证？

SSH 是一种更安全的 Git 认证方式，避免使用用户名和密码。你需要：

1. **在你的电脑上生成 SSH key（公钥 + 私钥）**
2. **将公钥添加到 GitHub / GitLab 账户中**
3. **将仓库的远程地址改为 SSH 地址**

---

## 🧭 二、完整设置流程（以 GitHub 为例）

---

### ✅ 第一步：检查是否已有 SSH key

在终端中执行：

```bash
ls -al ~/.ssh
```

如果你看到如下文件说明已存在（可跳过生成步骤）：

```
id_ed25519     ← 私钥
id_ed25519.pub ← 公钥
```

---

### ✅ 第二步：生成新的 SSH key（如果没有）

在终端中输入（把邮箱改成你自己的）：

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

它会提示你：

```
Enter file in which to save the key (/Users/你的用户名/.ssh/id_ed25519): [直接回车]
Enter passphrase (empty for no passphrase): [可选密码]
```

完成后你会得到：

* **公钥**：`~/.ssh/id_ed25519.pub`（可以分享）
* **私钥**：`~/.ssh/id_ed25519`（千万不能泄露）

---

### ✅ 第三步：将公钥添加到 GitHub

1. 打开 GitHub → [https://github.com/settings/keys](https://github.com/settings/keys)
2. 点击 **New SSH key**
3. Title：比如 `MacBook Air` 或 `Office PC`
4. 在终端中复制公钥：

```bash
cat ~/.ssh/id_ed25519.pub | pbcopy  # macOS
```

5. 粘贴进 GitHub → 保存

---

### ✅ 第四步：测试连接是否成功

```bash
ssh -T git@github.com
```

如果成功，会显示：

```
Hi your_username! You've successfully authenticated.
```

---

### ✅ 第五步：设置 Git 使用 SSH 地址

#### 1. 查看当前仓库使用的地址：

```bash
git remote -v
```

可能是 HTTPS：

```
origin  https://github.com/yourname/repo.git
```

#### 2. 修改为 SSH 地址：

```bash
git remote set-url origin git@github.com:yourname/repo.git
```

你可以从 GitHub 仓库页面 → `Code` 按钮 → 选择 `SSH` → 复制地址。

---

## 🧠 三、在 GitHub Desktop 中使用 SSH

GitHub Desktop 默认支持 SSH，只要你配置好上述步骤，它会自动使用。

但你要确保：

1. 仓库地址是 SSH 格式（见上文）
2. SSH key 存在于 `~/.ssh`，并且终端中能 `ssh -T` 成功连接

---

## 🔁 四、（可选）全局配置 Git 使用 SSH

有些工具可能默认使用 HTTPS，你可以全局指定：

```bash
git config --global url."git@github.com:".insteadOf "https://github.com/"
```

这样即使你粘贴 HTTPS 地址，也会自动用 SSH。

---

## 🧯 五、常见问题排查

| 问题                              | 原因或提示                 | 解决办法                          |
| ------------------------------- | --------------------- | ----------------------------- |
| `Permission denied (publickey)` | 没有添加公钥或权限不对           | 确保公钥添加到了 GitHub               |
| `Host key verification failed`  | 首次连接，未信任主机            | 尝试 `ssh -T git@github.com` 信任 |
| GitHub Desktop 仍然要求密码           | 仓库地址仍是 HTTPS          | `git remote set-url` 改为 SSH   |
| SSH key 未自动加载                   | macOS 可加入 `ssh-agent` | 见下方补充设置                       |

---

## 🧠 补充：macOS 设置 SSH 自动加载（推荐）

1. 编辑 SSH 配置文件：

```bash
nano ~/.ssh/config
```

2. 添加以下内容：

```bash
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes
  UseKeychain yes
```

3. 启用 agent 并添加 key：

```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

---

## ✅ 示例：一条龙命令（适合熟练用户）

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
pbcopy < ~/.ssh/id_ed25519.pub
```

然后直接粘贴到 GitHub 上即可。

---

如果你需要我帮你检查某一台电脑上的 SSH 配置、或你已经生成了 key 但连接失败，我可以一步步帮你调试。是否需要我生成一份你专用的 SSH 配置脚本？
