
## ✅ 一、官方/经典推荐镜像（稳定可靠）

### 1. **Jupyter Data Science Notebook**

📦 镜像名：`jupyter/datascience-notebook`

* **内置内容**：

  * Python + R + Julia
  * `numpy`, `pandas`, `scikit-learn`, `matplotlib`, `seaborn`
  * `conda`, `pip`, `jupyterlab`
* **启动命令**：

```bash
docker run -p 8888:8888 jupyter/datascience-notebook
```

* 访问地址：[http://localhost:8888](http://localhost:8888)



### 2. **Jupyter TensorFlow Notebook**

📦 镜像名：`jupyter/tensorflow-notebook`

* ✅ 包含 `tensorflow`, `keras`, `pandas`, `matplotlib`, `sklearn`
* 对于深度学习入门非常适合

```bash
docker run -p 8888:8888 jupyter/tensorflow-notebook
```



### 3. **ContinuumIO Miniconda3**（官方最小conda基础环境）

📦 镜像名：`continuumio/miniconda3`

* 非常干净，可以按需安装所有库
* 推荐用于高级用户自定义环境

```bash
docker run -it continuumio/miniconda3 bash
```



## ✅ 二、高级开发镜像（适合专业项目）

### 4. **dataspell/ds-base**（JetBrains DataSpell Docker 环境）

* JetBrains 官方提供的 Python 数据科学镜像，含 PyTorch、Jupyter、科学库等
* 可用于 VS Code / JetBrains IDE 的远程开发环境
* Docker Hub 地址：[https://hub.docker.com/r/dataspell/ds-base](https://hub.docker.com/r/dataspell/ds-base)



### 5. **Kaggle 官方开发环境**

📦 镜像名：`kaggle/python`

* 同步 Kaggle 竞赛环境，包含：

  * `scikit-learn`, `lightgbm`, `xgboost`, `catboost`
  * `tensorflow`, `torch`, `fastai`, `spacy`
  * `opencv`, `matplotlib`, `seaborn`, `plotly`, `nlp` 包

```bash
docker run -it kaggle/python bash
```

> ⚠️ 注意：这个镜像体积较大（> 5GB）



### 6. **PyTorch / TensorFlow 官方镜像（适合深度学习）**

* 📦 `pytorch/pytorch`: 包含 PyTorch + Jupyter + torchvision + CPU/GPU 支持
* 📦 `tensorflow/tensorflow`: 包含 TF + Jupyter + 常用工具

示例：

```bash
# PyTorch
docker run -it --rm pytorch/pytorch

# TensorFlow with Jupyter
docker run -p 8888:8888 tensorflow/tensorflow:latest-jupyter
```



## ✅ 三、自定义 Dockerfile（适合高级定制）

你也可以创建自己的 Dockerfile：

```Dockerfile
FROM continuumio/miniconda3

RUN conda install -y \
    numpy pandas matplotlib seaborn scikit-learn \
    jupyterlab notebook ipykernel

WORKDIR /workspace
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--allow-root", "--no-browser"]
```

构建并运行：

```bash
docker build -t my-ds-env .
docker run -p 8888:8888 -v $HOME/dev:/workspace my-ds-env
```



## ✅ 下一步：

* 🔧 配置一个包含 PyTorch / TensorFlow + 数据科学工具的 Dockerfile？
* 🧩 设置好 Jupyter + VS Code 的远程开发体验？
* 📦 创建 docker-compose 配置以便项目管理和挂载数据目录？

