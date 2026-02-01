---
title: git使用实践
date: 2023-09-04 22:14:32
description: git的使用教程，关于git怎么使用，工作流程及其原理
tags:
  - git
  - 使用教程
categories:
cover: https://tse1-mm.cn.bing.net/th/id/OIP-C.R1Bi3fNDyYjLsXdg9OqoIAHaEo?w=315&h=196&c=7&r=0&o=5&dpr=1.3&pid=1.7
top_img: https://tse1-mm.cn.bing.net/th/id/OIP-C.R1Bi3fNDyYjLsXdg9OqoIAHaEo?w=315&h=196&c=7&r=0&o=5&dpr=1.3&pid=1.7
---

# Git

### git工作流程

以下是git的工作流程

![git工作流程](https://www.runoob.com/wp-content/uploads/2015/02/git-process.png)



工作大致就是三个操作：**修改、提交、推送**

### git工作区

- **工作区**：就是平常的工作目录，项目文件夹
- **暂存区(stage/index)**：存放在`.git`目录下面的index文件夹下面 有时也叫索引区
- **版本库**：工作区一个隐藏目录`.git`，这个文件夹就是git的版本库

![git工作区和版本库中间的关系](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)



- 图中左侧为工作区，右侧为版本库。在版本库中标记为 “index” 的区域是暂存区（stage/index），标记为 “master” 的是 master 分支所代表的目录树。
- 图中我们可以看出此时 “HEAD” 实际是指向 master 分支的一个”游标”。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。
- 图中的 objects 标识的区域为 Git 的对象库，实际位于 “.git/objects” 目录下，里面包含了创建的各种对象及内容。
- 当对工作区修改（或新增）的文件执行 `git add` 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
- 当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
- 当执行`git reset HEAD`命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
- 当执行 `git rm --cached <file>` 命令时，会直接从暂存区删除文件，工作区则不做出改变。
- 当执行 `git checkout .` 或者 `git checkout -- <file>` 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区中的改动。
- 当执行 `git checkout HEAD .` 或者 `git checkout HEAD <file>`命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

### git创建仓库

当前文件夹创建仓库

```BASH
git init # 指定本文件夹为git仓库
```

指定文件夹创建仓库

```BASH
git init directory # 指定该文件夹为git仓库
```

克隆仓库

```BASH
git clone <repo> <directory> # repo：克隆仓库链接 directory：目录
```

配置

```BASH
git config --list # 显示当前配置
git config -e # 针对当前仓库
git config -e --global   # 针对系统上所有仓库
```

设置提交代码的用户

```BASH
git config --global user.name "runoob"
git config --global user.email test@runoob.com
```

### git基本操作

Git 的工作就是创建和保存你项目的快照及与之后的快照进行对比。

> Git 常用的是以下 6 个命令：**git clone**、**git push**、**git add** 、**git commit**、**git checkout**、**git pull**

![git操作演示](https://blog.znxs.vip/znxs/znxsgit-command.jpg)

**说明：**

- workspace：工作区
- staging area：暂存区/缓存区
- local repository：版本库或本地仓库
- remote repository：远程仓库

##### 基本操作

```BASH
git init # 参考初始化
git add . # 添加本地所有文件到暂存区
git commit # 提交 将暂存区文件提交到仓库中

# 如果想要跳过add步骤 可以直接使用-a 提交 m 表示提交日志
git commit -am
```

##### 创建仓库命令

```BASH
git init	# 初始化仓库
git clone	# 拷贝一份远程仓库，也就是下载一个项目。
```

##### 提交与修改

```BASH
git add	# 添加文件到暂存区
git status	# 查看仓库当前的状态，显示有变更的文件。
git diff	# 比较文件的不同，即暂存区和工作区的差异。
git commit	# 提交暂存区到本地仓库。
git reset	# 回退版本。
git rm	# 将文件从暂存区和工作区中删除。
git mv	# 移动或重命名工作区文件。
```

##### 提交日志

```BASH
git log	  # 查看历史提交记录
git blame <file> # 以列表形式查看指定文件的历史修改记录
```

##### 远程操作

```BASH
git remote	# 远程仓库操作
git fetch	# 从远程获取代码库
git pull	# 下载远程代码并合并
git push	# 上传远程代码并合并
```

### git分支管理

创建分支

```BASH
git branch (branchname)
或
git switch -c (branchname) # 创建分支并且切换到该分支
```

切换分支

```BASH
git checked (branchname)
git switch (brachname)
```

> 当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录

合并分支

```BASH
git merge 
```

你可以多次合并到统一分支， 也可以选择在合并之后直接删除被并入的分支

##### 实例

```BASH
$ mkdir gitdemo
$ cd gitdemo/
$ git init
Initialized empty Git repository...
$ touch README
$ git add README
$ git commit -m '第一次版本提交'
[master (root-commit) 3b58100] 第一次版本提交
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README
```

##### 列出分支

列出分支基本命令

```BASH
git branch
```

没有参数的时候，默认是所在的本地分支

```BASH
$ git branch
* master
```

当执行`git init`的时候，git会默认创建master分支 如果需要手动创建一个分支，执行``git branch (branchname) `

```BASH
$ git branch testing
$ git branch
* master
  testing
```

新建一个分支之后，如果后面又有了提交的更新，然后有切换到了testing分支，git将还原你的工作目录到创建分支的样子

##### 实例2

```BASH
$ ls
README
$ echo 'runoob.com' > test.txt
$ git add .
$ git commit -m 'add test.txt'
[master 3e92c19] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
$ ls
README        test.txt
$ git checkout testing
Switched to branch 'testing'
$ ls
README
```

当在**master**添加一个文件到暂存区之后，然后提交到本地仓库之后

再切换到**testin**分支的时候 发现添加的`test.txt`文件没了，但是切换到**master**分支的时候，它又重新出现了

```BASH
$ git checkout master
Switched to branch 'master'
$ ls
README        test.txt
```

我们也可以使用 `git checkout -b (branchname)/git switch -c (branchname)` 命令来创建新分支并立即切换到该分支下，从而在该分支中操作。

```BASH
$ git checkout -b newtest
Switched to a new branch 'newtest'
$ ls
README.md  test.txt
$ git rm test.txt 
rm 'test.txt'
$ ls
README
$ touch runoob.php
$ git add .
$ git commit -am 'removed test.txt、add runoob.php'
[newtest c1501a2] removed test.txt、add runoob.php
 2 files changed, 1 deletion(-)
 create mode 100644 runoob.php
 delete mode 100644 test.txt
$ ls
README        runoob.php
$ git checkout master
Switched to branch 'master'
$ ls
README        test.txt
```

当创建了一个分支，在该分支上移除了一些文件 test.txt，并添加了 znxs.php 文件，然后切换回我们的主分支，删除的 test.txt 文件又回来了，且新增加的 znxs.php 不存在主分支中。

使用分支将工作切分开来，从而让我们能够在不同开发环境中做事，并来回切换。

##### 删除分支

删除分支命令

```BASH
git branch -d (branchname)
```

如果要删除testing分支

```BASH
$ git branch
* master
  testing
  newTest
$ git branch -d testing
Deleted branch testing (was 85fc7e7).
$ git branch
* master
  newTest
```

##### 分支合并

一旦某分支有了独立内容，你终究会希望将它合并回到你的主分支。 你可以使用以下命令将任何分支合并到当前分支中去：

```BASH
git merge
$ git branch
* master
  newTest
$ ls
README        test.txt
$ git merge newTest  # 注意 在哪个分支下面使用 就是将需要合并的分支合并到该分支下 例如：master 分支下执行git merge newTest 就是将newTest 分支合并到master test将被删除
Updating 3e92c19..c1501a2
Fast-forward
 znxs.php | 0
 test.txt   | 1 -
 2 files changed, 1 deletion(-)
 create mode 100644 znxs.php
 delete mode 100644 test.txt
$ ls
README        znxs.php
```

将 newtest 分支合并到主分支去，test.txt 文件被删除

##### 合并冲突

合并并不仅仅是简单的文件添加、移除的操作，Git 也会合并修改。

```BASH
$ git branch
* master
  newTest
$ cat znxs.php
```

新建一个分支changeTest 切换过去，将znxs.php内容修改为

```php
<?php
echo 'runoob';
?>
```

提交

```BASH
git commit -am 'changed the znxs.php'
```

修改的内容提交到 change_site 分支中。 现在，假如切换回 master 分支我们可以看内容恢复到我们修改前的(空文件，没有代码)，我们再次修改 znxs.php 文件。

```BASH
$ git checkout master
Switched to branch 'master'
$ cat znxs.php
$ vim znxs.php    # 修改内容如下
$ cat znxs.php
<?php
echo 1;
?>
$ git diff
diff --git a/znxs.php b/znxs.php
index e69de29..ac60739 100644
--- a/znxs.php
+++ b/znxs.php
@@ -0,0 +1,3 @@
+<?php
+echo 1;
+?>
$ git commit -am '修改代码'
[master c68142b] 修改代码
 1 file changed, 3 insertions(+)
```

现在这些改变已经记录到我的 “master” 分支了。接下来我们将 “change_site” 分支合并过来。

```BASH
$ git merge change_site
Auto-merging znxs.php
CONFLICT (content): Merge conflict in znxs.php
Automatic merge failed; fix conflicts and then commit the result.

$ cat znxs.php     # 打开文件，看到冲突内容
<?php
<<<<<<< HEAD
echo 1;
=======
echo 'znxs';
>>>>>>> change_site
?>
```

我们将前一个分支合并到 master 分支，一个合并冲突就出现了，接下来我们需要手动去修改它。

```BASH
$ vim znxs.php 
$ cat znxs.php
<?php
echo 1;
echo 'znxs';
?>
$ git diff
diff --cc znxs.php
index ac60739,b63d7d7..0000000
--- a/runoob.php
+++ b/runoob.php
@@@ -1,3 -1,3 +1,4 @@@
  <?php
 +echo 1;
+ echo 'znxs';
  ?>
```

在 Git 中，我们可以用 git add 要告诉 Git 文件冲突已经解决

```BASH
$ git status -s
UU runoob.php
$ git add runoob.php
$ git status -s
M  runoob.php
$ git commit
[master 88afe0e] Merge branch 'change_site'
```

现在成功解决了合并中的冲突，并提交了结果

### git标签

当使用版本更新，或者一个比较重要的阶段，需要对快照进行标记，就可以对快照打上标签`git tag`

例如 user_center发布了一年版本大更新 可以用`git tag -a v1.0`打上(HEAD)v1.0标签

`-a`表示带上注解的标签

```BASH
$ git tag -a v1.0
```

当执行git tag -a命令的时候 git可能会打开编辑器写一句注解

查看标签

```BASH
git log --decorate
```

如果要查看所有的标签可以使用以下命令

```BASH
$ git tag
```

指定标签信息命令

```BASH
git tag -a <tagname> -m "znxs.com标签"
```

PGP标签命令

```BASH
git tag -s <tagname> -m "znxs.com标签"
```

[![git命令大全](https://www.runoob.com/wp-content/uploads/2015/02/011500266295799.jpg)](https://www.runoob.com/wp-content/uploads/2015/02/011500266295799.jpg)

git命令大全

```BASH
git init                                                  # 初始化本地git仓库（创建新仓库）
git config --global user.name "xxx"                       # 配置用户名
git config --global user.email "xxx@xxx.com"              # 配置邮件
git config --global color.ui true                         # git status等命令自动着色
git config --global color.status auto
git config --global color.diff auto
git config --global color.branch auto
git config --global color.interactive auto
git config --global --unset http.proxy                    # remove  proxy configuration on git
git clone git+ssh://git@192.168.53.168/VT.git             # clone远程仓库
git status                                                # 查看当前版本状态（是否修改）
git add xyz                                               # 添加xyz文件至index
git add .                                                 # 增加当前子目录下所有更改过的文件至index
git commit -m 'xxx'                                       # 提交
git commit --amend -m 'xxx'                               # 合并上一次提交（用于反复修改）
git commit -am 'xxx'                                      # 将add和commit合为一步
git rm xxx                                                # 删除index中的文件
git rm -r *                                               # 递归删除
git log                                                   # 显示提交日志
git log -1                                                # 显示1行日志 -n为n行
git log -5
git log --stat                                            # 显示提交日志及相关变动文件
git log -p -m
git show dfb02e6e4f2f7b573337763e5c0013802e392818         # 显示某个提交的详细内容
git show dfb02                                            # 可只用commitid的前几位
git show HEAD                                             # 显示HEAD提交日志
git show HEAD^                                            # 显示HEAD的父（上一个版本）的提交日志 ^^为上两个版本 ^5为上5个版本
git tag                                                   # 显示已存在的tag
git tag -a v2.0 -m 'xxx'                                  # 增加v2.0的tag
git show v2.0                                             # 显示v2.0的日志及详细内容
git log v2.0                                              # 显示v2.0的日志
git diff                                                  # 显示所有未添加至index的变更
git diff --cached                                         # 显示所有已添加index但还未commit的变更
git diff HEAD^                                            # 比较与上一个版本的差异
git diff HEAD -- ./lib                                    # 比较与HEAD版本lib目录的差异
git diff origin/master..master                            # 比较远程分支master上有本地分支master上没有的
git diff origin/master..master --stat                     # 只显示差异的文件，不显示具体内容
git remote add origin git+ssh://git@192.168.53.168/VT.git # 增加远程定义（用于push/pull/fetch）
git branch                                                # 显示本地分支
git branch --contains 50089                               # 显示包含提交50089的分支
git branch -a                                             # 显示所有分支
git branch -r                                             # 显示所有原创分支
git branch --merged                                       # 显示所有已合并到当前分支的分支
git branch --no-merged                                    # 显示所有未合并到当前分支的分支
git branch -m master master_copy                          # 本地分支改名
git checkout -b master_copy                               # 从当前分支创建新分支master_copy并检出
git checkout -b master master_copy                        # 上面的完整版
git checkout features/performance                         # 检出已存在的features/performance分支
git checkout --track hotfixes/BJVEP933                    # 检出远程分支hotfixes/BJVEP933并创建本地跟踪分支
git checkout v2.0                                         # 检出版本v2.0
git checkout -b devel origin/develop                      # 从远程分支develop创建新本地分支devel并检出
git checkout -- README                                    # 检出head版本的README文件（可用于修改错误回退）
git merge origin/master                                   # 合并远程master分支至当前分支
git cherry-pick ff44785404a8e                             # 合并提交ff44785404a8e的修改
git push origin master                                    # 将当前分支push到远程master分支
git push origin :hotfixes/BJVEP933                        # 删除远程仓库的hotfixes/BJVEP933分支
git push --tags                                           # 把所有tag推送到远程仓库
git fetch                                                 # 获取所有远程分支（不更新本地分支，另需merge）
git fetch --prune                                         # 获取所有原创分支并清除服务器上已删掉的分支
git pull origin master                                    # 获取远程分支master并merge到当前分支
git mv README README2                                     # 重命名文件README为README2
git reset --hard HEAD                                     # 将当前版本重置为HEAD（通常用于merge失败回退）
git rebase
git branch -d hotfixes/BJVEP933                           # 删除分支hotfixes/BJVEP933（本分支修改已合并到其他分支）
git branch -D hotfixes/BJVEP933                           # 强制删除分支hotfixes/BJVEP933
git ls-files                                              # 列出git index包含的文件
git show-branch                                           # 图示当前分支历史
git show-branch --all                                     # 图示所有分支历史
git whatchanged                                           # 显示提交历史对应的文件修改
git revert dfb02e6e4f2f7b573337763e5c0013802e392818       # 撤销提交dfb02e6e4f2f7b573337763e5c0013802e392818
git ls-tree HEAD                                          # 内部命令：显示某个git对象
git rev-parse v2.0                                        # 内部命令：显示某个ref对于的SHA1 HASH
git reflog                                                # 显示所有提交，包括孤立节点
git show HEAD@{5}
git show master@{yesterday}                               # 显示master分支昨天的状态
git log --pretty=format:'%h %s' --graph                   # 图示提交日志
git show HEAD~3
git show -s --pretty=raw 2be7fcb476
git stash                                                 # 暂存当前修改，将所有至为HEAD状态
git stash list                                            # 查看所有暂存
git stash show -p stash@{0}                               # 参考第一次暂存
git stash apply stash@{0}                                 # 应用第一次暂存
git grep "delete from"                                    # 文件中搜索文本“delete from”
git grep -e '#define' --and -e SORT_DIRENT
git gc
git fsck
```
