---
title: Git 分支模型实践
date: 2016-04-05 08:48:24
updated:
tags: Git
---

有了一套成熟的[分支模型](/2016/04/03/git-branch/)以及配套的[权限控制](/2016/04/04/git-permissions/)之后，接下来我们以一个例子来演示如何实践这套流程。

# 分支模型实践

## 创建特性分支

首先，开发人员（Developer）从 `dev` 分支中创建出特性分支：

```bash
$ git checkout -b feature-test

do something and commit...

$ git push origin feature-test
```

### 定期合并

由于特性分支可能会跨版本开发，因此需要定期维护：主要的工作就是定期将 `dev` 分支合并（`merge`）进来，保持同步。

### 决断代码

特性分支开发完成之后，如果想要筛选出将要被合并的提交有哪些，可以参考[这里](/2015/08/04/git-log/#筛选提交历史)。

## 合并特性分支

开发完成后，开发人员（Developer）需要申请将特性分支合并回预发布分支，以便发布新版本。具体做法就是[发起一次合并请求](/2016/04/04/git-permissions/#发起合并请求)即可，项目管理员（Master）在代码审查通过后就会接受该次合并请求。

## 标记新版本

版本发布之后，Master 应该标记该新版本，以便后续回顾：

```bash
$ git tag v1.0 -m "XX 项目 v1.0 版本"
$ git push origin v1.0
```

注意，在默认情况下，`git push` 并不会把标签（tag）推送到远端仓库上，只有通过显式命令才能分享标签到远端仓库。其命令格式如同推送分支，运行 `git push origin [tagname]` 即可。如果要一次推送所有本地新增的标签上去，可以使用 `--tags` 选项。

## 删除特性分支

最后是一些清理工作，Master 需要删除已开发完成的分支，避免分支越来越多导致不好管理：

```bash
$ git branch -d feature-test
$ git push --delete origin feature-test
```

# 总结

## 代码提交指南

* 请不要在更新中提交多余的白字符（whitespace）。Git 有种检查此类问题的方法，在提交之前，先运行 `git diff --check` ，会把可能的多余白字符修正列出来。
* 请将每次提交限定于完成一次逻辑功能。并且可能的话，适当地分解为多次小更新，以便每次小型提交都更易于理解。
* 最后需要谨记的是提交说明的撰写。可以理解为第一行的简要描述将用作邮件标题，其余部分作为邮件正文。

## 分支管理指南

* 主分支 `master` 、预发布分支 `release-*` 一般不提交代码，只合并代码。
* 开发人员只需发起合并请求。合并特性代码到预发布分支的操作，由项目管理员负责。
* 各特性分支要定期将 `dev` 分支合并进来，并在发起合并请求前将预发布分支也合并进来，避免后续处理合并请求时产生冲突，以减轻项目管理员的工作负担。
* 发版之后，项目管理员要记得打 tag 。

# 参考

* 《[分布式 Git](https://git-scm.com/book/zh/v1/%E5%88%86%E5%B8%83%E5%BC%8F-Git)》