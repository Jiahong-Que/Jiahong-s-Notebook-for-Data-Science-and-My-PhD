下面是为你量身定制的一套完整数据架构方案，适用于你当前的**多机场ADS-B大数据项目**，从数据存储、数据库结构、处理流程到分析工具，结合**PostgreSQL + Parquet + Python** + Docker 部署，最终支持你的建模、分析与可视化目标。

---

## ✅ 一、整体架构概览

```
        ┌────────────┐
        │ 原始下载器 │────→ 保存为 Parquet (按机场/月份)
        └────────────┘
               ↓
      ┌─────────────────────┐
      │ 数据清洗+事件提取脚本 │ ← 用 Pandas / Dask 实现
      └─────────────────────┘
               ↓
      ┌────────────────────┐
      │ PostgreSQL + TimescaleDB │ ← 结构化存入数据库（Docker部署）
      └────────────────────┘
               ↓
  ┌──────────────┐       ┌───────────────┐
  │ Python 模型训练 │ ←→ │ Streamlit 可视化 │
  └──────────────┘       └───────────────┘
```

---

## ✅ 二、Parquet 存储结构建议（适用于 NAS 或 S3）

目录结构：

```
/adsb_data/
  ├── EDDL/             # 每个机场一个文件夹
  │   ├── 2024_01.parquet
  │   ├── 2024_02.parquet
  ├── EHAM/
  └── EDDF/
```

文件命名统一：`<airport_code>_<YYYY_MM>.parquet`
每个文件结构示例（字段）：

```
icao24, callsign, time, latitude, longitude, baro_altitude, ground_speed, on_ground, airport_code
```

---

## ✅ 三、PostgreSQL + TimescaleDB 表结构设计

**Docker 快速部署命令：**

```bash
docker run -d --name timescaledb \
  -p 5432:5432 \
  -e POSTGRES_PASSWORD=mysecretpassword \
  timescale/timescaledb:latest-pg14
```

**建表示例（Taxi-in Events）：**

```sql
CREATE TABLE taxi_in_events (
    id SERIAL PRIMARY KEY,
    airport_code VARCHAR(10),
    icao24 VARCHAR(10),
    callsign VARCHAR(10),
    timestamp_landing TIMESTAMP,
    timestamp_stop TIMESTAMP,
    duration_sec INT,
    landing_runway VARCHAR(10),
    parking_stand VARCHAR(10),
    final_parking_area VARCHAR(5),
    flight_type VARCHAR(5), -- PAX or CAR
    insert_time TIMESTAMP DEFAULT now()
);

-- 创建 TimescaleDB 的时间分区
SELECT create_hypertable('taxi_in_events', 'timestamp_landing');
```

**加速查询的索引建议：**

```sql
CREATE INDEX ON taxi_in_events (airport_code);
CREATE INDEX ON taxi_in_events (landing_runway);
CREATE INDEX ON taxi_in_events (timestamp_landing DESC);
```

---

## ✅ 四、Python 脚本处理流程（示意）

### 1. 将 Parquet 写入数据库

```python
import pandas as pd
from sqlalchemy import create_engine

# 读取数据
df = pd.read_parquet("EDDF_2024_01.parquet")

# 连接数据库
engine = create_engine("postgresql://postgres:mysecretpassword@localhost:5432/postgres")

# 假设你已提取 taxi_in_events
df_events.to_sql("taxi_in_events", engine, if_exists="append", index=False)
```

### 2. 进行查询/建模/分析

```python
query = """
SELECT *
FROM taxi_in_events
WHERE airport_code = 'EDDF'
  AND timestamp_landing BETWEEN '2024-01-01' AND '2024-01-31'
"""
df = pd.read_sql(query, engine)
```

---

## ✅ 五、结合 DVC 管理数据版本（可选）

* 每次数据清洗 + Taxi-in 提取后，用 DVC 管理输出：

```bash
dvc init
dvc add eddf_taxi_in_2024_01.csv
git add eddf_taxi_in_2024_01.csv.dvc .gitignore
git commit -m "Add taxi-in data for EDDF Jan 2024"
```

* 后续你可以记录哪些版本的数据用于模型训练，追踪来源。

---

## ✅ 六、可选扩展：分布式处理

| 工具         | 用法                             |
| ---------- | ------------------------------ |
| **Dask**   | 替代 Pandas 读取大规模 Parquet，支持多核计算 |
| **Spark**  | 可部署到 Synology 的 K3s 上进行多机场特征工程 |
| **DuckDB** | 直接用 SQL 读取 Parquet 文件，无需数据库    |

---

## ✅ 七、可视化建议（Streamlit 简单示例）

```python
import streamlit as st
import pandas as pd
import sqlalchemy

engine = sqlalchemy.create_engine("postgresql://postgres:mysecretpassword@localhost:5432/postgres")

st.title("Taxi-in Duration Overview")

airport = st.selectbox("Select Airport", ['EDDF', 'EHAM', 'LFPG'])
query = f"SELECT * FROM taxi_in_events WHERE airport_code = '{airport}'"
df = pd.read_sql(query, engine)

st.write(df.describe())
st.bar_chart(df['duration_sec'])
```

---

## ✅ 八、总结：你的下一步行动建议

| 步骤         | 内容                                             |
| ---------- | ---------------------------------------------- |
| ✅ 步骤 1     | 在 Synology NAS 上部署 PostgreSQL + TimescaleDB 容器 |
| ✅ 步骤 2     | 构建 Parquet 数据保存结构（每机场/每月）                      |
| ✅ 步骤 3     | 开发 Python 脚本：Parquet→事件提取→写入 DB                |
| ✅ 步骤 4     | 使用 Pandas/Dask 查询数据分析，或构建 ML pipeline          |
| ✅ 步骤 5     | 用 Streamlit 构建 Dashboard 或输出报告                 |
| ✅ 步骤 6（可选） | 使用 DVC 记录数据版本，支持模型复现                           |

---

如果你希望我为你：

* 写 Docker Compose 文件
* 提供 TimescaleDB 示例数据表
* 给你搭一个完整的 Streamlit Demo
* 或是编写提取+写入数据库的批处理脚本

请告诉我你想从哪一步开始，我来一步步帮你搭建。
