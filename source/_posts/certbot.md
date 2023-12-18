---
title: 'Certbot 申请证书以及续费'
date: 2023-07-03 21:40:29
tags: []
---
安装过程跳过

# 申请证书
通过DNS验证域名：
```bash
certbot certonly -d *.yokinanya.icu --manual --preferred-challenges dns
```
按照提示在域名服务商处，添加对应的DNS TXT解析记录。
配置好后按回车继续，如果是修改记录的话，建议等一个TTL的时间再按回车

# 续费
自动续费：
```bash
certbot renew
```
可以设置个定时任务：
```bash
1 1 */1 * * certbot renew > /var/log/crontab/certbot
```
后面日志文件路径随便改，前面定时也可以随便改

强制重签:
```bash
certbot certonly --manual -d *.yokinanya.icu
```