---
title: "用Certbot自动获取Let's Encrypt证书"
date: 2023-07-03 21:40:29
updated: 2024-05-12 14:26:28
tags: []
---

# Let’s Encrypt
Let’s Encrypt 是一家全球性的证书颁发机构（CA），作为一个非营利性组织，它的任务是通过推广 HTTPS 来创建一个更加安全和尊重隐私的 Web 环境。Let’s Encrypt 提供了免费的 SSL 证书供每个人使用。

Let’s Encrypt 使用自动化证书管理环境（ACME）协议来验证域名控制权以及颁发证书，使用 Certbot 首次连接 Let’s Encrypt 服务器时会要求输入邮箱等信息来自动创建账户，帐户 ID 可以在 /etc/letsencrypt/accounts/acme-v02.api.letsencrypt.org/directory/<ID\> 路径中找到，如果在使用中遇到问题，可以提供账户 ID 信息进行反馈。

# SSL证书类型
目前主流的 SSL 证书有以下三种：

域名验证型（DV）证书：通过验证域名所有权即可签发证书，只验证网站域名所有权，适合个人和小微企业申请，能起到加密传输的作用，但是证书中无法显示企业信息。
组织验证型（OV）证书：通过验证域名所有权和申请企业的真实身份信息才能签发证书，适合中型企业和互联网业务申请，能通过证书查看到企业相关信息。
扩展验证型（EV）证书：在 OV 证书的基础上额外验证企业的其他相关信息，比如 GoDaddy 会在在授予企业 EV 证书前验证企业是否符合以下条件：已合法注册、目前正常运营、位于所列地址、所列电话号码有效、拥有网站域名。多使用于银行、金融、证券、支付等高安全标准行业。
Let’s Encrypt 提供的是域名验证型（DV）证书，不提供组织验证型（OV）或扩展验证型（EV）证书。

# 安装Certbot

Certbot 团队建议使用 Snap 安装 Certbot ，以避免其他包管理系统上发行的 Cerbot 因为未及时更新而存在问题。

如果使用的是其他 Linux 发行版或者你使用的不是 Linux 可以查阅 [certbot instructions](https://certbot.eff.org/instructions)，当然Certbot也可以通过 pip 安装，但并不建议使用该方式进行安装

下面以Ubuntu为例简单介绍一下Certbot的安装流程：

如果使用 apt、dnf 或 yum 等操作系统软件包管理器安装了任何 Certbot 软件包，应在安装 Certbot snap 之前删除它们，以确保运行 certbot 命令时使用的是 snap，而不是操作系统软件包管理器的安装。具体的命令视操作系统而定，常见的有 `sudo apt-get remove certbot`、`sudo dnf remove certbot` 或 `sudo yum remove certbot`。

```bash
# ubuntu可以直接通过apt安装snap
sudo apt update
sudo apt install snapd
sudo snap install --classic certbot
# 创建软链接，以确保certbot命令可用
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

# 申请证书
通过DNS验证域名：
```bash
certbot certonly -d *.yokinanya.icu --manual --preferred-challenges dns
```
按照提示在域名服务商处，添加对应的DNS TXT解析记录。
配置好后按回车继续，建议先使用dig查询解析是否生效，或者前往<https://toolbox.googleapps.com/apps/dig/?lang=zh-CN>查询

然后在nginx中的对应位置添加SSL证书配置并开启ssl

```nginx
listen 443 ssl http2;
listen [::]:443 ssl http2;
server_name www.yokinanya.icu;

ssl_certificate /etc/letsencrypt/live/yokinanya.icu/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/yokinanya.icu/privkey.pem;
```

# 续费
自动续费：
```bash
certbot renew
```
可以设置个定时任务：
```bash
1 1 */1 * * certbot renew > /var/log/crontab/certbot
```

强制重签:
```bash
certbot certonly --manual -d *.yokinanya.icu
```