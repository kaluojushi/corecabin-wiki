# 绑定自定义域名

将自定义域名绑定到 Vercel 项目，可以让你的网站使用自己的域名而不是默认的 `.vercel.app` 域名。

## 前置条件

1. 确保你拥有一个域名（可以从阿里云、腾讯云、GoDaddy、Cloudflare 等服务商购买）
2. 确保 Vercel 项目已成功部署
3. 拥有域名的 DNS 解析管理权限

## 绑定步骤

### 1. 在 Vercel 中添加域名

1. 进入你的 Vercel 项目仪表板
2. 点击 **Settings** 选项卡
3. 在左侧菜单中选择 **Domains**
4. 在输入框中输入你的域名（例如：`example.com` 或 `www.example.com`）
5. 点击 **Add** 按钮

### 2. 获取 DNS 解析配置

添加域名后，Vercel 会显示需要配置的 DNS 记录。通常有两种情况：

#### A 记录（用于根域名）

如果你添加的是根域名（如 `example.com`），Vercel 会提供 A 记录地址：

```
类型: A
名称: @
值: 76.76.21.21
```

#### CNAME 记录（用于子域名）

如果你添加的是子域名（如 `www.example.com` 或 `blog.example.com`），Vercel 会提供 CNAME 记录：

```
类型: CNAME
名称: www (或你的子域名)
值: cname.vercel-dns.com
```

### 3. 配置域名 DNS 解析

登录你的域名服务商控制台，找到 DNS 解析管理页面，添加相应的记录：

#### 阿里云配置示例

1. 登录 [阿里云域名控制台](https://dc.console.aliyun.com/)
2. 找到对应域名，点击 **解析**
3. 点击 **添加记录**
4. 填写记录信息：
   - **记录类型**：选择 `A` 或 `CNAME`
   - **主机记录**：填写 `@`（根域名）或子域名名称
   - **解析线路**：默认
   - **记录值**：填写 Vercel 提供的地址
   - **TTL**：默认（10 分钟）
5. 点击 **确认** 保存

#### 腾讯云配置示例

1. 登录 [腾讯云域名控制台](https://console.cloud.tencent.com/cns)
2. 找到对应域名，点击 **解析**
3. 点击 **添加记录**
4. 填写记录信息：
   - **主机记录**：填写 `@` 或子域名名称
   - **记录类型**：选择 `A` 或 `CNAME`
   - **线路类型**：默认
   - **记录值**：填写 Vercel 提供的地址
   - **TTL**：默认
5. 点击 **保存**

#### Cloudflare 配置示例

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 选择你的域名
3. 进入 **DNS** → **Records**
4. 点击 **Add Record**
5. 填写记录信息：
   - **Type**：选择 `A` 或 `CNAME`
   - **Name**：填写 `@` 或子域名名称
   - **IPv4 address / Target**：填写 Vercel 提供的地址
   - **Proxy status**：建议关闭代理（灰色云朵），以便 Vercel 正确配置 SSL
6. 点击 **Save**

### 4. 等待 DNS 生效

DNS 解析生效时间取决于 TTL 设置和 DNS 服务商，通常需要：
- **最快**：几分钟
- **一般**：10-30 分钟
- **最慢**：48 小时（罕见）

你可以使用以下命令检查 DNS 是否已生效：

```bash
# 查询 A 记录
dig example.com

# 查询 CNAME 记录
dig www.example.com

# 或使用 nslookup
nslookup example.com
nslookup www.example.com
```

### 5. 验证域名绑定

返回 Vercel 项目的 **Domains** 页面，检查域名状态：

- ✅ **Valid Configuration**：域名配置成功
- ⚠️ **Invalid Configuration**：DNS 配置有误，请检查记录
- ⏳ **Pending**：等待 DNS 生效

## 高级配置

### 同时配置根域名和 www 子域名

推荐同时添加 `example.com` 和 `www.example.com`，并设置重定向：

1. 在 Vercel 中添加两个域名
2. 在域名后面选择其中一个作为主域名（Primary）
3. 另一个域名会自动重定向到主域名

DNS 配置示例：

```
类型: A
名称: @
值: 76.76.21.21

类型: CNAME
名称: www
值: cname.vercel-dns.com
```

### 配置多个子域名

一个 Vercel 项目可以绑定多个域名，重复上述步骤即可：

```
类型: CNAME
名称: blog
值: cname.vercel-dns.com

类型: CNAME
名称: docs
值: cname.vercel-dns.com
```

### 子路径路由

如果需要将子路径（如 `example.com/blog`）指向另一个 Vercel 项目，可以使用 Vercel 的 [Rewrites](https://vercel.com/docs/projects/project-configuration#rewrites) 功能。

在 `vercel.json` 中配置：

```json
{
  "rewrites": [
    {
      "source": "/blog",
      "destination": "https://blog-project.vercel.app"
    }
  ]
}
```

## SSL/HTTPS 证书

Vercel 会自动为你的域名配置免费的 SSL 证书：

- **自动配置**：DNS 生效后，Vercel 会自动申请 Let's Encrypt 证书
- **生效时间**：通常在 DNS 生效后几分钟内完成
- **自动续期**：证书到期前会自动续期，无需手动操作

如果证书长时间未生效，可以尝试：

1. 检查 DNS 是否正确解析到 Vercel
2. 在 Domains 页面点击域名旁边的刷新按钮
3. 确保 DNS 服务商没有开启 DNSSEC（部分情况下可能冲突）

## 常见问题

### 1. 域名显示 "Invalid Configuration"

**原因**：DNS 记录配置错误或未生效

**解决方案**：
- 检查 DNS 记录类型是否正确（A 记录或 CNAME 记录）
- 确认记录值是否正确
- 等待 DNS 完全生效（最长 48 小时）
- 使用 `dig` 或 `nslookup` 命令验证解析结果

### 2. 无法添加域名

**原因**：域名可能已被其他 Vercel 项目或账户使用

**解决方案**：
- 检查是否有其他 Vercel 项目已绑定该域名
- 如果是刚从其他平台迁移，等待 DNS 完全更新
- 联系 Vercel 支持团队

### 3. HTTP 无法重定向到 HTTPS

**原因**：Vercel 默认开启 HTTPS 重定向，但可能被浏览器缓存

**解决方案**：
- 清除浏览器缓存
- 使用隐私/无痕模式访问
- 在 Vercel 项目设置中确认 HTTPS 强制重定向已开启

### 4. www 和非 www 域名访问不一致

**原因**：两个域名分别解析，未设置重定向

**解决方案**：
- 在 Vercel Domains 设置中，将其中一个设为主域名
- 另一个域名会自动 301 重定向到主域名

### 5. Cloudflare 代理导致的证书问题

**原因**：Cloudflare 的代理模式可能影响 Vercel 的 SSL 配置

**解决方案**：
- 在 Cloudflare DNS 设置中，将代理状态改为 "DNS only"（灰色云朵）
- 或在 Cloudflare SSL/TLS 设置中选择 "Full" 或 "Full (strict)" 模式

## 域名服务商特定说明

### Cloudflare

如果使用 Cloudflare 作为 DNS 服务商：

- 建议暂时关闭代理功能（Proxy status 设为 DNS only）
- SSL/TLS 加密模式建议选择 "Full"
- 如需使用 Cloudflare CDN，确保 SSL 模式为 "Full (strict)"

### 国内服务商（阿里云/腾讯云）

- 域名需要完成实名认证和 ICP 备案（如果服务器在中国大陆）
- Vercel 服务器在海外，不需要 ICP 备案
- 解析生效时间通常较快

## 参考资源

- [Vercel 官方文档 - Custom Domains](https://vercel.com/docs/projects/domains)
- [Vercel 官方文档 - DNS Records](https://vercel.com/docs/projects/domains/dns-records)
- [Vercel 官方文档 - SSL Certificates](https://vercel.com/docs/projects/domains/ssl-certificates)
