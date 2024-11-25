---
title: "用Certbot自动获取Let's Encrypt证书"
date: 2023-07-03 21:40:29
updated: 2024-09-20 10:34:11
tags: []
copyright: false
---

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
# 创建软链接，以确保certbot命令可用，如果certbot命令已经能用就不用创建软链接
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

# 申请证书
通过DNS验证域名：
```bash
certbot certonly -d *.yourdomain.com --manual --preferred-challenges dns
```
按照提示在域名服务商处，添加对应的DNS TXT解析记录。
配置好后按回车继续，建议先查询解析是否生效，可以用<https://toolbox.googleapps.com/apps/dig/?lang=zh-CN>查询

然后在nginx中的对应位置添加SSL证书配置并开启ssl

```nginx
listen 443 ssl http2;
listen [::]:443 ssl http2;
server_name www.yokinanya.icu;

ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;
```

# 续签
自动续签：
```bash
certbot renew --force-renewal --post-hook "/usr/sbin/nginx -s reload"
```

强制重签:
```bash
certbot certonly --manual -d *.yourdomain.com
```

如果在执行`certbot renew`时出现如下错误时，可以试试使用[这个脚本](https://github.com/al-one/certbot-auth-dnspod)(仅支持DNSPod)，使用其他服务商的可以自行寻找其他脚本
```bash
--manual --preferred-challenges dns --manual-auth-hook /path_to/certbot-auth-dnspod.sh
```