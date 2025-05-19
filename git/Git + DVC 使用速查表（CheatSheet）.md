以下是一个完整的 `Git + DVC` 使用速查表（CheatSheet），适合你管理和协作开发数据科学或机器学习项目：

---

# 🧠 Git + DVC CheatSheet（速查表）

## 🔧 项目初始化

```bash
git init                       # 初始化 Git 仓库
dvc init                      # 初始化 DVC 项目（会创建 .dvc/config 和 .dvc/.gitignore）
git add .dvc .dvcignore
git commit -m "Initialize DVC"
```

---

## 📁 添加数据文件/目录到 DVC 管理

```bash
dvc add data/raw_data.csv            # 添加单个数据文件
dvc add data/                        # 添加整个数据目录

git add data/raw_data.csv.dvc .gitignore
git commit -m "Add raw data with DVC"
```

---

## 🌐 配置远程存储（例如 NAS 或云端）

```bash
dvc remote add -d myremote <url>     # 添加远程存储，-d 表示设置为默认
dvc remote modify myremote user <username>   # 如果需要认证
dvc remote modify myremote password <password>
```

例如：

```bash
dvc remote add -d nas_storage sftp://192.168.1.100:/volume1/dvc-storage
```

---

## ⬆️ 上传数据到远程存储

```bash
dvc push                      # 上传 .dvc 管理的数据到远程
```

---

## ⬇️ 从远程拉取数据（在 clone 项目后）

```bash
git clone <repo-url>
dvc pull                      # 下载 DVC 管理的数据
```

---

## 🔁 版本管理（Git + DVC 同步）

```bash
git add file.dvc              # 添加 .dvc 文件或 .dvc.yaml
git commit -m "Track new version of data/model"
dvc push                      # 推送新的数据版本
git push                      # 推送代码仓库
```

---

## 📋 运行实验与记录流程（使用 DVC pipelines）

```bash
dvc run -n train_model \
        -d data/processed \
        -d src/train.py \
        -o model.pkl \
        python src/train.py

git add dvc.yaml dvc.lock
git commit -m "Add training pipeline"
```

---

## ▶️ 重新运行步骤（自动追踪依赖变化）

```bash
dvc repro                    # 根据 dvc.yaml 自动重跑受影响的步骤
```

---

## 🧪 管理实验（实验分支和可重复性）

```bash
dvc exp run                  # 运行实验（自动保存快照）
dvc exp show                 # 显示实验结果
dvc exp apply <exp-name>     # 应用实验结果到当前工作区
```

---

## 🕰️ 恢复历史数据或模型版本

```bash
git checkout <commit-id>                 # 回退代码到历史版本
dvc checkout                             # 恢复对应版本的数据
```

---

## 🧹 清理无用缓存和文件

```bash
dvc gc -w                      # 清理当前工作区无用的数据缓存
```

---

## 🔍 查看状态和差异

```bash
dvc status                     # 查看当前工作区与缓存/远程的差异
dvc diff                       # 显示两个版本之间的数据差异（通过 Git 提交或 DVC 版本）
```

---

## 🚀 合作开发建议

1. 每次变更数据后运行：

   ```bash
   dvc add ... -> git add -> git commit -> dvc push
   ```
2. 拉取他人改动后运行：

   ```bash
   git pull -> dvc pull
   ```
3. 使用 `.dvc`, `dvc.yaml`, `dvc.lock`，**不要直接把 data/ 文件夹加入 Git**

---

## ✅ 常见文件说明

| 文件/文件夹          | 说明                 |
| --------------- | ------------------ |
| `.dvc/`         | DVC 配置和缓存信息        |
| `data/file.dvc` | 跟踪文件的元数据           |
| `dvc.yaml`      | DVC 流程文件（pipeline） |
| `dvc.lock`      | pipeline 运行依赖锁定信息  |
| `.gitignore`    | 自动忽略被 DVC 跟踪的数据文件  |

---

如果你有具体的项目目录结构，我可以再帮你生成定制化的 workflow 图或模板。是否需要？
