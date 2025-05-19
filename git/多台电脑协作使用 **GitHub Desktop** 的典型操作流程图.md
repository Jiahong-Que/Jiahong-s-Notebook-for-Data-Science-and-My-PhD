以下是多台电脑协作使用 **GitHub Desktop** 的典型操作流程图，用于保持多个环境下代码同步、避免冲突，并顺利进行版本控制：

---

## 🖥️ 多台电脑协作的 GitHub Desktop 操作流程图

```mermaid
graph TD
  A[电脑 A：开始工作] --> B[Fetch origin]
  B --> C[Pull origin]
  C --> D[新建或切换分支（如 feature-x）]
  D --> E[进行修改]
  E --> F[查看变更并 Commit]
  F --> G[Push origin]

  subgraph 🚧 电脑 B 同步工作
    H[电脑 B：开始工作] --> I[Fetch origin]
    I --> J[Pull origin]
    J --> K[切换同一分支 feature-x]
    K --> L[进行修改]
    L --> M[Commit]
    M --> N[Push origin]
  end

  G --> J

  subgraph 🔁 合并工作完成
    O[切回主分支 main]
    O --> P[Merge feature-x into main]
    P --> Q[Resolve conflicts if any]
    Q --> R[Commit merge]
    R --> S[Push main]
  end

  N --> O
```

---

## ✅ 操作说明（对应图中步骤）

| 步骤编号 | 操作内容                | 建议说明                |
| ---- | ------------------- | ------------------- |
| A/H  | 在任一电脑开始前            | 保证网络连接良好            |
| B/I  | `Fetch origin`      | 拉取远程信息但不合并          |
| C/J  | `Pull origin`       | 同步最新代码，避免冲突         |
| D/K  | 切换/创建分支 `feature-x` | 每次新功能或修改用新分支        |
| E/L  | 修改代码                | 本地正常编辑              |
| F/M  | `Commit`            | 写清楚 message         |
| G/N  | `Push origin`       | 上传更改到远程分支           |
| O    | 切回 `main` 分支        | 准备合并                |
| P    | `Merge into main`   | 来自 `feature-x`      |
| Q    | 解决冲突                | GitHub Desktop 提示操作 |
| R    | `Commit merge`      | 合并后提交               |
| S    | `Push origin`（main） | 主分支推送完成             |

---

## 📝 建议补充工具

| 工具/设置             | 说明                |
| ----------------- | ----------------- |
| `.gitignore`      | 每台电脑建议统一，避免误传     |
| SSH 认证            | 每台电脑设置自己的 SSH key |
| GitHub Actions    | 可设置 push 时自动测试或构建 |
| VS Code + GitLens | 协助查看变更与历史记录       |

---

如果你希望，我可以导出这张图为 PNG/PDF 或给出更详细的 **冲突处理分支图**。是否需要我帮你生成静态版本或其他可打印格式？
