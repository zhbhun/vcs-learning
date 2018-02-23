- https://git-scm.com/

# 工具
- [GUI Clients](http://git-scm.com/downloads/guis)

# 命令
- [这些GIT经验够你用一年了](http://www.techug.com/post/some-git-tips.html)
- [Git查看、删除、重命名远程分支和tag](http://zengrong.net/post/1746.htm)

## 本地仓库
- 概念

    - 工作区: 项目目录
    - 版本库: `.git`
    - 暂存区: `git add`
    - 管理修改: `git commit` 只提交暂存区的内容

- 创建

    - `git init`
    - `git add readme.txt`
    - `git commit -m "wrote a readme file"`

- 查看状态: `git status`
- 查看差异: `git diff readme.txt`
- 添加暂存区: `git add readme.txt`
- 条件分支: `git commit readme.txt`
- 版本历史: `git log [--pretty=oneline]`, `git reflog`
- 版本回退: `git reset --hard HEAD^`, `git reset --hard 提交ID`

    - HEAD 当前版本
    - HEAD^ 上一个版本
    - HEAD^^ 上上一个版本

- 取消修改: `git checkout -- readme.txt`, `git reset HEAD readme.txt`

    - 还没添加到暂存区: 回到和版本库一致
    - 已添加到暂存区: 回到添加到暂存区的状态

- 删除文件: `git rm readme.txt` + `git commit -m "delete readme.txt"`
- 恢复删除: `git checkout -- readme.txt`

## 远程仓库
- SSH 配置

    - 创建 SSH Key: `ssh-keygen -t rsa -C "youremail@example.com"` --- 生成 id_rsa 和 id_rsa.pub
    - 配置 Github: 添加 SSH Key
    - 检查是否成功: ...

- 添加远程库

    - `git remote add origin git@github.com:account/repository.git`
    - `git push -u origin master`, `git push origin master`

- 克隆远程库: `git clone git@github.com:account/repository.git`
- 查看远程仓库: `git remote`, `git remote -v`

## 分支
- 创建分支: `git checkout -b dev`

    - `git branch dev`
    - `git checkout dev`

- 查看分支: `git branch`
- 切换分支: `git checkout master`
- 合并分支:

    - `git merge dev`
    - `git merge --no-ff -m "merge with no-ff" dev`
    - `git merge dev --squash`

- 删除分支

    - `git branch -d dev`
    - `git branch -D dev`

- 解决冲突: ...
- 分支管理策略: master, dev, issue-xxx, feture-xxx...
- Bug 分支

    - `git stash`
    - `git checkout master`
    - `git add readme.txt `
    - `git commit -m "fix bug 101"`
    - `git checkout master`
    - `git merge --no-ff -m "merged bug fix 101" issue-101`
    - `git branch -d issue-101`
    - `git checkout dev`
    - `git status`
    - `git stash list`
    - `git stash apply`, `git stash drop` --- `git stash pop`

- Feture 分支
- 多人协作

    - 抓取分支:

        - `git clone git@github.com:michaelliao/learngit.git`
        - `git branch` --- 只有 master 分支
        - `git checkout -b dev origin/dev` --- 抓取 dev 分支
        - `git branch --set-upstream dev origin/dev`
        - `git pull` --- 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并

    - 推送分支: `git push origin master`, `git push origin dev` --- 选择必要的分支进行推送

## 标签
TODo

## 子模块
- 作用

    - 分割代码到不同的库
    - 添加子模块到多个库

- 用法

    - 初始化：克隆仓库后使用

        - `git submodule update --init`：更新子模块，没有初始化的会先初始化；
        - `git submodule update --init --recursive`：子模块内有子模块时，可使用该命令递归初始化和更新子模块

    - 添加子模块

        1. `git submodule add git@github.com:url_to/awesome_submodule.git path_to_awesome_submodule`：添加子模块时，并没有将子模块代码添加到主仓库，只是添加了子模块的信息 —— 子模块信息在 `.gitmodules` 文件中；
        2. `git submodule init`：获取子模块代码；
        3. `git add .gitmodules`，`git mommit -m "add submodule xxx"` 和 `git push`：提交子模块至远程仓库；

    - 修改子模块

        1. 修改并提交子模块代码：子模块是一个单独的库，可以像正常的库一样修改子模块库 —— 在子模块目录执行仓库操作命令；
        2. 执行第一步后，在主仓库执行 `git status` 可以发现子模块在列表 `Changes not staged for commit` 中 —— 主模块指向的提交记录和子模块已经不一致了；
        3. `git add`, `git commit -m "..."` 和 `git push`：使主仓库指向新的子模块提交记录；

    - 更新子模块

        1. `git pull`：只会更新当前主仓库指向的子模块提交记录，而不会去更新子模块的代码；
        2. `git submodule update`：更新子模块代码 —— 如果不执行的化，会发现 `git status` 显示子模块是处于修改状态（指向了不一致的提交记录）；

    - 推送子模块：`git push --recurse-submodules=on-demand`


- 误区

    - 主仓库通过 `.gitmodules` 来维护包含的子模块，并且主仓库只是包含了子模块提交记录的指针（主仓库引用的是子模块哪一个版本）。
    - 在维护子模块的时候，要将子模块当做一个单独的仓库来处理。
    - 在维护主仓库的时候，要将子模块当成一个包含子模块指针的文件来处理。
        
- 参考文献

    - [Git Submodules basic explanation](https://gist.github.com/gitaarik/8735255)

# 钩子
- https://github.com/typicode/husky

    - https://github.com/okonet/lint-staged
    - https://github.com/azz/pretty-quick
    - https://github.com/pre-commit/pre-commit
    - https://github.com/nrwl/precise-commits

- https://github.com/ghooks-org/ghooks

# 问题
- [Oh shit, git!](http://ohshitgit.com/)
- [使用 "5W1H" 写出高可读的 Git Commit Message](https://zhuanlan.zhihu.com/p/26791124)
- [How do I commit case-sensitive only filename changes in Git?](https://stackoverflow.com/questions/17683458/how-do-i-commit-case-sensitive-only-filename-changes-in-git)
- [CLI Tips #3：如何知道仓库的代码行数？](https://zhuanlan.zhihu.com/p/26684427)

## AutoCRLF
- Window 换行符是 CR + LF，OS X 和 Linux 换行符使用 LF；
- Git 换行符转换功能 autocrlf

    - false：提交检出均不转换；
    - true：提交时转换为LF，检出时转换为 CRLF；
    - input：提交时转换为LF，检出时不转换；

- 建议配置：Window 下将 core.autocrlf 设为 true，在 OS X 和 Linux 下设为 input
- 参考文献

    - [Git自动转换换行符(autocrlf)带来的问题](http://www.luckyonecn.com/blog/git-auto-crlf-problem/)

## SafeCRLF
- true：拒绝提交包含混合换行符的文件
- false：允许提交包含混合换行符的文件
- warn：提交包含混合换行符的文件时给出警告

# 博文
- [Learn Git Branching](http://learngitbranching.js.org/)
- [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
    - http://www.juvenxu.com/2010/11/28/a-successful-git-branching-model/
- [Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
- [Git 使用规范流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)
- [Git分支管理策略](http://www.ruanyifeng.com/blog/2012/07/git.html)
- [git - 简易指南](http://rogerdudler.github.io/git-guide/index.zh.html)
- [猴子都能懂的GIT入门](http://backlogtool.com/git-guide/cn/)
- [Git 参考手册](http://gitref.justjavac.com/)
- [Pro Git](http://git-scm.com/book/zh)
- [Pro Git 中文版](https://www.gitbook.com/book/0532/progit/details)
- [Git Magic](http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/)
- [GotGitHub](http://www.worldhello.net/gotgithub/index.html)
- [Git Community Book 中文版](http://gitbook.liuhui998.com/index.html)
- [沉浸式学 Git](http://igit.linuxtoy.org/)
- [Git-Cheat-Sheet ](https://github.com/flyhigher139/Git-Cheat-Sheet)
- [GitHub秘籍](http://snowdream86.gitbooks.io/github-cheat-sheet/content/zh/index.html)
- [Github帮助文档](https://github.com/waylau/github-help)
- [git-flow 备忘清单](http://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)
- [GITLAB: Self Hosted Git Management Application](http://www.gitlabhq.com/)
- [git-flow 备忘清单](http://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)
- [nvie/gitflow · GitHub](https://github.com/nvie/gitflow)
- [图解Git](http://my.oschina.net/xdev/blog/114383)
- [Git Magic - 前言](http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/index.html)
- [goops：给仓库添加最佳 gitignore 规则的命令行工具](https://github.com/captainsafia/goops)
- [git-recipes](https://github.com/geeeeeeeeek/git-recipes/wiki)

# 书籍
- 《Pro Git》

    - http://git-scm.com/book/en/v2
    - http://git-scm.com/book/zh/v2
    
- 《Git版本控制管理》
- 《Version Control with Git》
- 《Git权威指南》
- 《Git Magic》
- 《Git Internals》
- 《Pragmatic Guide to Git》

# 规范
- [Angular Git Commit Guidelines](https://github.com/angular/angular.js/blob/v1.4.8/CONTRIBUTING.md#commit)
- [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)

    https://www.oschina.net/translate/a-successful-git-branching-model

- [Merging vs. Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [闲谈 git merge 与 git rebase 的区别](https://segmentfault.com/a/1190000005013964)
- [Git分支管理策略](http://www.codeceo.com/article/git-plan.html)
