# 快速开始

本指南帮助你快速创建并部署一个 GitHub Pages 网站。

## 前置条件

- 拥有 GitHub 账户（如果没有，前往 [github.com](https://github.com) 注册）
- 安装 Git（可选，也可使用 GitHub 网页界面上传文件）

## 域名说明

GitHub Pages 提供两种默认域名：

| 仓库名称 | 默认域名 | 用途 |
|---------|---------|------|
| `<username>.github.io` | `https://<username>.github.io/` | 用户/组织主页 |
| 其他名称 | `https://<username>.github.io/<repository>/` | 项目页面 |

将 `<username>` 替换为你的 GitHub 用户名。

## 方式一：创建用户主页

### 1. 创建仓库

1. 登录 GitHub
2. 点击右上角 **+** → **New repository**
3. Repository name 填写 `<username>.github.io`（将 `<username>` 替换为你的用户名）
4. 选择 **Public**（GitHub Pages 免费版需要公开仓库）
5. 勾选 **Add a README file**
6. 点击 **Create repository**

### 2. 添加网站文件

在仓库中创建 `index.html` 文件：

1. 点击 **Add file** → **Create new file**
2. 文件名输入 `index.html`
3. 输入以下内容：

```html
<!DOCTYPE html>
<html>
<head>
    <title>我的主页</title>
</head>
<body>
    <h1>Hello, GitHub Pages!</h1>
    <p>这是我的第一个 GitHub Pages 网站。</p>
</body>
</html>
```

4. 点击 **Commit new file**

### 3. 启用 GitHub Pages

1. 进入仓库页面
2. 点击 **Settings** 选项卡
3. 在左侧菜单中点击 **Pages**
4. 在 **Source** 部分：
   - Branch：选择 `main`（或 `master`）
   - Folder：选择 `/ (root)`
5. 点击 **Save**

### 4. 访问网站

等待 1-2 分钟后，访问 `https://<username>.github.io/` 即可看到你的网站。

在 **Pages** 设置页面顶部会显示你的网站地址。

## 方式二：创建项目页面

### 1. 创建仓库

1. 登录 GitHub
2. 点击右上角 **+** → **New repository**
3. Repository name 填写任意名称（如 `my-site`）
4. 选择 **Public**
5. 勾选 **Add a README file**
6. 点击 **Create repository**

### 2. 添加网站文件

与用户主页相同，创建 `index.html` 文件。

### 3. 启用 GitHub Pages

步骤与用户主页相同。

### 4. 访问网站

访问 `https://<username>.github.io/<repository>/`（将 `<repository>` 替换为仓库名称）。

## 方式三：使用 Git 本地推送

如果你熟悉 Git，可以在本地创建项目后推送：

```bash
# 克隆仓库
git clone https://github.com/<username>/<username>.github.io.git

# 进入目录
cd <username>.github.io

# 创建 index.html
echo '<!DOCTYPE html>
<html>
<head><title>我的主页</title></head>
<body><h1>Hello, GitHub Pages!</h1></body>
</html>' > index.html

# 添加、提交、推送
git add index.html
git commit -m "Add index.html"
git push origin main
```

然后在 GitHub 仓库设置中启用 Pages。

## 默认入口文件

GitHub Pages 按以下顺序查找入口文件：

1. `index.html`
2. `index.md`
3. `README.md`

如果存在 `index.html`，它将作为网站首页；否则会渲染 `README.md`。

## 更新网站

修改仓库中的文件后，GitHub Pages 会自动重新部署：

- **HTML/CSS/JS 文件**：通常 1-2 分钟生效
- **Markdown 文件**：需要 Jekyll 构建，可能稍慢

你可以在仓库的 **Actions** 标签页查看部署状态。

## 下一步

- 添加更多页面和样式
- 学习 HTML/CSS/JavaScript
- 探索 GitHub Pages 的其他功能
