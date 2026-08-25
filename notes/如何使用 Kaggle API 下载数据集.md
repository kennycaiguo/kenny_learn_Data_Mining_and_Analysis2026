# 如何使用 Kaggle API 下载数据集

Kaggle 数据集下载用浏览器经常限速、断点或卡住，尤其是 1GB+ 大文件。用 **Kaggle CLI**（命令行工具）下载速度更快、支持自动续传，还能在脚本/Colab 里自动化。下面分享我刚刚下载 [dasgroup/rba-dataset](https://www.kaggle.com/datasets/dasgroup/rba-dataset)（1.19 GB）的完整流程。

> **为什么 API 下载更好？**
>
> - 速度往往是浏览器 2–5 倍
> - 支持断点续传（命令重跑就继续）
> - 适合批量下载、Jupyter/Colab 自动化

## 准备工作（只需一次，5 分钟搞定）

### 1. 注册/登录 Kaggle

- 访问 https://www.kaggle.com/
- 用 Google 账号或邮箱注册/登录（免费）

### 2. 安装 Kaggle CLI

需要 Python 3.6+（大多数电脑已有）。

在终端（Windows: PowerShell/CMD；Mac/Linux: Terminal）运行：

```bash
# 安装或升级到最新版
pip3 install kaggle --upgrade
```

验证安装：

```bash
kaggle --version
```

看到版本号（如 1.6.x 或更高）就 OK。

### 3. 获取 Legacy API Token（最容易卡的一步）

2025–2026 年 Kaggle API 分成**新版 Token**（KGAT_ 开头）和**Legacy（旧版）**两种。
**经典 CLI（kaggle 命令）目前最稳定，强烈推荐用 Legacy 方式**。

步骤：

1. 登录后打开：https://www.kaggle.com/settings

2. 向下滚动到 **API** 部分

3. 找到 **Legacy API Credentials** 小节（可能需要继续往下拉）

4. 点击

    

   Create Legacy API Key

   （或类似按钮，如 "Create Legacy API Key"）

   - 浏览器会**自动下载** `kaggle.json` 文件

   - 文件内容示例：

     ```json
     {"username": "你的用户名", "key": "一串长密钥"}
     ```

**如果没看到 Legacy 按钮**：

- 先试试点击 "Generate New Token" 生成新版 token（复制字符串备用）
- 但 CLI 更认 Legacy，所以刷新页面或换浏览器重试 Legacy 选项

------

## **下面展示 create token 截图** ![image](https://img2024.cnblogs.com/blog/207221/202602/207221-20260225165758752-1213526408.png)

## **下面展示 create legacy api key，用来生成 kaggle.json 文件**

## ![image](https://img2024.cnblogs.com/blog/207221/202602/207221-20260225165737538-1617836760.png)

### 4. 放置 kaggle.json 文件

- **Windows**：

  - 创建文件夹：`C:\Users\你的用户名\.kaggle`（注意开头点 `.`）
  - 把 `kaggle.json` 放进去

- **Mac / Linux**：

  ```bash
  mkdir -p ~/.kaggle
  mv ~/Downloads/kaggle.json ~/.kaggle/
  chmod 600 ~/.kaggle/kaggle.json   # 必须！设置只读权限
  ```

### 5. 验证配置

运行：

```bash
kaggle config view
```

看到你的 username 等信息 → 配置成功！

## 下载数据集（一行命令搞定）

### 基本下载

找到数据集 URL：https://www.kaggle.com/datasets/dasgroup/rba-dataset
owner/slug 就是 `dasgroup/rba-dataset`

```bash
kaggle datasets download -d dasgroup/rba-dataset
```

![image](https://img2024.cnblogs.com/blog/207221/202602/207221-20260225165917757-2106631065.png)

- 会下载 `rba-dataset.zip` 到当前目录，并自动解压

### 实用选项

```bash
# 下载到指定文件夹
kaggle datasets download -d dasgroup/rba-dataset -p ./my_datasets/rba
 
# 不自动解压
kaggle datasets download -d dasgroup/rba-dataset --unzip false
 
# 强制重新下载（忽略本地文件）
kaggle datasets download -d dasgroup/rba-dataset --force
```

**断网了？** 直接重跑相同命令，它会自动续传！

### 在 Google Colab / Jupyter 中用

```python
!pip install kaggle --upgrade
 
# 上传 kaggle.json 或手动创建
!mkdir -p ~/.kaggle
# 假设你已上传文件到 Colab
!cp kaggle.json ~/.kaggle/
!chmod 600 ~/.kaggle/kaggle.json
 
!kaggle datasets download -d dasgroup/rba-dataset
!unzip rba-dataset.zip -d ./rba_data
```

或者用更现代的 kagglehub（新版库）：

```python
import kagglehub
path = kagglehub.dataset_download("dasgroup/rba-dataset")
print("下载到：", path)
```

## 常见问题排查

- **403 Forbidden** → Token 过期或不对，重新生成 Legacy key 并替换文件
- **命令 not found** → pip install 没成功，或 PATH 没加（重启终端）
- **Legacy 按钮不见** → 页面刷新、换 Chrome/Edge、或先点 Generate New Token 后再看
- **下载慢** → 换 Wi-Fi、有线网，或等非高峰期

## 小结 & 亲测心得

完整流程：安装 kaggle → 生成 Legacy API Key → 放 kaggle.json → 一行命令下载。
我用这个方式下载 1.19 GB 数据集，速度比浏览器快很多，中途断网重跑也无缝续传。强烈推荐！

这个方法在 2026 年 2 月亲测有效（Amsterdam 网络下）。如果 Kaggle 页面又更新