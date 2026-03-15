# 绑定自定义域名

## 原理

通过 DNS 解析将自定义域名指向 GitHub Pages 服务器，GitHub 自动为域名配置 SSL 证书。

## GitHub Pages IP 地址

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

可通过 `ping github.io` 或访问 [https://api.github.com/meta](https://api.github.com/meta) 的 `pages` 字段获取。

## DNS 记录配置

### 顶点域名（Apex Domain）

如 `example.com`，配置四条 A 记录：

```
类型: A, 名称: @, 值: 185.199.108.153
类型: A, 名称: @, 值: 185.199.109.153
类型: A, 名称: @, 值: 185.199.110.153
类型: A, 名称: @, 值: 185.199.111.153
```

### 子域名（Subdomain）

如 `www.example.com`，配置 CNAME 记录：

```
类型: CNAME, 名称: www, 值: <username>.github.io
```

### ALIAS/ANAME 记录

部分 DNS 服务商（Cloudflare、DNSimple）支持在顶点域名使用 ALIAS/ANAME：

```
类型: ALIAS, 名称: @, 值: <username>.github.io
```

## 步骤

1. 在仓库根目录创建 `CNAME` 文件，内容为域名（不含协议和路径）
2. 或在 **Settings** → **Pages** → **Custom domain** 中输入域名，GitHub 自动创建 CNAME 文件
3. 在域名服务商添加 DNS 记录
4. 等待 DNS 生效（通常 10-30 分钟）
5. 在 **Settings** → **Pages** 中勾选 **Enforce HTTPS**

## 验证 DNS

```bash
dig example.com +short
dig www.example.com +short
```

## 同时配置顶点域名和 www

1. 将顶点域名设为 Custom domain
2. 配置顶点域名的 A 记录
3. 配置 www 的 CNAME 记录
4. GitHub 自动将 www 重定向到顶点域名

## SSL 证书

- DNS 生效后自动申请 Let's Encrypt 证书
- 自动续期
- Enforce HTTPS 选项不可用时，等待几分钟或重新添加域名

## Cloudflare 注意事项

- Proxy status 设为 "DNS only"（灰色云朵）
- 或 SSL/TLS 模式选择 "Full"

## 常见问题

| 问题 | 原因 | 解决方案 |
|-----|------|---------|
| 404 | CNAME 文件未配置或 DNS 未生效 | 检查 CNAME 文件和 DNS 记录 |
| Improperly configured | DNS 记录不完整 | 确保四条 A 记录都已添加 |
| www 和非 www 不一致 | 只配置了其中一个 | 同时配置两者的 DNS 记录 |
| 域名被其他仓库占用 | 一个域名只能绑定一个仓库 | 移除旧仓库的绑定 |

## 参考

- [GitHub Pages 官方文档 - Custom Domains](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [GitHub Pages 官方文档 - HTTPS](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/securing-your-github-pages-site-with-https)
