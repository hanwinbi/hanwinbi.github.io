---
title: 📒【Git】git基础教程
date: 2021-01-22
categories: 计算机基础
comments: true
tag: Git
toc: true
---

git是进行版本管理的利器，掌握基本的命令就可以很快的上手，养成良好的代码管理可以更好的和他人协作。（虽然Linus说它写git和Linux的原因是因为他自己要用，而不是
为了更好的和他人协作，笑
对于不常用但是又会经常忘记的我来说，这是一份文档

- ssh配置
    - 在主机查看`~/.ssh`下有没有`id_rsa`和`id_rsa.pub`两个文件，前者是私钥，后者是公钥。如果没有，运行`ssh-keygen -t rsa -C "example@mail.com"`
    - 打开GitHub中打开设置页面，找到`SSH and GPG keys,`添加`id_rsa.pub`中的文本即可
- 三分区
    - working dir
    - stage
    - history
- 基础命令
    - **add**：添加到暂存，-A添加所有文件
    - **clone**：克隆仓库
    - **push**：推送到远程仓库
    - **pull**：拉取远程仓库的内容
    - **commit**：提交到远程仓库
- 版本管理
    - `git log --pretty=oneline` 查看提交历史
    - 撤销变更
        - `git reset --hard <commit_id>`  回滚到某个版本
        - 恢复到原始版本，`git reflog`查看历史提交的commit id，`git reset --hard <commit_id>`
        - `git revert HEAD`回退到当前版本的前一个版本，但会多出一个提交记录
- 分支管理
    - 切换分支`git checkout branchName`
    - 合并分支
        - merge
            - 在master上可以合并分支，分支信息会消失
            - 在分支上合并master会保留分支信息
        - rebase
            - 相当于副本复制，在分支上`git rebase master`即可合并到master。这样操作的好处是使开发过程相对线性化
- 标签管理
    - 为什么需要打标签？
        - 用来记录比较重要的版本更新，只可以查看，不能像branch一样更改代码，适合我这种线性的开发方式
    - 增
        - `git tag <tag_name>` 添加tag的时候，会打在当前head上
        - `git log --pretty=oneline --abbrev-commit` 这个用来找到历史的`commit id`，然后使用`git tag <tagname> commit_id`，这样就能对历史提交打tag了
        - `-a` 用来指定tag名，`-m` 用来指定说明文字
        - `git push origin --tags`推送全部未推送过的本地标签
    - 删
        - `git tag -d <tagname>` 使用后会删除本地的tag，但是如果推送到远程仓库，必须使用`git push origin --delete <tagname>`
        - `git branch -D <branch_name>` 
    - 改
    - 查
        - `git tag` 显示所有的标签
        - `git show <tagname>` 可以查看标签信息
- 分离HEAD
    - `git checkout hash_history`
    - 相对引用
        - 使用`git log`查看提交记录的哈希值
        - 使用`^`向上移动1个提交记录,`git checkout HEAD^`
        - 使用~<num>向上移动多个提交记录，如`~4`
        - `master^`相当于master的父节点
        - 强制main指向之前的三个节点`git branch -f main HEAD^3`
- 远程仓库
    - `git fetch`获取远程分支的内容，本地的指针并不会改变
    - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FGraduateDesign%2FyUJ8eFpTmS.png?alt=media&token=2f5c7a38-15c9-43a1-9297-366a9904f4c4)
    - 此处是记录fork别人文件并魔改，同时保持主分支和原作者同步的记录
        - 先在GitHub上面fork，以下的操作都是拉取到本地之后的
        - `git remote -v`检查远程仓库的路径，如果没有`upstream`的话那就没和原作者的仓库联动起来，以后不方便拉取更新了`git remote add upstream url_xxx.git`，然后运行上一条命令就可以看到`upstream`设置好了
        - 然后将本地仓库的内容提交到远程仓库，`git status`先检查本地有没有没有添加的文件
            - `git add -A` or `git add filename`
            - `git commit -m "message"`
            - `git push origin master`
            - `git status`
        - merge的关键命令
            - `git fetch upstream`用来抓取原作者的更新
            - `git checkout master`切换到主分支
            - `git merge upstream/master`合并远程的master分支
            - `git push`把本地仓库想GitHub仓库提交推送修改
    - 本地仓库和远程仓库关联
        - `git init`初始化本地仓库
        - 创建远程仓库
        - `git remote add origin git@github.com:github_username/repo.git`
    - 克隆远程私密仓库
        - `git clone https://github-username:github-password@github.com/yuedu/website.git`
        - git clone https://github_username:Password@github.com/GitHub_username/repo_name.git
- 整理提交 Advanced
    - `git cherry-pick <commit-id>` 将已经存在任意位置的提交，复制到当前HEAD下
    - `git rebase -i <commit-id>`在不知道提交哈希值的时候，使用交互式`rebase`调整提交
    - `git describe`描述与距离最近的tag的信息，`<tag_name>_<distance>_<hash>`
- 参考链接
    - [Fork并更新自己代码](https://github.com/selfteaching/the-craft-of-selfteaching/issues/67)
    - [廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600)
    - [Visualize Git](https://dev.to/lydiahallie/cs-visualized-useful-git-commands-37p1)
