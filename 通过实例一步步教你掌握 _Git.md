当然可以！以下是一个**通过实例一步步教你掌握 Git** 的完整教程，涵盖了工作中最常用的操作：

---

## 🚀 目录结构（你可以跟着练习）

```bash
~/projects
└── my_project/
    ├── main.py
    └── README.md
```

---

## 🧱 Step 1：初始化一个 Git 仓库

```bash
cd ~/projects/my_project
git init
```

🔎 作用：初始化 Git 仓库，会创建 `.git` 文件夹。

---

## 📥 Step 2：添加文件到 Git

```bash
git add main.py
```

或者一次性添加全部：

```bash
git add .
```

---

## ✅ Step 3：提交代码

```bash
git commit -m "add main.py"
```

🔎 作用：把 `add` 的内容记录为一个“快照”。

---

## 🧾 Step 4：查看当前状态和历史

```bash
git status     # 查看当前状态
git log        # 查看历史提交记录
```

---

## 🛠 Step 5：修改文件并提交变更

修改 `main.py` 后：

```bash
git add main.py
git commit -m "update logic in main.py"
```

---

## 🔙 Step 6：回滚更改

1. **撤销尚未提交的修改**：

```bash
git checkout -- main.py
```

2. **撤销上一次提交（但保留修改）**：

```bash
git reset --soft HEAD~1
```

3. **撤销上一次提交（也撤销修改）**：

```bash
git reset --hard HEAD~1
```

---

## 🌳 Step 7：分支管理（多人协作必学）

```bash
git branch feature/login   # 创建新分支
git checkout feature/login # 切换到该分支
# or 两步合一：
git switch -c feature/login
```

提交后合并回主分支：

```bash
git checkout main
git merge feature/login
```

---

## 🌐 Step 8：连接远程仓库（如 GitHub）

```bash
git remote add origin https://github.com/yourname/my_project.git
git push -u origin main
```

以后只需：

```bash
git push
```

---

## 📥 Step 9：拉取远程代码

```bash
git pull origin main
```

---

## 🪄 Step 10：查看差异、解决冲突

```bash
git diff            # 查看改动
git diff --staged   # 已添加暂存区的改动
```

如果 `pull` 时有冲突：

* Git 会提示冲突文件，手动修改后：

```bash
git add conflict_file.py
git commit -m "resolve merge conflict"
```

---

## 📦 Step 11：打标签（发布版本）

```bash
git tag v1.0
git push origin v1.0
```

---

## 📁 Step 12：忽略文件（`.gitignore`）

编辑 `.gitignore`：

```
__pycache__/
*.log
.env
```

然后：

```bash
git add .gitignore
git commit -m "add gitignore file"
```

---

## 🧼 Step 13：清理无用文件

```bash
git clean -fd    # 删除工作目录中未跟踪的文件和文件夹
```

---

## 🧠 小结：最常用的 Git 命令清单

| 场景   | 命令                                          |
| ---- | ------------------------------------------- |
| 初始化  | `git init`                                  |
| 添加文件 | `git add .`                                 |
| 提交更改 | `git commit -m "message"`                   |
| 查看状态 | `git status`                                |
| 查看历史 | `git log`                                   |
| 分支操作 | `git branch` / `git checkout` / `git merge` |
| 远程操作 | `git remote` / `git push` / `git pull`      |
| 忽略文件 | `.gitignore` 文件                             |
| 回滚操作 | `git reset` / `git checkout`                |
| 差异比较 | `git diff`                                  |

---

是否需要我帮你做一个「Markdown 小抄」版，方便你复制粘贴？或者你想继续学一下 GitHub 上的 Pull Request、Fork、Rebase、Submodule 等高级内容？
