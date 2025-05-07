---
title: "SSH公钥实现Github免密登录"
date: 2025-05-07T10:05:06+08:00
draft: false
---
## 一、背景

1. 为什么需要SSH公钥登录：传统HTTPS需要每次输入凭据，而SSH更安全便捷
2. 安全性优势：SSH使用非对称加密，比密码认证更难以破解
3. 使用场景：频繁与远程仓库交互的开发场景

## 二、步骤

1. 生成SSH密钥对

`ssh-keygen -t ed25519 -C "your_email@example.com"`

- 解释-t ed25519：使用更安全现代的EdDSA算法代替传统的RSA

2. 添加公钥到GitHub账户

`cat ~/.ssh/id_ed25519.pub`

- 如何在GitHub设置中添加SSH key：
  1. 登录GitHub → Settings → SSH and GPG keys
  2. 点击"New SSH key"
  3. 粘贴公钥内容
  4. 添加有意义的标题（如"Work Laptop - Ed25519"）
3. 测试连接


`ssh -T git@github.com`

- 解释预期成功响应："Hi username! You've successfully authenticated..."
- 说明首次连接时的known_hosts确认提示

## 三、注意
1. 多密钥管理：为不同账户配置多个密钥（~/.ssh/config文件配置）
2. 协议切换：如何将现有HTTPS仓库切换为SSH协议

`git remote set-url origin git@github.com:username/repo.git`