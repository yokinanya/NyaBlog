---
title: '通过 leaflow 部署 VTsuruEventFetcher'
date: 2025-11-16 17:15:30
updated: 2025-11-16 17:34:07
tags: ['leaflow']
copyright: false
---

# 前期准备 / Preparations
- 一个 Leaflow 账号，已创建对应的工作空间，任意地区都行，不需要外部访问

# 教程 / Tutorial
进入 [Leaflow 部署清单页面](https://leaflow.net/apply) ，填入以下内容
```yaml
kind: Deployment
name: vtsuru
replicas: 1
image_pull_secrets: []
labels: {}
containers:
- name: vtsuru
    image: ghcr.io/megghy/vtsurueventfetcher.net:latest
    working_dir: ''
    command: []
    args: []
    ports: []
    env:
    - name: VTSURU_TOKEN
        value: <token>
    - name: COOKIE_CLOUD
        value: <key@password>
    env_from_configmap: []
    env_from_secret: []
    resources:
    cpu: 200
    memory: 128
    ephemeral_storage: 1024
    volume_mounts: []
    configmap_mounts: []
    secret_mounts: []
init_containers: []
```
其中:

**{token} 需要替换为从<vtsuru.live> 面板 处获取的那串**

**{key@password} 替换为 CookieCloud 的 Key + @ + 端到端密码, 详见 [关于 CookieCloud](https://www.wolai.com/8XYEoE1UxxXou6WsxfTjTw)**

如果不需要 CookieCloud 可以把这部分配置去掉
```yaml
- name: COOKIE_CLOUD
    value: <key@password>
```

参考：[VTsuruEventFetcher Docker部署教程](https://www.wolai.com/ePg5MCvxULsub2YYau8eUM)