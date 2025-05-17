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

太好了，你现在要**系统性下载欧洲主要枢纽机场的ADS-B数据并保存到Synology NAS**。下面是一个完整的、可落地的**全流程设计方案**，分为五大阶段：

---

## ✅ 总览：五步完成下载+保存+管理

| 阶段             | 内容                         | 工具                     | 是否自动化 |
| -------------- | -------------------------- | ---------------------- | ----- |
| ① 机场与时间规划      | 明确机场和时间范围                  | Excel 或脚本配置            | ✅     |
| ② 数据下载         | 调用 OpenSky API 批量下载        | Python + OpenSky API   | ✅     |
| ③ 存储与压缩        | 保存为 Parquet 文件至 NAS        | Polars/Pandas + NAS 挂载 | ✅     |
| ④ 数据结构与版本管理    | 用 DVC 跟踪和组织数据              | DVC + Git              | ✅     |
| ⑤ 数据后处理与入库（可选） | 提取 taxi-in 事件、入 PostgreSQL | 脚本 + PostGIS           | ✅（部分） |

---

## 🛫 步骤 ①：机场 + 时间规划

### 推荐机场列表（可自行扩展）

```python
AIRPORTS = {
    "FRA": {"icao": "EDDF", "name": "Frankfurt"},
    "CDG": {"icao": "LFPG", "name": "Paris CDG"},
    "AMS": {"icao": "EHAM", "name": "Amsterdam"},
    "LHR": {"icao": "EGLL", "name": "London Heathrow"},
    "LEJ": {"icao": "EDDP", "name": "Leipzig"},
    "CGN": {"icao": "EDDK", "name": "Cologne"},
    "LUX": {"icao": "ELLX", "name": "Luxembourg"},
}
```

### 时间范围定义

你可以定义要下载的时间范围（例如 `2023-01-01` 至 `2023-12-31`），按天或按小时抓取。

---

## ⏬ 步骤 ②：数据抓取脚本（Python 示例）

### 📦 OpenSky Historical API 下载器（简化示例）

```python
import requests, time, os
from datetime import datetime, timedelta
import polars as pl

USERNAME = "your_opensky_username"
PASSWORD = "your_password"
NAS_DIR = "/volume1/ADS-B_Project/raw_data"

def download_adsb(airport_icao, date, save_dir):
    start = int(datetime.strptime(date, "%Y-%m-%d").timestamp())
    end = int((datetime.strptime(date, "%Y-%m-%d") + timedelta(days=1)).timestamp())

    url = f"https://opensky-network.org/api/flights/arrival?airport={airport_icao}&begin={start}&end={end}"
    response = requests.get(url, auth=(USERNAME, PASSWORD))
    if response.status_code == 200:
        data = response.json()
        if data:
            df = pl.DataFrame(data)
            fname = f"{airport_icao}_{date}.parquet"
            df.write_parquet(os.path.join(save_dir, fname))
            print(f"✅ Saved {fname}")
        else:
            print(f"⚠️ No data for {airport_icao} on {date}")
    else:
        print(f"❌ Error {response.status_code}: {response.text}")
```

你可以循环调用这个函数下载多个机场多天数据。

---

## 🗂️ 步骤 ③：保存结构设计（NAS上）

```bash
/ADS-B_Project/
├── raw_data/
│   ├── FRA/
│   │   ├── 2023-01-01.parquet
│   │   ├── ...
│   ├── CDG/
│   └── ...
```

确保你用 Docker 容器挂载了 `/volume1/ADS-B_Project`，并写入路径。

---

## 📦 步骤 ④：用 DVC 管理数据

1. 初始化：

   ```bash
   git init
   dvc init
   dvc remote add -d nas /volume1/ADS-B_Project/dvc_storage
   ```

2. 添加数据集（每天/每月）

   ```bash
   dvc add raw_data/FRA/2023-01-01.parquet
   git add raw_data/FRA/2023-01-01.parquet.dvc
   git commit -m "Add FRA Jan 1 2023 ADS-B"
   dvc push
   ```

---

## 🧮 步骤 ⑤：后处理（可选）

你可以创建 pipeline 或 notebook，自动：

* 提取 taxi-in event
* 加入天气信息
* 构造建模用数据集
* 将清洗后的结果保存到：

```bash
/ADS-B_Project/processed/FRA_2023_Q1_events.parquet
/ADS-B_Project/datasets/FRA_2023_Q1_dataset.csv
```

并继续用 DVC 管理。

---

## ✅ 你现在可以自动化的工具组合

| 模块    | 工具                       |
| ----- | ------------------------ |
| 抓取调度  | Python + Cron 或 Airflow  |
| 存储压缩  | Parquet + Polars         |
| 数据版本  | DVC                      |
| 远程存储  | Synology NAS             |
| 数据库分析 | PostgreSQL + PostGIS（可选） |

---

## 🎁 BONUS：我可以为你生成

1. 全自动下载多机场 ADS-B 数据脚本（支持每天/每月）
2. 保存为 Parquet 并自动 DVC 跟踪
3. 项目 `Makefile` 或 `.sh` 脚本自动执行抓取、存储、版本管理


