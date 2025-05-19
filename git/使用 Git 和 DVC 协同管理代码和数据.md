非常好！你使用 Git 和 DVC 协同管理代码和数据，这是现代机器学习项目开发的最佳实践之一。

下面我将 **总结你日常使用 Git + DVC 的常见情况**，并为每种情况提供**具体的使用方法和命令**，让你无论在哪台电脑、哪个阶段工作都能得心应手。

---

## 🧠 核心原则

* **Git 管代码和结构（小文件）**
* **DVC 管数据和模型（大文件）**
* 二者一起用，实现代码 + 数据的版本一致性

---

## 🧾 一、常见情况 + 使用方法

---

### ✅ 1. 项目初始化（仅做一次）

| 场景            | 操作 |
| ------------- | -- |
| 初始化 Git 和 DVC |    |

```bash
git init
dvc init
git add .dvc .gitignore .dvcignore
git commit -m "Initialize Git and DVC"
```

---

### ✅ 2. 添加数据文件进行版本控制（DVC）

| 场景       | 操作 |
| -------- | -- |
| 添加单个数据文件 |    |

```bash
dvc add data/raw/data.csv
git add data/raw/data.csv.dvc
git commit -m "Add raw data"
```

\| 添加整个目录 |

```bash
dvc add data/raw/
git add data/raw.dvc
git commit -m "Add raw data folder"
```

---

### ✅ 3. 添加训练好的模型文件

| 场景                             | 操作 |
| ------------------------------ | -- |
| 添加模型文件（如 `.pkl`, `.pt`, `.h5`） |    |

```bash
dvc add models/model.pkl
git add models/model.pkl.dvc
git commit -m "Add trained model"
```

---

### ✅ 4. 推送数据到远程（NAS、云盘等）

| 场景                | 操作 |
| ----------------- | -- |
| 推送所有 `.dvc` 数据到远程 |    |

```bash
dvc push
```

\| 查看状态（是否已 push） |

```bash
dvc status -c
```

---

### ✅ 5. 切换电脑后同步代码和数据

| 场景                  | 操作 |
| ------------------- | -- |
| 在另一台电脑上 clone 并拉取数据 |    |

```bash
git clone git@github.com:xxx/your-repo.git
cd your-repo
pip install dvc
dvc pull   # 获取数据
```

---

### ✅ 6. 修改原始数据后重新追踪

| 场景     | 操作 |
| ------ | -- |
| 更新数据文件 |    |

```bash
# 修改了 data/raw/data.csv 后
dvc add data/raw/data.csv
git add data/raw/data.csv.dvc
git commit -m "Update raw data"
dvc push
```

---

### ✅ 7. 使用 Git 提交代码更新

| 场景   | 操作 |
| ---- | -- |
| 常规提交 |    |

```bash
git add src/train.py
git commit -m "Update training script"
git push
```

---

### ✅ 8. 恢复历史版本的代码 + 数据

| 场景          | 操作 |
| ----------- | -- |
| 回滚某个 commit |    |

```bash
git checkout <commit-hash>
dvc checkout   # 同步数据到对应版本
```

---

### ✅ 9. 管理完整实验流程（使用 dvc.yaml）

| 场景             | 操作 |
| -------------- | -- |
| 添加 pipeline 步骤 |    |

```bash
dvc stage add -n preprocess \
  -d data/raw/data.csv -o data/processed/data_clean.csv \
  python scripts/preprocess.py
```

\| 运行所有步骤 |

```bash
dvc repro
```

\| 查看 pipeline |

```bash
dvc dag
```

---

### ✅ 10. 清理旧数据（防止 NAS 爆满）

| 场景            | 操作 |
| ------------- | -- |
| 清理所有未用数据（需小心） |    |

```bash
dvc gc -c
```

---

## 🧰 常用命令速查表

| 命令                                | 说明              |
| --------------------------------- | --------------- |
| `dvc init`                        | 初始化 DVC 项目      |
| `dvc add <file>`                  | 添加单个数据或模型       |
| `dvc push`                        | 上传数据到远程         |
| `dvc pull`                        | 下载数据到本地         |
| `dvc status -c`                   | 查看数据是否已同步       |
| `dvc repro`                       | 自动执行依赖的任务链      |
| `dvc checkout`                    | 同步 Git 和 DVC 状态 |
| `dvc remote add -d <name> <path>` | 添加远程存储          |
| `dvc gc -c`                       | 清理远程未使用的数据      |

---

## 📝 推荐项目结构（ML 项目最佳实践）

```text
SmartPouchML/
├── data/
│   ├── raw/
│   └── processed/
├── models/
├── src/
│   ├── preprocess.py
│   └── train.py
├── dvc.yaml
├── dvc.lock
├── .gitignore
├── .dvc/
└── ...
```

---

如果你想，我可以为你生成一个 **Git + DVC 项目模版 repo**，包括：

* `.gitignore`
* `dvc.yaml`
* 示例 `add/push/pull`
* 文档说明

是否需要我生成这个结构 + 模板脚本给你？你也可以告诉我你平时的使用习惯，我来帮你定制。
