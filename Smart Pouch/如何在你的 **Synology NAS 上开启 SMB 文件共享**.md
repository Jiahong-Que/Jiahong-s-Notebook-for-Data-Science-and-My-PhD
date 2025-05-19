我们一步一步来讲解如何在你的 **Synology NAS 上开启 SMB 文件共享**，以便作为 DVC 的远程数据存储用。

---

## ✅ 目标

在 Synology NAS 上创建一个共享文件夹（例如 `dvc-store`），并通过 SMB 协议使你的 Mac 可以访问它，从而作为 DVC 的远程存储。

---

## 🔧 步骤详解：Synology 上创建 SMB 文件共享目录

### 📌 步骤 1：登录 DSM 系统

1. 打开浏览器，输入 NAS 的地址，例如：

   ```
   http://192.168.1.10:5000
   ```
2. 使用你的 Synology 用户名和密码登录 DSM。

---

### 📌 步骤 2：创建共享文件夹 `dvc-store`

1. 在 DSM 主界面，点击左上角的 **控制面板（Control Panel）**。
2. 点击 **共享文件夹（Shared Folder）**。
3. 点击 **创建（Create）**。
4. 输入以下信息：

   * **名称（Name）**: `dvc-store`
   * **位置（Location）**: 通常默认是 `/volume1`
   * **描述（Description）**: 可以写 “DVC Data Storage”
5. 点击“下一步”数次，根据需要设置：

   * 勾选 `启用回收站`（可选）
   * 不需要启用加密（除非你需要数据加密）
6. 点击 **应用（Apply）** 完成。

---

### 📌 步骤 3：设置文件夹权限

在创建共享文件夹后，系统会要求你设置权限。

1. 找到你当前的用户账户（或者你希望用来访问的用户）
2. 勾选 `读取/写入（Read/Write）`
3. 确保“其他人”无权限，保证安全性。

---

### 📌 步骤 4：启用 SMB 文件服务

1. 打开 **控制面板 > 文件服务（File Services）**
2. 切换到 **SMB/AFP/NFS**
3. 在 **SMB** 标签页中：

   * 勾选 **启用 SMB 服务（Enable SMB Service）**
   * 可以设置 SMB 协议版本为 SMB2 或更高
4. 点击应用

---

## 📥 在 Mac 上访问共享目录

1. 在 Finder 中，按下快捷键 `⌘ + K`
2. 输入以下地址：

   ```
   smb://192.168.1.10/dvc-store
   ```

   > 替换为你的 NAS 局域网 IP 和共享文件夹名
3. 点击“连接”，输入你的 NAS 用户名和密码。
4. 如果成功，会在 Finder 的左侧栏或 `/Volumes/` 下看到 `dvc-store` 被挂载。

---

## 📁 小提示

你也可以用终端挂载：

```bash
mkdir -p /Volumes/dvc-store
mount_smbfs //username@192.168.1.10/dvc-store /Volumes/dvc-store
```

---

## ✅ 下一步（接下来你可以做）

当你在 `/Volumes/dvc-store` 中看到文件夹后，就可以在项目中设置：

```bash
cd your-dvc-project
dvc remote add -d nas_remote /Volumes/dvc-store/smart-pouch-analyzer
```

---

是否需要我下一步详细讲解如何在 DVC 项目中配置这个 remote？还是你先试试挂载 NAS？遇到问题我随时可以帮你。
