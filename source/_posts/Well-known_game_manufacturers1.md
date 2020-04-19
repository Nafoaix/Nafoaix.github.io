---
title: 盘点国外知名游戏厂商
tags:
  - 游戏
  - 杂谈
categories:
  - 游戏
toc: true
cover: "https://cdn.jsdelivr.net/gh/Nafoaix/myblog@master/source/images/cover/git.jpg"
date: 2020-04-17 16:52:59
---

# Git 学习笔记(一)

{% note info %}
作为一个刚接触 Git 不久的小白，该笔记是这两天我在网上看视频和教程自己总结记录下来的，主要为了方便未来自己查阅和帮助像我一样的小白学习 Git。文章中斜体部分为我自己方便记忆的理解，如有错误还请指正，见谅！
{% endnote %}

## Git 的一点介绍

### Git 下载地址：

`https://git-scm.com/`

### Git 是什么

&emsp;百度百科的介绍是：

> Git 是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。

_可以理解为一个小秘书来帮你打理你的项目，让你专注于项目上，而不是被错综复杂的项目版本和文件弄晕。_

### Git 记录的是什么

&emsp;集中式版本控制系统 SVN 记录的是每一次版本变动的内容，Git 则是将项目的每个版本独立保存。Git 通过维护 **工作区域** **暂存区域** **Git 仓库** 来实现版本控制。_你可以理解为三座在本地的仓库。_

### Git 管理的文件的三种状态

- 已修改（modified）
- 已暂存（staged）
- 已提交（committed）

## Git 的使用流程

### Git 初始化

&emsp;和游戏或 APP 一样，下载完之后要在命令行或在 GitBash 中输入以下命令来注册个账号
&emsp; `git config --global user.name "你随便起的ID"`
&emsp; `git config --global user.email "你的邮箱"`
&emsp;使用时首先要在工作的文件夹初始化 Git，你可以在命令行中进入工作文件夹目录也可以以直接在文件夹右键选择 Git Bash Here 然后输入：
&emsp;`git init`
&emsp;_这样就相当于你告诉了 git：“这个文件夹需要你帮我管理。”_
&emsp;这里就成了这个项目的根目录，也就是**工作区域**。初始化完成后该目录下会多了一个`.git`的目录，这个目录就是 Git 用来跟踪管理版本库的，没事不要手动修改这个目录里的文件。（如果没看到也没关系，因为该目录默认是隐藏的。）

### 提交文件

- 当你对项目进行一顿操作后可以使用
  `git add 文件名 ---就是将指定文件添加到暂存区`
  `git add ./-A/--all/* ---可以提交工作区中所有的文件`
  将文件添加到**暂存区**，也就是上面提到三座仓库中的第二座，这个区域区如其名，只是临时保存你的改动。
  _可以理解为小秘书记在随身携带的临时笔记本上。_

- 当你完成了某个改动、变更了某项设置、写完了一个章节就可以使用
  `git commit -m "提交说明(你干了啥)"`
  &emsp;将暂存区文件提交到 **Git 仓库**，也就是上面提到的本地的第三座仓库。就相当于你告诉秘书：_“这些你都帮我打印出来放到 **Git 仓库**存个档。”_
  &emsp;_提交说明就相当于一个档案上贴的小标签比如‘三年二班期末考试成绩’方便自己或别人查阅仓库中存档的档案_
  {% note info no-icon %}
  修改最后一次提交:  
   执行带 --amend 选项的 commit 提交命令 Git 就会更正最近的一次提交
  git commit --amend
  如果不希望修改:q!退出后包保留旧的说明
  {% endnote %}

### 查看状态与提交历史

- `git status`可以查看目前工作区和暂存区的状态。 _就相当于你把秘书叫过来核对下_  
   当你在工作区新增了某些文件后使用此命令秘书就会告诉你:
  `Untracked files: (use "git add <file>..." to include in what will be committed)`
  _“你放了个文件进来怎么都不和我说一声，快用 add 命令添加进仓库里。”_:

&emsp;&emsp;当你在工作区删除或者更改了某些文件后使用此命令秘书就会告诉你:
&emsp;&emsp;`Changes not staged for commit:`
&emsp;&emsp;`(use "git add/rm <file>..." to update what will be committed)`
&emsp;&emsp;`(use "git restore <file>..." to discard changes in working directory)`
&emsp;&emsp;_“你的工作区和我上次过来的时候不一样了，某某文件删掉了，某某文件变更了，你想确认更改或者删掉文件或者让我把这些文件按照 **暂存区** 恢复回来都可以。”_  
&emsp;&emsp;你会注意到我上面说的，使用 `git restore file` 指令 git 小秘书是从**暂存区**来恢复文件的，如果需要从**Git 仓库**来恢复，小秘书需要去仓库查档案，而且你也需要告诉他是恢复成哪次存档的版本，也就是下面关于版本回退提到的`git reset`命令。

- `git log` 可以查看 **Git 仓库** 的历史提交记录。记录中会显示你每次 commit 到 **Git 仓库** 的历史记录，包含每次提交的 id、提交人、日期、提交说明等信息。
  {% note info no-icon %}
  图形化显示快照
  git log --decorate --oneline --graph --all
  --graph 图形化显示
  --all 显示所有快照
  {% endnote %}

### 关于版本回退

<pre>
                   -add->            -commit->
  WorkingDirectory ------ Stage(Index) ------ Repository(HEAD)
                 <-restore-          <-reset-
</pre>

- 回滚指定快照
  `git reset 至少前5位的版本快照号`

- 回滚个别文件
  `git reset 版本快照 文件名/路径`

- `git reset HEAD~` 其中 `HEAD~`表示回滚到上个版本的快照,如果两个`~~`就表示回滚到上上个版本的快照,或写作 `git reset HEAD~2`往后同理， _你可以叫 Git 小秘书去**Git 仓库** 里查任意时候的档案_

_一般情况下为了防止将你的工作区弄乱，你的小秘书一般都是将查到的档案记在她随身携带的笔记本（**暂存区**）上。_
所以一般当你不设置`reset`指令的参数的时，Git 默认使用`--mixed`参数，如`git reset --mixed HEAD~`
`--mixed` 参数表示:

1. 移动 HEAD 的指向，将其指向上一个快照
2. 将 HEAD 移动后指向的快照回滚到暂存区域

与之相对的`reset`指令还有其他一些参数比如：

- `git reset --soft HEAD~`  
  --soft 参数表示 移动 HEAD 的指向将其指向上一个快照，并不会修改暂存区，也就相当于撤回上一次的提交。
- `git reset --hard HEAD~`  
  --hard 参数表示:

1.  移动 HEAD 的指向，将其指向上一个快照

2.  将 HEAD 移动后指向的快照回滚到暂存区域

3.  将暂存区域的文件还原到工作目录 （会覆盖掉工作目录里最新的文件！！！）

{% note info no-icon %}
`reset` 指令不仅可以回滚还可以往前滚，如果上面使用了`reset`指令撤销了上次提交，这时`git log`是看不到前面版本的 ID 的，需要使用`git reflog`查看所有操作的日志。
{% endnote %}

### 关于 diff 命令

diff 顾名思义就是不同，使用`git diff`不加任何参数就是比较**暂存区域**和**工作目录**的差异。

- 比较两个历史快照
  `git diff 快照ID1 快照ID2`
- 比较当前工作目录和 GIt 仓库中的快照
  `git diff 快照ID`
- 比较当前工作目录和 GIt 仓库中的快照
  `git diff 快照ID`
- 比较最新的一份快照和当前工作目录
  `git diff HEAD`
- 比较暂存区域和 Git 仓库快照
  `git diff --cached 快照 ID`

  {% note info no-icon %}
  显示的格式正是 Unix 通用的 diff 格式，查看常用如下键盘操作：

- 移动命令
  J 向下移一行 K 向上移一行
  F 向下移一页 B 向上移一页
  D 向下移半页 U 向上移半页

- 跳转命令
  g 跳到第一行
  G 跳到最后一行
  num g 跳到第 num 行
  num G 跳到倒数第 num 行

- 搜索命令
  / +关键词 从上往下搜索关键词
  ? +关键词 从下往上搜索关键词
  n 跳转到下一个搜索结果

- h 帮助文档

- q 退出
  {% endnote %}

### 其他文件操作

- 删除文件
  `git rm 文件名`此命令会删除工作目录和暂存区域的文件，也就是取消跟踪，在下次提交时不纳入版本管理，不会删除在仓库快照里的文件。
  > 如果你工作区和暂存区有两个不同的文件 git 会不知道你删除哪个
  > 使用-f 指令强制删除工作区和暂存区文件
  > 使用--cached 文件名 只删除暂存区的文件
- 重命名文件
  `git mv 旧文件名 新文件名`
