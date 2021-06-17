# Git

- https://git-scm.com/
- https://learngitbranching.js.org/
- https://www.git-tower.com/learn/

## 客户端

- [GUI Clients](http://git-scm.com/downloads/guis)

## 教程

- [git-recipes](https://github.com/geeeeeeeeek/git-recipes)
- [这些GIT经验够你用一年了](http://www.techug.com/post/some-git-tips.html)
- [Git查看、删除、重命名远程分支和tag](http://zengrong.net/post/1746.htm)

## 命令

### 回滚

- 未跟踪文件

    - `git clean -fd`：强制删除所有未跟踪的文件和文件夹
    - `git clean -fdx`：包含 gitignore 文件

- 版本回退: `git reset --hard HEAD^`, `git reset --hard 提交ID`

    - HEAD 当前版本
    - HEAD^ 上一个版本
    - HEAD^^ 上上一个版本

- 取消修改: `git checkout -- readme.txt`, `git reset HEAD readme.txt`

    - 还没添加到暂存区: 回到和版本库一致
    - 已添加到暂存区: 回到添加到暂存区的状态

- 恢复删除: `git checkout -- readme.txt`


参考文献

- [代码回滚：Reset、Checkout、Revert 的选择](https://github.com/geeeeeeeeek/git-recipes/wiki/5.2-%E4%BB%A3%E7%A0%81%E5%9B%9E%E6%BB%9A%EF%BC%9AReset%E3%80%81Checkout%E3%80%81Revert-%E7%9A%84%E9%80%89%E6%8B%A9)

### 日志

- 可视化查看当前分支

    `git log --graph`

- 可视化查看所有提交、标签和分支

    `git log --oneline --decorate --all --graph`

- 查找日志

    `git log -i --grep search_string`

- 查看指定文件或目录的变更历史

    `git log -p path/to/file_or_directory`

- 查看指定范围的日志

    `git log <hash_or_tag>..HEAD`

### 仓库

#### 本地仓库

- 概念

    - 工作区: 项目目录
    - 版本库: `.git`
    - 暂存区: `git add`
    - 管理修改: `git commit` 只提交暂存区的内容

- 创建

    - `git init`
    - `git add readme.txt`

        > `git rm --cached <filePath>` / `git reset -- <filePath>` 取消暂存

    - `git commit -m "wrote a readme file"`

- 查看状态: `git status`
- 查看差异: `git diff readme.txt`
- 添加暂存区: `git add readme.txt`
- 条件分支: `git commit readme.txt`
- 版本历史: `git log [--pretty=oneline]`, `git reflog`
- 删除文件: `git rm readme.txt` + `git commit -m "delete readme.txt"`

#### 远程仓库

- SSH 配置

    - 创建 SSH Key: `ssh-keygen -t rsa -C "youremail@example.com"` --- 生成 id_rsa 和 id_rsa.pub
    - 配置 Github: 添加 SSH Key
    - 检查是否成功: ...

- 添加远程库

    - `git remote add origin git@github.com:account/repository.git`
    - `git push -u origin master`, `git push origin master`

- 修改远程仓库地址

    `git remote set-url origin <newurl> [<oldurl>]`

- 克隆远程库: `git clone git@github.com:account/repository.git`
- 查看远程仓库: `git remote`, `git remote -v`
- 拉取远程仓库分支：`git fetch origin xxx`
- 创建本地分支：`git checkout -b xxx(本地分支名称) origin/xxx(远程分支名称)`

### 分支

- 创建分支: `git checkout -b dev`

    - `git branch dev`
    - `git checkout dev`

- 查看分支: `git branch`
- 查看差异：

    - `git diff branch1 branch2 --stat`
    - `git diff branch1 branch2 文件名`
    - `git diff branch1 branch2`

- 切换分支: `git checkout master`
- 合并分支:

    - `git merge dev`
    - `git merge --no-ff -m "merge with no-ff" dev`
    - `git merge dev --squash`
    - `git merge --ff origin/develop`

- 删除分支

    - 删除本地分支：`git branch -d dev`
    - 强制删除本地分支：`git branch -D dev`
    - 删除远程分支：`git push origin :branch_name` 

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

### 标签

- 删除标签
    - `git tag -d tag_name`
    - `git push origin :refs/tags/tag_name`
- ...

### 子模块

- 作用

    - 分割代码到不同的库
    - 添加子模块到多个库

- 用法

    - 克隆：`git clone --recurse-submodules <url>`
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

### 钩子

- https://github.com/typicode/husky

    - https://github.com/okonet/lint-staged
    - https://github.com/azz/pretty-quick
    - https://github.com/pre-commit/pre-commit
    - https://github.com/nrwl/precise-commits

- https://github.com/ghooks-org/ghooks

### 备份

- `git bundle`
- `git archive`


    ```shell
    function bugit() {
      pwd
      cd $1
      git clean -df && git reset && git archive --format=tar HEAD -o ./backup.tar && tar -rf ./backup.tar -C ./ .git && gzip < ./backup.tar >     backup.tar.gz && rm -f backup.tar && mv ./backup.tar.gz $2/$1.tar.gz
      cd ..
    }
    
    # butgits <target_dir> 
    function butgits() {
      for file in ./*
      do
        if test -d $file
        then
          if [ ! -f "$1/${file:2}.tar.gz" ]; then
            echo ">>${file:2}"
            bugit ${file:2} $1
          fi
        fi
      done
    }
    ```

## 配置

### Ignore

- https://github.com/github/gitignore
- [使.gitignore忽略除了几个文件之外的所有内容](https://codeday.me/bug/20170317/773.html)

## 问题

- [Oh shit, git!](http://ohshitgit.com/)
- [使用 "5W1H" 写出高可读的 Git Commit Message](https://zhuanlan.zhihu.com/p/26791124)
- [How do I commit case-sensitive only filename changes in Git?](https://stackoverflow.com/questions/17683458/how-do-i-commit-case-sensitive-only-filename-changes-in-git)
- [CLI Tips #3：如何知道仓库的代码行数？](https://zhuanlan.zhihu.com/p/26684427)

### AutoCRLF

- Window 换行符是 CR + LF，OS X 和 Linux 换行符使用 LF；
- Git 换行符转换功能 autocrlf

    - false：提交检出均不转换；
    - true：提交时转换为LF，检出时转换为 CRLF；
    - input：提交时转换为LF，检出时不转换；

- 建议配置：Window 下将 core.autocrlf 设为 true，在 OS X 和 Linux 下设为 input
- 参考文献

    - [Git自动转换换行符(autocrlf)带来的问题](http://www.luckyonecn.com/blog/git-auto-crlf-problem/)

### SafeCRLF

- true：拒绝提交包含混合换行符的文件
- false：允许提交包含混合换行符的文件
- warn：提交包含混合换行符的文件时给出警告

### "old mode 100755 new mode 100644"

- [How do I remove files saying “old mode 100755 new mode 100644” from unstaged changes in Git?](https://stackoverflow.com/questions/1257592/how-do-i-remove-files-saying-old-mode-100755-new-mode-100644-from-unstaged-cha)

### rebase vs merge 

- [git merge --no-ff是什么意思](https://segmentfault.com/q/1010000002477106)
- [Git rebase学习笔记](https://www.jianshu.com/p/9cfd206635c0)
- [Git commits历史是如何做到如此清爽的？](https://www.zhihu.com/question/61283395/answer/186122300)

### `git@github.com: Permission denied (publickey)`

- [git@github.com: Permission denied (publickey)](https://blog.csdn.net/qq_32786873/article/details/80947195)

### 自定义 SSH Key

- [How to specify the private SSH-key to use when executing shell command on Git?](https://stackoverflow.com/questions/4565700/how-to-specify-the-private-ssh-key-to-use-when-executing-shell-command-on-git)
- [git 指定要提交的ssh key](https://www.cnblogs.com/chenkeyu/p/10440798.html)

## 博文

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

## 书籍

- [《Pro Git》](https://github.com/progit/progit)

    - http://git-scm.com/book/en/v2
    - http://git-scm.com/book/zh/v2
    
- 《Git版本控制管理》
- 《Version Control with Git》
- 《Git权威指南》
- 《Git Magic》
- 《Git Internals》
- 《Pragmatic Guide to Git》

## 规范

### 分支管理
- [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)

    - https://github.com/nvie/gitflow/
    - https://jeffkreeftmeijer.com/git-flow/
    - https://www.oschina.net/translate/a-successful-git-branching-model

- http://scottchacon.com/2011/08/31/github-flow.html
- [Git分支管理策略](http://www.ruanyifeng.com/blog/2012/07/git.html)
- [Angular Git Commit Guidelines](https://github.com/angular/angular.js/blob/v1.4.8/CONTRIBUTING.md#commit)
- [Merging vs. Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [闲谈 git merge 与 git rebase 的区别](https://segmentfault.com/a/1190000005013964)
- [Git分支管理策略](http://www.codeceo.com/article/git-plan.html)
- [Learn Version Control with Git](https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/git-flow)

### 提交信息

- [Angular.js Git Commit Guidelines](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)
- [cz-cli](https://github.com/commitizen/cz-cli)
- [gitmoji-cli](https://github.com/carloscuesta/gitmoji-cli)
- [更优雅的使用 Git](https://segmentfault.com/a/1190000014776954)

## 工具

- 流程图绘制工具

    - http://beta.gitflowchart.com/
    - http://gitgraphjs.com/

## 应用

- [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog)
