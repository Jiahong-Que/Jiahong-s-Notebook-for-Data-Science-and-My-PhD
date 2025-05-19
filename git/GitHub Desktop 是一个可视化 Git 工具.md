GitHub Desktop 是一个可视化 Git 工具，非常适合不熟悉命令行的用户进行版本控制操作。下面我将从**工作流程、功能点、常见问题及解决方法**几个方面系统介绍 GitHub Desktop 的使用。

---

## 🧭 一、GitHub Desktop 的主要工作流程（完整步骤）

### 1. 克隆远程仓库到本地（Clone）

* 打开 GitHub Desktop → `File > Clone Repository...`
* 选择 GitHub 上的仓库或输入 URL → 选择保存路径 → Clone

### 2. 查看分支和切换分支

* 主界面左上角选择分支（Current Branch）
* 可以：

  * `Create New Branch` 创建新分支（如 `feature-x`）
  * `Publish Branch` 推送该分支到 GitHub

### 3. 拉取最新代码（Pull）

* 点击顶部的 `Fetch origin` → 如果有更新，按钮会变成 `Pull origin`
* 点击 `Pull origin` 同步远程代码

### 4. 修改文件后提交（Commit）

* 修改本地文件 → GitHub Desktop 会自动检测更改
* 在左侧文件变更区选择要提交的文件
* 输入 Commit message → 点击 `Commit to <branch>`

### 5. 推送本地更改到远程仓库（Push）

* Commit 后点击顶部的 `Push origin`

### 6. 合并分支（Merge）

* 切换到目标分支（如 `main`）
* 菜单栏选择 `Branch > Merge into Current Branch`
* 选择你想合并的分支（如 `feature-x`）

### 7. 解决冲突（Conflict Resolution）

* 如果冲突出现，会提示你文件有冲突，点击 `Resolve conflicts`
* 进入内置合并工具，选择保留哪一部分
* 合并后点击 `Mark as resolved` → `Commit merge` → `Push origin`

---

## 🧩 二、常见问题与解决方案

### 🟡 问题 1：**Push 被拒绝（rejected）**

> 提示 `rejected - non-fast-forward`

🔧 原因：

* 远程仓库更新了，你本地没拉取最新版本

✅ 解决方法：

1. 点击 `Fetch origin`
2. 然后点击 `Pull origin`
3. 解决冲突（如果有）
4. 最后 `Push origin`

---

### 🔴 问题 2：**身份验证失败（Authentication Failed）**

> 无法连接 GitHub 或提示 token 失效

🔧 原因：

* 旧的凭据缓存
* 使用了 HTTPS 而非 SSH
* GitHub Desktop 未更新 token

✅ 解决方法：

1. macOS：打开钥匙串访问 → 删除与 GitHub 相关条目
2. GitHub Desktop 中 `File > Options > Accounts` → 退出重新登录
3. 推荐使用 **SSH 认证**：

   * 在 GitHub 网站添加你的 SSH 公钥
   * 将仓库 URL 改为 SSH 形式：
     `git@github.com:username/repo.git`

---

### 🟡 问题 3：**分支切换失败，提示未提交变更**

> Cannot switch branch with uncommitted changes

✅ 解决方法：

* 提交更改或放弃更改

  * 点击 `Discard Changes`
  * 或 `Stash changes`（需要配合命令行）

---

### 🔴 问题 4：**合并冲突（Merge Conflict）**

> 提示 Merge conflict in 某文件

✅ 解决方法：

1. 点击提示的 `Resolve conflicts`
2. 手动选择保留内容（选择“Use ours”、“Use theirs”或手动编辑）
3. 点击 `Mark as resolved` → `Commit merge` → `Push origin`

---

### 🟡 问题 5：**仓库不显示或无法同步**

✅ 排查方法：

* 确保 `.git` 目录存在
* 目录路径没有中文或特殊字符
* 仓库未被删除、权限未改变

---

## 🧰 三、GitHub Desktop 常用菜单功能概览

| 菜单路径                               | 功能说明                        |
| ---------------------------------- | --------------------------- |
| File > Clone Repository            | 克隆远程仓库到本地                   |
| File > Add Local Repo              | 把本地已有项目添加到 GitHub Desktop 中 |
| Repository > Repository settings   | 设置仓库信息                      |
| Branch > New Branch                | 创建新分支                       |
| Branch > Merge into Current Branch | 分支合并                        |
| Branch > Delete                    | 删除分支                        |
| History 标签页                        | 查看提交历史（包含图形分支结构）            |
| Repository > Open in Terminal      | 打开当前仓库目录终端                  |

---

## 💡 四、GitHub Desktop 使用建议

* **每次开始工作前**先 `Fetch origin` + `Pull origin`
* **开发新功能**用新分支，便于隔离和管理
* **遇到问题**不要直接 discard，多使用 commit / stash
* **建议开启自动保存和同步设置**
* **配合命令行**可以解决更复杂的问题（例如 `git stash`）

---

## 🧾 五、附：典型使用场景图（文字简述）

```text
[开始工作]
↓
Fetch origin → Pull origin → 开始编辑
↓
保存 → GitHub Desktop 检测变更
↓
Commit → Push origin
↓
（可选）合并分支 → 解决冲突 → Push
```

---

如果你想，我可以提供：

* 多台电脑协作的 **GitHub Desktop 操作流程图**
* 各类问题的 **错误截图对照表**
* GitHub Desktop + 命令行混合工作流程建议

需要哪个我可以立刻为你生成。
