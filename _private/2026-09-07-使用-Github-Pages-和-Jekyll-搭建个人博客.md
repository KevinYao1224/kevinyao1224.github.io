---
title: "使用 Github Pages 和 Jekyll/Chirpy 搭建个人博客"
date: 2026-09-07
categories: [技术分享]
tags: ["Github Pages"]
description: "针对计算机新手的个人博客搭建指南."
---

## 前言

本博客就是用 Jekyll Theme Chirpy 搭建的, 并部署在 Github Pages 上; 一般而言, 搭建成功后的效果与本博客基本相同.

如果不自行付费并进行绑定, 那么你不会拥有类似 `blog.kyee.top` 的网络地址. 相反, 假如你的 Github 账户用户名为 `ABC`, 则你的博客会部署在 `abc.github.io` 上.

为了完成搭建需要的知识或工具有:

- 启动计算机, 访问网络, 使用键盘打字的方法技巧.
- VPN/加速器. 这是为了更流畅地访问 Github 和其他相关技术网站.
- 一个免费的 Github 账户. 本文不会涉及如何注册. 正如上面所说, 如果你的 Github 用户名很晦涩, 你的博客地址也会如此.

## 参考资料

本文参考的以及你可以参考的资料:

- Chirpy 官方博客教程: <https://chirpy.cotes.page/posts/getting-started/>.

## 基础知识

> 以后均假定你的 Github 用户名是 `ABC`.

Jekyll 是一个基于 Ruby 语言及其运行库的博客框架. Chirpy 是基于 Jekyll 开发的博客模板. 通过复制 Chirpy 提供的博客模板并基于 Jekyll 进行部署, 你可以得到一个类似本博客的网页界面.

Github Pages 是由 Github 提供的静态网页托管服务. 由 Jekyll 生成的静态网页文件将会经过 Github Pages 的解析, 被放置在 `abc.github.io` 上, 供所有人公开访问.

## 创建仓库

首先, 进入 `github.com` 并登陆你的账号 `ABC`.

接下来, 访问 Chirpy 主题的官方入门模板仓库 <https://github.com/cotes2020/chirpy-starter>. 在仓库界面的右上角, 点击 `Use this template`, 选择 `Create a new repository`. 此时你将会跳转到仓库创建界面.

在仓库创建页面中, 你必须填写的只有一条: `General` 栏目下 `Owner` 后面的空格 `Repository name` 条目. 你必须在该条目中填写 `abc.github.io`. 注意到这与你预期的博客地址是相同的.

下面的 `Description` 栏目可以留空, 也可以随意填写, 它与你创建的博客的内容没有任何关系. 其他所有条目都应该保持默认, 即 `Owner` 应该是 `ABC`, `visibility` 应该选择 `Public`.

最后, 点击右下角的绿色按钮 Create repository, 你就得到了所需仓库.

## 为仓库配置 Github Pages 选项

在你的仓库页面, 选择上方栏目 (它应该形如 `Code, Issues, ...`) 中的最后一栏 `Settings`.

在左侧栏目选择 Pages. 找到其中 `Build and deployment` 小标题中的 `Source` 条目, 将它从默认的 `Deploy from a branch` 修改为 `Github Actions`, 并等待修改生效.

## 配置博客信息

博客的基础信息保存在仓库中的文件 `_config.yaml` 中. 点击进入文件页面, 右上角有一个铅笔图标, 表示编辑. 一般而言, 需要配置的部分如下:

- `lang`: 改成 `zh-CN`.
- `timezone`: 改成你的时区, 如对于中国大陆用户一般是 `Asia/Shanghai`.
- `title`: 博客标题. 对于本博客是 "通天塔".
- `tagline`: 博客副标题. 对于本博客是 "即使如此, 我也从来没有受骗."
- `description`: 会显示在推送, 转发等场景. 可以留空, 也可以写一句博客简介.
- `url`: 应该写 `https://abc.github.io`.
- `baseurl`: 应该留空.
- `github`: 填写你的 github 个人主页地址. 会显示在左下角. 可以不填.
- `twitter`: 同上. 可以不填.
- `social`: 下面有一些子项目:
  - `name`: 你的名字. 将会显示在所有文章的作者栏 (如果没有专门配置特定文章的 `author` 属性).
  - `email`: 你的邮箱. 会显示在左下角的邮箱位置. 可以不填.
- `theme_mode`: `dark` 或者 `light`. 会决定你的博客是黑色主题还是白色主题.

编辑完成后, 点击右上角的 `Commit`, 将会自动提交这次修改. 弹出窗口中的相关信息可以选择填写, 也可以留空默认.

在一次 commit 完成后, 将会自动运行编译部署等步骤. 一段时间后, 可以访问 `abc.github.io` 检查网站是否已经成功部署. 此时, 网页上应该没有任何文章, 左侧有首页, 分类, 标签, 归档, 关于五个栏目. 这说明部署已经初步成功.

## 上传头像, 图标和插入图片

为了在博客网站上显示图片, 你必须首先将图片上传到仓库中的 `assets` 目录. 具体地, 进入 该目录后, 点击右上角的 `Add file`, 选择 `Upload file`, 上传文件, 等待绿色条消失 (这一步很重要, 如果等待时间不够长, 后续 commit 将会失败, 显示 400). 然后填写 commit 信息 (或留空默认), 并点击下方的 commit 按键.

假设已经上传了名为 `avatar.jpg` 的图片到 `assets` 目录
