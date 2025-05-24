当然可以！下面是一个完整的 **从零开始用 Docker 搭建 PostgreSQL + pgAdmin4 + dvdrental 示例数据** 的详细步骤，适合你直接上手。

---

## 🎯 目标：本地部署一个 PostgreSQL 容器，自动导入 `dvdrental` 示例数据库，并通过 pgAdmin4 可视化管理。

---

## ✅ 第 1 步：准备工作

### 🧰 你需要：

* 已安装 [Docker](https://www.docker.com/products/docker-desktop)
* 一个空文件夹，例如：

  ```bash
  mkdir pg-dvdrental-demo && cd pg-dvdrental-demo
  ```

---

## ✅ 第 2 步：下载示例数据库文件

### 🗃 下载 `dvdrental.tar`

从 PostgreSQL 官方教程中下载：

* 官网地址：[https://www.postgresqltutorial.com/postgresql-sample-database/](https://www.postgresqltutorial.com/postgresql-sample-database/)

或者直接命令下载：

```bash
wget https://www.postgresqltutorial.com/wp-content/uploads/2019/05/dvdrental.zip
unzip dvdrental.zip
mv dvdrental.tar ./   # 移到当前目录
```

---

## ✅ 第 3 步：创建 `docker-compose.yml` 文件

在当前目录中创建 `docker-compose.yml`：

```yaml
version: '3'
services:
  postgres:
    image: postgres:15
    container_name: dvdrental-db
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: 123456
      POSTGRES_DB: dvdrental
    ports:
      - "5432:5432"
    volumes:
      - ./dvdrental.tar:/docker-entrypoint-initdb.d/dvdrental.tar

  pgadmin:
    image: dpage/pgadmin4
    container_name: pgadmin4
    restart: always
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@example.com
      PGADMIN_DEFAULT_PASSWORD: admin
    ports:
      - "8080:80"
```

✅ 说明：

* PostgreSQL 启动时会自动检测 `dvdrental.tar`，并还原到 `dvdrental` 数据库中
* pgAdmin 启动后通过浏览器访问 `http://localhost:8080`

---

## ✅ 第 4 步：启动容器

在该目录运行：

```bash
docker-compose up -d
```

* 第一次启动可能需要下载镜像，稍等片刻
* 启动成功后，你将看到两个容器在运行：

  ```bash
  docker ps
  ```

---

## ✅ 第 5 步：使用 pgAdmin4 访问 PostgreSQL

### 🔑 登录 pgAdmin4

* 打开浏览器访问：`http://localhost:8080`
* 登录账号密码（在 compose 中配置）：

  * **Email:** `admin@example.com`
  * **Password:** `admin`

### 🔗 添加 PostgreSQL 连接

1. 在 pgAdmin4 左上角点击 **"Add New Server"**

2. 设置连接信息：

   * **Name:** dvdrental
   * 选择 `Connection` 标签页：

     * **Host name/address:** `postgres`（注意是服务名，不是 localhost）
     * **Port:** `5432`
     * **Username:** `postgres`
     * **Password:** `123456`
     * ✅ 勾选 "Save Password"

3. 连接后，你会看到：

   * `dvdrental` 数据库
   * 多个表（如 actor、customer、film 等）
   * 可以开始写查询、看表结构、导出数据等操作

---

## ✅ 第 6 步：测试 SQL 查询

在 pgAdmin 中打开 Query Tool，试试下面的语句：

```sql
SELECT title, rental_rate
FROM film
ORDER BY rental_rate DESC
LIMIT 10;
```

---

## ✅ 第 7 步（可选）：停止和清理容器

* 停止：

  ```bash
  docker-compose down
  ```
* 清理数据（删除 volume）：

  ```bash
  docker-compose down -v
  ```

---

## ✅ 总结图解

```
📦 pg-dvdrental-demo/
├── docker-compose.yml
└── dvdrental.tar
```

服务启动后，你就拥有了：

* 🐘 PostgreSQL 数据库（含 dvdrental 数据）
* 🖥️ pgAdmin4 可视化管理界面
* 🌐 浏览器访问地址：`http://localhost:8080`

---

如你希望我帮你生成这个完整目录和文件压缩包，或部署其它示例数据（如 pagila、northwind），只需告诉我。我也可以提供一个一键执行的脚本给你打包部署。是否需要我为你打包这个 demo 环境？
