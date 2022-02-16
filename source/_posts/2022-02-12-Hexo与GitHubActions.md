---
layout: post
title: Hexo 与 GitHub Actions
date: 2022-02-12
categories: 计算机基础
tag: Git
---
本地配置 Hexo 博客环境实在是一件比较伤脑筋的事情，之前有尝试过使用 GitHub Actions，但是由于配置过程中不知道配置的内容，复制粘贴配置文件不可行就放弃了。为了让自己写一点内容进行沉淀，于是还是尝试使用 GitHub Actions 来生成静态网页内容。

## 问题一: Permission denied
![](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/picgo/GithubActions_q1.png)
如上图所示，我知道肯定是公钥和私钥的问题。复制的时候没出错应该不会出现这个问题，最后发现是把私钥放错了位置。正确的位置应该是
![](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/picgo/GitHubActions_privatekey.png)

## 问题 二：Workflow不能正常运行
![](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/picgo/GitHubActions_workflow.png)
我原意是使用源文件中的主题文件，所以删掉了上图中红框部分，导致 `index.html` 不能正常渲染。解决办法是 `fork` 我想使用的主题。更改红框中的内容即可。 

## Reference
[1] [使用 GitHub Actions 自动部署 Hexo 博客 - PRIN BLOG (printempw.github.io)](https://printempw.github.io/use-github-actions-to-deploy-hexo-blog/#/)
[2] [hexo 生成的 html 文件为空的问题 - Alan Lee](https://alanlee.fun/2021/02/28/hexo-empty-html/#/)
[3] [利用 Github Actions 自动部署 Hexo 博客 | Sanonz](https://sanonz.github.io/2020/deploy-a-hexo-blog-from-github-actions/#/)