---
title: 'GPG密钥的配置以及在Git中的使用'
date: 2024-05-12 13:40:54
updated: 2024-05-12 13:41:00
tags: ['git','gpg','canokey']
---


# 安装与配置GPG
> 虽然Git自带GPG，但这里仍单独安装了GPG程序

Windows 用户可下载 [Gpg4Win](https://gpg4win.org/download.html)，Linux/macOS 用户使用对应包管理软件安装即可。

安装完成后在Git配置gpg路径
```bash
git config --global gpg.program "C:\Program Files (x86)\GnuPG\bin\gpg.exe"
```

# 创建与设置密钥
建议参考 <https://editst.com/2022/canokey-guide/#OpenPGP> 生成并配置密钥，并且建议对密钥进行备份。如果有需要可以把密钥导入到智能卡内（如Canokey/Yubikey），如果没有智能卡，在完成备份密钥后可以继续之后的操作。

需要注意：**主密钥中的用户名，邮箱地址，需要和Github或其他Git平台中使用的一致**，Comment仅为备注，可以留空

# 在Git中配置密钥
在Git中添加密钥
```bash
git config --global user.signingkey <GPG key ID> # GPG key ID是子密钥中用于签名的密钥
```
之后在`git commit`时使用`-S`参数即可使用gpg进行签名。也可以在配置中配置自动gpg签名，此处不建议开启全局选项，因为有的脚本可能会使用 `git am` 之类的涉及到 commit 的命令，如果全局开启的话会导致问题。
```bash
git config commit.gpgsign true
```
如果提交到 GitHub，前往 [GitHub SSH and GPG keys](https://github.com/settings/keys) 添加公钥。此处添加后，可以直接通过对应 GitHub ID 来获取公钥：https://github.com/<yourid\>.gpg


文章有参考以下内容：
[GPG密钥的配置与Git的使用
](https://kiyamou.github.io/2019/06/06/git-use-0/)
[Canokey 指南：OTP，FIDO2，PGP 与 PIV
](https://editst.com/2022/canokey-guide)