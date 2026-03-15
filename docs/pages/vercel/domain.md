# 绑定自定义域名

## 原理

Vercel 通过 DNS 解析将自定义域名指向 Vercel 服务器，然后自动配置 SSL 证书。

## DNS 记录类型

| 域名类型 | 记录类型 | 名称 | 值 |
|---------|---------|------|-----|
| 根域名（如 `example.com`） | A | @ | `76.76.21.21` |
| 子域名（如 `www.example.com`） | CNAME | www | `cname.vercel-dns.com` |

## 步骤

1. Vercel 项目 **Settings** → **Domains**，添加域名
2. Vercel 会显示需要配置的 DNS 记录
3. 在域名服务商 DNS 解析中添加对应记录
4. 等待 DNS 生效（通常 10-30 分钟）

## 验证 DNS

```bash
dig example.com
dig www.example.com
```

## 多域名配置

一个 Vercel 项目可绑定多个域名。添加域名后，可选择其中一个作为主域名（Primary），其他域名自动重定向。

DNS 配置示例：

```
# 根域名
类型: A, 名称: @, 值: 76.76.21.21

# www 子域名
类型: CNAME, 名称: www, 值: cname.vercel-dns.com

# 其他子域名
类型: CNAME, 名称: blog, 值: cname.vercel-dns.com
```

## 子路径路由

通过 `vercel.json` 配置 Rewrites，将子路径指向另一个 Vercel 项目：

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

## SSL 证书

- DNS 生效后自动申请 Let's Encrypt 证书
- 自动续期
- 证书长时间未生效时，在 Domains 页面点击刷新按钮

## Cloudflare 注意事项

- Proxy status 设为 "DNS only"（灰色云朵），避免影响 Vercel SSL 配置
- 如需使用 Cloudflare CDN，SSL/TLS 模式选择 "Full" 或 "Full (strict)"

## 常见问题

| 问题 | 原因 | 解决方案 |
|-----|------|---------|
| Invalid Configuration | DNS 配置错误或未生效 | 检查记录类型和值，等待 DNS 生效 |
| 无法添加域名 | 域名已被其他项目使用 | 移除其他项目的绑定 |
| HTTP 未重定向到 HTTPS | 浏览器缓存 | 清除缓存，确认 HTTPS 重定向已开启 |
| www 和非 www 不一致 | 未设置主域名 | 在 Domains 设置中指定主域名 |

## 参考

- [Vercel 官方文档 - Custom Domains](https://vercel.com/docs/projects/domains)
- [Vercel 官方文档 - DNS Records](https://vercel.com/docs/projects/domains/dns-records)
