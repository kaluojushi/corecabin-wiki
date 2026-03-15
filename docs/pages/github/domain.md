# 绑定自定义域名

将自定义域名绑定到 GitHub Pages，可以让你的网站使用自己的域名而不是默认的 `username.github.io` 域名。

## 前置条件

1. 确保你拥有一个域名（可以从阿里云、腾讯云、GoDaddy、Cloudflare 等服务商购买）
2. 确保 GitHub Pages 网站已成功部署并可以访问
3. 拥有域名的 DNS 解析管理权限
4. GitHub 仓库的 Admin 权限

## 绑定步骤

### 1. 获取 GitHub Pages 的 IP 地址

首先需要获取 GitHub Pages 服务器的 IP 地址，打开终端执行：

```bash
ping github.io
```

你会看到类似以下输出：

```
PING github.io (185.199.108.153): 56 data bytes
64 bytes from 185.199.108.153: icmp_seq=0 ttl=54 time=12.345 ms
```

GitHub Pages 的 IP 地址有四个：

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

### 2. 在 GitHub 仓库中添加 CNAME 文件

有两种方式添加 CNAME 文件：

#### 方式一：通过 GitHub 网页界面

1. 进入你的 GitHub 仓库
2. 点击 **Settings** 选项卡
3. 在左侧菜单中选择 **Pages**
4. 在 **Custom domain** 输入框中填写你的域名（例如：`example.com`）
5. 点击 **Save**
6. GitHub 会自动在仓库根目录创建 CNAME 文件

#### 方式二：手动创建 CNAME 文件

1. 在仓库根目录（或 `docs` 目录，取决于你的 Pages 配置）创建名为 `CNAME` 的文件
2. 文件内容只包含你的域名，没有其他内容：

```
example.com
```

3. 提交并推送到 GitHub

### 3. 配置域名 DNS 解析

根据你的域名类型，配置不同的 DNS 记录：

#### 顶点域名（Apex Domain）

如果你使用顶点域名（如 `example.com`），需要配置 A 记录：

```
类型: A
名称: @
值: 185.199.108.153

类型: A
名称: @
值: 185.199.109.153

类型: A
名称: @
值: 185.199.110.153

类型: A
名称: @
值: 185.199.111.153
```

#### 子域名（Subdomain）

如果你使用子域名（如 `www.example.com` 或 `blog.example.com`），需要配置 CNAME 记录：

```
类型: CNAME
名称: www (或你的子域名)
值: username.github.io
```

将 `username` 替换为你的 GitHub 用户名。

### 4. 各大域名服务商配置示例

#### 阿里云配置

1. 登录 [阿里云域名控制台](https://dc.console.aliyun.com/)
2. 找到对应域名，点击 **解析**
3. 点击 **添加记录**
4. 添加四条 A 记录（顶点域名）：

| 记录类型 | 主机记录 | 解析线路 | 记录值 | TTL |
|---------|---------|---------|--------|-----|
| A | @ | 默认 | 185.199.108.153 | 10分钟 |
| A | @ | 默认 | 185.199.109.153 | 10分钟 |
| A | @ | 默认 | 185.199.110.153 | 10分钟 |
| A | @ | 默认 | 185.199.111.153 | 10分钟 |

5. 如果需要 www 子域名，添加 CNAME 记录：

| 记录类型 | 主机记录 | 解析线路 | 记录值 | TTL |
|---------|---------|---------|--------|-----|
| CNAME | www | 默认 | username.github.io | 10分钟 |

#### 腾讯云配置

1. 登录 [腾讯云域名控制台](https://console.cloud.tencent.com/cns)
2. 找到对应域名，点击 **解析**
3. 点击 **添加记录**
4. 配置方式与阿里云类似

#### Cloudflare 配置

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 选择你的域名
3. 进入 **DNS** → **Records**
4. 点击 **Add Record**
5. 添加记录：

| Type | Name | IPv4 address | Proxy status |
|------|------|--------------|--------------|
| A | @ | 185.199.108.153 | DNS only |
| A | @ | 185.199.109.153 | DNS only |
| A | @ | 185.199.110.153 | DNS only |
| A | @ | 185.199.111.153 | DNS only |
| CNAME | www | username.github.io | DNS only |

**注意**：建议将 Proxy status 设为 "DNS only"（灰色云朵），以便 GitHub Pages 正确配置 SSL。

#### GoDaddy 配置

1. 登录 GoDaddy 账户
2. 进入 **My Products** → **Domains**
3. 点击域名旁边的 **DNS**
4. 在 **DNS Management** 页面添加记录：

**A 记录**：
- Type: A
- Host: @
- Points to: 185.199.108.153
- TTL: 600 seconds

重复添加其他三个 IP 地址。

**CNAME 记录**（www 子域名）：
- Type: CNAME
- Host: www
- Points to: username.github.io
- TTL: 1 Hour

### 5. 等待 DNS 生效

DNS 解析生效时间取决于 TTL 设置和 DNS 服务商：

- **最快**：几分钟
- **一般**：10-30 分钟
- **最慢**：48 小时（罕见）

验证 DNS 是否生效：

```bash
# 查询 A 记录
dig example.com +short

# 查询 CNAME 记录
dig www.example.com +short

# 或使用 nslookup
nslookup example.com
nslookup www.example.com
```

你应该能看到 GitHub Pages 的 IP 地址或 CNAME 指向。

### 6. 启用 HTTPS

DNS 生效后，启用 HTTPS：

1. 进入 GitHub 仓库的 **Settings** → **Pages**
2. 在 **Custom domain** 下方，勾选 **Enforce HTTPS**
3. 如果选项不可用，等待几分钟后再试（GitHub 需要时间配置 SSL 证书）

## 高级配置

### 同时配置顶点域名和 www 子域名

推荐做法：

1. 在 GitHub Pages 设置中将顶点域名（`example.com`）设为自定义域名
2. 配置顶点域名的 A 记录
3. 配置 www 子域名的 CNAME 记录指向 `username.github.io`
4. GitHub 会自动将 www 子域名重定向到顶点域名

### 为项目页面配置自定义域名

如果你的 GitHub Pages 是项目页面（如 `username.github.io/repo-name`），可以：

1. 在项目仓库的 **Settings** → **Pages** 中设置自定义域名
2. CNAME 文件需要放在项目的 `gh-pages` 分支或 `main` 分支的 `docs` 目录

### 子域名配置示例

配置多个子域名指向不同的 GitHub Pages：

```
类型: CNAME
名称: blog
值: username.github.io

类型: CNAME
名称: docs
值: username.github.io
```

然后在各自的仓库中设置对应的自定义域名。

## DNS 记录类型详解

### A 记录 vs CNAME 记录

| 记录类型 | 使用场景 | 优点 | 缺点 |
|---------|---------|------|------|
| A 记录 | 顶点域名（如 example.com） | 兼容性好 | IP 地址变更需手动更新 |
| CNAME 记录 | 子域名（如 www.example.com） | 自动跟随目标 IP | 不能用于顶点域名 |

### ALIAS/ANAME 记录

部分 DNS 服务商（如 Cloudflare、DNSimple）支持 ALIAS 或 ANAME 记录，可以在顶点域名上使用类似 CNAME 的功能：

```
类型: ALIAS (或 ANAME)
名称: @
值: username.github.io
```

## SSL/HTTPS 证书

GitHub Pages 为自定义域名提供免费的 SSL 证书：

### 证书特点

- **提供商**：Let's Encrypt
- **自动配置**：DNS 生效后自动申请
- **自动续期**：证书到期前自动续期
- **免费**：完全免费

### 证书申请流程

1. 在 GitHub Pages 设置中添加自定义域名
2. 等待 DNS 完全生效
3. GitHub 自动检测域名并申请证书
4. 证书申请成功后（通常几分钟到几小时），启用 **Enforce HTTPS**

### 常见 SSL 问题

**问题：Enforce HTTPS 选项不可用**

解决方案：
- 等待 DNS 完全生效（最长 48 小时）
- 确保所有 A 记录都已正确配置
- 取消并重新添加自定义域名
- 在 **Pages** 设置页面取消勾选 **Enforce HTTPS**，等待片刻后重新勾选

**问题：SSL 证书申请失败**

解决方案：
- 检查 DNS 记录是否正确
- 确保 CNAME 文件中只包含域名，没有多余空格或换行
- 等待一段时间后重试

## 常见问题

### 1. 域名访问显示 404

**原因**：CNAME 文件未正确配置或 DNS 未生效

**解决方案**：
- 检查仓库中是否存在 CNAME 文件
- 确认 CNAME 文件内容是否正确（只包含域名）
- 验证 DNS 记录是否正确配置
- 等待 DNS 完全生效

### 2. 域名被标记为 "Improperly configured"

**原因**：DNS 记录配置不完整

**解决方案**：
- 确保所有四条 A 记录都已添加
- 检查 CNAME 记录是否指向正确的 GitHub Pages 地址
- 在 GitHub Pages 设置中移除并重新添加域名

### 3. www 和非 www 访问不一致

**原因**：只配置了其中一个域名

**解决方案**：
- 同时配置顶点域名和 www 子域名的 DNS 记录
- 在 GitHub Pages 中将其中一个设为主域名
- GitHub 会自动处理重定向

### 4. DNS 解析正常但无法访问

**原因**：可能是 GitHub Pages 配置问题

**解决方案**：
- 检查 GitHub Pages 是否从正确的分支和目录部署
- 查看 GitHub 仓库的 Actions 标签页，确认部署是否成功
- 移除自定义域名，等待几分钟后重新添加

### 5. 自定义域名被其他仓库占用

**原因**：一个域名只能绑定到一个 GitHub Pages 仓库

**解决方案**：
- 在之前绑定的仓库中移除自定义域名
- 删除旧的 CNAME 文件
- 等待 DNS 更新

### 6. Cloudflare 代理导致的证书问题

**原因**：Cloudflare 的代理模式可能影响 GitHub Pages 的 SSL 验证

**解决方案**：
- 在 Cloudflare DNS 设置中，将代理状态设为 "DNS only"（灰色云朵）
- 或在 Cloudflare SSL/TLS 设置中选择 "Full" 模式

## 验证清单

绑定域名后，按此清单验证：

- [ ] CNAME 文件存在于仓库中
- [ ] CNAME 文件只包含域名，无多余内容
- [ ] DNS A 记录配置了所有四个 GitHub Pages IP
- [ ] DNS CNAME 记录指向 username.github.io（如需要）
- [ ] DNS 解析验证成功（dig/nslookup）
- [ ] 通过自定义域名可以访问网站
- [ ] GitHub Pages 设置中显示域名已验证
- [ ] HTTPS 已启用并正常工作
- [ ] www 和非 www 域名重定向正常

## 参考资源

- [GitHub Pages 官方文档 - Custom Domains](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [GitHub Pages 官方文档 - Managing a custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
- [GitHub Pages 官方文档 - HTTPS](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/securing-your-github-pages-site-with-https)
- [GitHub Pages 官方文档 - DNS Records](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-a-subdomain)
- [GitHub Pages IP 地址列表](https://api.github.com/meta)（查看 `pages` 字段）
