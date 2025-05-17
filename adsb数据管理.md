**清晰、可扩展、适合长期管理的目录结构和命名规范**，包括：

1. **整体目录结构设计（建议放在 Synology NAS 上）**
2. **原始数据、处理数据、数据集、模型、日志分类保存**
3. **一致的文件命名规范：易查找、易排序、可追溯**

---

## ✅ 顶层目录结构（建议挂载路径：`/volume1/adsb_project/`）

```
/adsb_project/
├── raw_data/             # 原始ADS-B数据（Parquet）
├── processed/            # 中间处理结果（如taxi-in events）
├── datasets/             # 特征工程完成后的建模数据集
├── metadata/             # 每个数据集的说明文件（JSON/CSV）
├── models/               # 保存训练好的模型
├── scripts/              # 下载、清洗、训练脚本
├── notebooks/            # 数据探索和建模的Notebook
├── dvc_storage/          # DVC remote data 目录
└── mlruns/               # MLflow 日志与模型追踪
```

---

## 📁 1. `raw_data/` 目录结构与命名

```
raw_data/
├── FRA/
│   ├── 2023-01-01.parquet
│   ├── 2023-01-02.parquet
│   └── ...
├── CDG/
├── AMS/
└── ...
```

### ✅ 文件命名格式：

```
<airport_code>_<YYYY-MM-DD>.parquet
# 示例：FRA_2023-01-01.parquet
```

### 🔁 可选按月分子文件夹（适合批处理）

```
raw_data/FRA/2023-01/2023-01-01.parquet
```

---

## ⚙️ 2. `processed/` 保存 taxi-in events（事件级数据）

```
processed/
├── FRA/
│   ├── taxiin_events_2023-01.parquet
│   ├── taxiin_events_2023-02.parquet
│   └── ...
├── CDG/
└── ...
```

### ✅ 命名格式：

```
taxiin_events_<YYYY-MM>.parquet
```

可根据需要扩展为：

```
taxiin_events_<airport_code>_<YYYY-MM>.parquet
```

---

## 📊 3. `datasets/` 保存建模使用的最终特征数据

```
datasets/
├── FRA/
│   ├── taxiin_dataset_2023-Q1.csv
│   ├── taxiin_dataset_2023-Q2.csv
│   └── ...
├── CDG/
└── ...
```

### ✅ 命名格式：

```
taxiin_dataset_<airport_code>_<YYYY>-<Qx>.csv
```

或按月：

```
taxiin_dataset_<airport_code>_<YYYY-MM>.csv
```

---

## 📎 4. `metadata/` 保存数据集说明（版本、字段、时间范围）

```
metadata/
├── FRA_2023-Q1.json
├── CDG_2023-Q2.json
└── ...
```

内容建议包括：

```json
{
  "airport": "FRA",
  "time_range": "2023-01-01 to 2023-03-31",
  "features": ["taxi_distance_m", "concurrent_aircraft", "weather"],
  "label": "duration_min",
  "source": "OpenSky API + Visual Crossing Weather",
  "created_by": "xiaohong",
  "created_at": "2025-05-17"
}
```

---

## 📦 5. `models/` 保存模型

```
models/
├── FRA_MLP_Q1.joblib
├── FRA_RF_Q1.joblib
└── ...
```

### 命名格式：

```
<airport>_<model_type>_<time_range>.joblib
```

---

## ✅ 最终总结表格（命名规范）

| 文件类型      | 示例文件名                            | 存储路径             |
| --------- | -------------------------------- | ---------------- |
| 原始数据      | `FRA_2023-01-01.parquet`         | `raw_data/FRA/`  |
| Taxi-in事件 | `taxiin_events_2023-01.parquet`  | `processed/FRA/` |
| 特征数据集     | `taxiin_dataset_FRA_2023-Q1.csv` | `datasets/FRA/`  |
| 数据说明      | `FRA_2023-Q1.json`               | `metadata/`      |
| 模型        | `FRA_RF_Q1.joblib`               | `models/`        |

---

✅ 如果你想，我可以生成一个 Bash 脚本来**自动创建所有目录**，并生成一个 sample dataset 的元数据 JSON 文件作为模板。是否需要我现在生成？
