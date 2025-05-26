**Docker 学习 Roadmap**（适合从零开始到精通）：

---

## 🧭 第一阶段：基础入门（Week 1–2）

### ✅ 目标：了解 Docker 的基本概念、架构和命令行操作

#### 🧱 基础知识

* 什么是 Docker？为什么使用它？
* 容器 vs 虚拟机
* Docker 架构：Client, Server (Daemon), Image, Container, Registry

#### 🧪 实践内容

* 安装 Docker（Mac、Windows、Linux）
* 基本命令操作：

  ```bash
  docker run hello-world
  docker ps -a
  docker start/stop/restart
  docker rm/rmi
  docker exec -it <container> bash
  ```
* 理解 Docker 镜像与容器的区别
* DockerHub 的使用（pull/push/search）

#### 📚 学习资料

* 官方文档：[https://docs.docker.com/get-started/](https://docs.docker.com/get-started/)
* Docker 入门教程（菜鸟/廖雪峰/Bilibili）

---

## ⚙️ 第二阶段：Docker 镜像与容器管理（Week 3–4）

### ✅ 目标：深入理解镜像构建与容器运行机制

#### 🔨 学习内容

* `Dockerfile` 的编写：

  * FROM / RUN / COPY / CMD / ENTRYPOINT / ENV / WORKDIR / EXPOSE
* 使用 `docker build` 构建自定义镜像
* `.dockerignore` 的使用
* 镜像优化技巧（分层、压缩、精简）

#### 🧪 实践项目

* 构建一个基于 Python/Node.js 的 Web 应用容器
* 制作一个带有 cron job 的定时任务容器
* 在容器中运行一个带界面的 JupyterLab

---

## 🧵 第三阶段：数据持久化与网络通信（Week 5）

### ✅ 目标：掌握容器之间的通信与持久化数据方法

#### 🧠 理论内容

* Volume vs Bind Mount（挂载本地目录 vs 管理型卷）
* Docker 网络模式：

  * bridge（默认）
  * host
  * none
  * container:\<name|id>
* 多容器之间的互联（通过自定义网络）

#### 🧪 实践任务

* 使用 `-v` 参数挂载数据
* 使用 `docker network create` 建立私有网络让两个容器互通
* MySQL 容器 + Flask 应用连接测试

---

## 🧰 第四阶段：Docker Compose（Week 6）

### ✅ 目标：学会使用 `docker-compose` 管理多容器应用

#### 📦 学习内容

* `docker-compose.yml` 格式和语法
* 多服务定义（如 web、db、redis）
* 使用 `.env` 文件管理变量
* `docker-compose up/down`、logs、restart 策略

#### 🧪 实战项目

* WordPress + MySQL
* Flask/Node.js + PostgreSQL + Redis
* JupyterLab + TensorFlow 环境搭建

---

## 🌐 第五阶段：部署与远程访问（Week 7–8）

### ✅ 目标：在本地或远程服务器上部署并访问容器化应用

#### 🚀 学习内容

* 将应用部署到：

  * 云服务器（例如使用 Docker + Nginx 部署网站）
  * NAS（如 Synology 上的容器管理）
* 使用 `nginx` 做反向代理
* 映射端口、域名绑定
* 使用 HTTPS（Let's Encrypt + Nginx）

---

## 🛠 第六阶段：进阶与生产环境使用（Week 9–12）

### ✅ 目标：掌握容器编排、CI/CD 集成与安全

#### 🧠 学习内容

* Docker 安全最佳实践（非 root、镜像签名）
* CI/CD 集成（GitHub Actions + Docker 构建 + 发布）
* 镜像缓存与分层优化
* Docker Compose 高级用法（依赖关系、healthcheck）
* 初步接触 Docker Swarm、Kubernetes（了解容器编排）

---

## 📦 第七阶段：实战项目 + 项目部署（持续进行）

### ✅ 目标：通过实战提升实战经验与信心

#### 💡 项目推荐

* 搭建个人博客系统（如 Hugo/MkDocs + Nginx + HTTPS）
* 部署自己的 JupyterLab GPU 深度学习环境
* Flask/Django 数据服务 + PostgreSQL/Redis
* Docker 化 ADS-B 数据分析项目 + Web dashboard 展示
* 构建并部署一个 AI 模型 API（FastAPI + Docker + Gunicorn）

---

## 🎓 额外建议

### 📂 本地文件和数据如何管理

* 选择挂载方式（volume vs bind）
* 自动备份机制设计（脚本 + cron）

### ☁️ 云服务延伸

* Docker + GCP/AWS/Hetzner
* Docker Registry 私有镜像仓库搭建（Harbor、Docker Registry）

### 📚 学习资源推荐

| 类别   | 推荐                                                                                                      |
| ---- | ------------------------------------------------------------------------------------------------------- |
| 视频教程 | Bilibili「码哥字节」「JustDoIT」、「freeCodeCamp」                                                                 |
| 文档资料 | [Docker Docs](https://docs.docker.com/), [Awesome Docker](https://github.com/veggiemonk/awesome-docker) |
| 工具辅助 | Portainer, Watchtower, VSCode Docker 插件                                                                 |

