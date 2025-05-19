好的！你现在已经有了一个本地 Git 仓库（如 `SmartPouchML`），接下来我将**一步一步详细指导你使用 [DVC（Data Version Control）](https://dvc.org/)**，用于管理你的数据、模型文件和实验记录 —— 特别适用于你正在做的 ML 项目。

---

## 🧭 目标

用 DVC 管理如下内容：

* 原始数据（如 CSV、Parquet）
* 处理后的数据
* 训练模型文件（如 `.pkl`, `.h5`）
* 实验参数和结果
* 可选：远程数据存储（如 Synology NAS）

---

## 🛠️ 前提准备

请确保你的本地环境有：

```bash
# 必须有 Python 和 pip
python3 --version
pip install dvc
```

---

## 🚀 第一步：初始化 DVC 项目

在你项目根目录中（例如 `SmartPouchML/`）执行：

```bash
dvc init
```

🔍 它会创建以下文件：

| 文件/目录         | 说明                   |
| ------------- | -------------------- |
| `.dvc/`       | DVC 配置目录             |
| `.dvc/config` | DVC 的本地配置            |
| `.dvcignore`  | 类似 `.gitignore`，忽略文件 |
| `dvc.lock`    | 记录 DVC 文件依赖和输出的锁定文件  |
| `.gitignore`  | 会自动添加 DVC 跟踪的文件      |

然后执行：

```bash
git add .dvc .dvcignore .gitignore
git commit -m "Initialize DVC"
```

---

## 📂 第二步：添加一个数据文件（如 sensor 数据）

假设你有一个数据文件 `data/raw/sensor.csv`，你希望使用 DVC 管理它。

```bash
dvc add data/raw/sensor.csv
```

🔍 会生成：

* `data/raw/sensor.csv.dvc`（这个文件记录了 data 的元信息）
* `.gitignore` 会自动忽略真正的数据文件

然后：

```bash
git add data/raw/sensor.csv.dvc
git commit -m "Track raw sensor data with DVC"
```

🔁 **注意：真正的数据（如 sensor.csv）不会上传到 Git，只会上传 .dvc 文件！**

---

## ☁️ 第三步：配置远程存储（建议使用 Synology NAS）

你可以把数据放在家用 NAS 或云端，方便在不同电脑同步。

```bash
# 示例：使用 NAS 挂载为本地路径
dvc remote add -d mynas /Volumes/MyNAS/dvc-storage
dvc remote modify mynas type local
```

然后推送数据：

```bash
dvc push
```

这一步会把本地的 `data/raw/sensor.csv` 上传到远程存储。

---

## 🔁 第四步：另一个电脑使用项目

在另一台电脑上：

```bash
git clone git@github.com:.../SmartPouchML.git
cd SmartPouchML
pip install dvc
dvc pull   # 下载 .dvc 中记录的数据文件
```

---

## 🧪 第五步：添加模型文件、输出文件等

假设你训练了一个模型 `models/model.pkl`：

```bash
dvc add models/model.pkl
git add models/model.pkl.dvc
git commit -m "Add trained model"
dvc push   # 上传模型文件
```

---

## 📜 第六步：使用 `dvc.yaml` 管理完整流水线（高级用法）

如果你有完整的数据处理 → 模型训练 → 评估流程，可以写：

```bash
dvc stage add -n preprocess \
  -d src/preprocess.py -d data/raw/sensor.csv \
  -o data/processed/cleaned.csv \
  python src/preprocess.py

dvc stage add -n train \
  -d src/train.py -d data/processed/cleaned.csv \
  -o models/model.pkl \
  python src/train.py
```

然后：

```bash
dvc repro      # 自动执行所有流程（仅当依赖有变化）
dvc push       # 上传输出文件
git add dvc.yaml dvc.lock
git commit -m "Add full ML pipeline"
```

---

## ✅ DVC 基本命令速查表

| 命令               | 作用              |
| ---------------- | --------------- |
| `dvc init`       | 初始化项目           |
| `dvc add <file>` | 添加单个文件/目录进行跟踪   |
| `dvc push`       | 上传数据到远程         |
| `dvc pull`       | 下载数据到本地         |
| `dvc status`     | 查看哪些文件改变        |
| `dvc repro`      | 重新运行流水线（根据依赖变动） |
| `dvc remote add` | 添加远程存储          |
| `dvc gc`         | 清理不再使用的数据       |

---

## 📦 文件结构建议（你的项目）

```text
SmartPouchML/
├── data/
│   ├── raw/
│   │   └── sensor.csv
│   └── processed/
│       └── cleaned.csv
├── models/
│   └── model.pkl
├── src/
│   ├── preprocess.py
│   └── train.py
├── .gitignore
├── dvc.yaml
├── dvc.lock
└── ...
```

---

## ✅ 接下来你要做的：

1. 回复我你目前的项目结构或数据文件路径
2. 我将手把手指导你完成：

   * 添加你的 sensor data
   * 设置远程存储（NAS）
   * 配置训练流程（如果已有脚本）
   * 提交到 Git + DVC

是否现在开始添加第一个数据文件？你可以发我路径，比如 `data/raw/data1.csv`，我来指导你下一步操作。
