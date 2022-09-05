---
title: GIT 基本操作
date: 2022-07-10 17:20:22
description: Git 是一个分布式版本控制系统，Git 版本控制的一些基本操作
categories:
- GIT
tags:
- GIT
---

***

**<font color=dd00dd size=6>FINE</font>**

***
- `Workspace`    工作区
- `add`    `↓`
- `Index / Stage`    暂存区
- `commit`    `↓`
- `Repository`    仓库区（或本地仓库）
- `push`    `↓`
- `Remote`    远程仓库    
- 
- `git init`    初始化仓库
- `git clone <url>`    获得一份已经存在了的 Git 仓库的拷贝
- `git add`     **跟踪**文件、把已跟踪的文件**添加到暂存区**、合并时把有**冲突**的文件标记为已**解决**状态
- `git remote add origin [remote]`    **将本地库与远程库关联**
- `git reset HEAD [filename]`    **取消暂存的文件**
- `git checkout -- [filename]`    **撤销对文件的修改**
- 
- `git commit`    提交，将暂存区内容**添加到本地仓库**
- `git commit -m`    附带提交信息
- `git commit -a`    附带`git add .`
- `git commit --amend`    **修改提交信息（**`:i`**输入模式 **`:wq`**退出并保存修改）**
- 
- `git reset HEAD`    **撤销最近的**`add`
- `git reset HEAD^`    默认参数`--mixed`，撤销最近的`commit`，并且撤销`add`
- `git reset HEAD~1`    对最近的`1`次`commit`执行操作
- `git reset HEAD~n`    对最近的`n`次`commit`执行操作（可以用来**合并**`commit`）
- `git reset --mixed HEAD^`    **撤销最近的`commit`，保留代码，撤销**`add`
- `git reset --soft HEAD^`    **撤销最近的`commit`，保留代码，不撤销**`add`
- `git reset --hard HEAD^`    **撤销最近的`commit`，撤销`add`，代码回到上次修改前的状态**
- 
- `git reflog`    **查看本地的历史提交版本**
- `git reset --hard [id]`    **转到历史提交版本（撤销更改，撤销合并等等）**
- 
- `git push -f`    **强制推送**到远程分支（可能会覆盖别人修改）
- `git push origin master/branch`    **推送到远程仓库**
- 
- `git revert HEAD`    **撤销最近一次提交 **
- `git revert HEAD~0`    **撤销上次的提交**
- `git revert HEAD~1`    **撤销上上次的提交，注意：数字从`0`开始**
- `git revert [id]`    **撤销指定的某次提交**
- `revert`     撤销某次提交并产生一个新提交，**代码回退但版本向前**，可正常`Pull`
- `revert`    若撤销的提交是用`revert`命令产生的，则撤销了撤销
- `revert HEAD~1`     撤销倒数第`2`次的提交，数字是从`0`开始的
- `revert [id]`    撤销的不是最近一次提交，可能会有代码冲突
- 
- `git remove [filename]`    从工作目录移除文件
- `git remove -f [filename]`    移除修改过或已经放到暂存区的文件
- `git remove --cached [filename]`    从暂存区域移除，但仍保留在当前工作目录
- 
- `git status (-s / -short)`    查看哪些文件处于什么状态
   - `Changes to be committed`    已暂存状态
   - `Changes not staged for commit`    已跟踪文件内容发生了变化，但还未放到暂存区
- 
- `git diff`    比较工作目录中当前文件和暂存区域快照之间的差异
- `git diff --staged / --cached`    比对已暂存文件与最后一次提交的文件差异
- **注：**`git diff`    本身只显示尚未暂存的改动
- 
- `git mv file_from file_to`    在`Git`中对文件**改名**
- 
- `git log`     查看提交历史
- `git log  -p / --patch (-num)`    显示每次提交所引入的差异，显示`num`条
- `git log --stat`    每次提交的简略统计信息
- `git log --pretty=[...]`    使用不同于默认格式的方式展示提交历史
   - `oneline`：一行展示，`short` `full`和`fuller`等，format 定制记录显示格式
   - 例如：`git log --pretty=format:"%h - %an, %ar : %s"`
   - 详情： [Git 查看提交历史](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2) 
- 
- `git stash`    **将所有未提交的修改（工作区和暂存区）保存至堆栈中**
- `git stash save []`    等同于`git stash`，但可以加一些注释
- `git stash list`    查看当前`stash`中的内容
- `git stash pop [name]`    将堆栈中**最近保存**或**指定**的内容应用到当前目录（**栈中删除**）
- `git stash apply [name]`    将堆栈中**最近保存**或**指定**的内容应用到当前目录（**栈中不删除**）
- `git stash drop [name]`    从堆栈中移除**最近保存**或**指定**的`stash`
- `git stash clear`    清除堆栈中的**所有**内容
- `git stash show`    查看堆栈中**最近保存**的`stash`和当前目录的差异
- `git stash branch [branch]`    **从最新的**`stash`**创建分支**
- **注：**在`powershell`中指定某一`stash`的名字：`stash@{0}`
- 
- `git branch`    显示当前所有分支
- `git branch -v`    查看每个分支的最后一次提交
- `git branch [branch-name]`    创建分支
- `git branch -d [branch-name]`    删除分支  `-D`强制删除
- `git checkout [branch-name]`    进入分支
- `git checkout -b [branch-name]`    创建加进入分支
- `git checkout -m [old-name] [new-name]`    重命名分支
- `git merge [branch-name]`    合并分支
- `git branch --merged`    查看哪些分支已经合并到当前分支
- `git branch --no-merged    `    查看所有包含未合并工作的分支
- 
- `git branch -a`    **显示远程分支**
- `git push origin -d [branch-name]`    **删除远程分支**
- 
- `git remote`    查看你已经配置的远程仓库服务器
- `git remote -v`    读写远程仓库`Git`保存的简写与其对应的`URL`
- `git remote add [shortname] [url]`    添加一个新的远程`Git`仓库
- `git remote show [remote]`    查看某一个远程仓库的更多信息
- `git remote rename [from] [to]`    修改一个远程仓库的简写名
- `git remote remove [remote]`    删除远程仓库
- `git fetch [remote]`    从远程仓库中获得数据
- `git pull [remote]`    从远程仓库中获得数据
   - **注：**`git fetch`拉取不会自动合并或修改当前工作，`git pull`会
- 
- **设置一些别名：**
- `git config --global alias.aa add .`  `=>`  `git cc == git add .`
- `git config --global alias.cc checkout`  `=>`  `git cc == git checkout`
- `git config --global alias.bb branch`  `=>`  `git bb == git branch`
- `git config --global alias.cm commit`  `=>`  `git cm == git commit`
- `git config --global alias.ss status`  `=>`  `git ss == git status`
- 
- **忽略文件：**
- 创建一个名为`.gitignore`的文件，列出要忽略的文件的模式，例如：
***
```bash
# 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```

***
