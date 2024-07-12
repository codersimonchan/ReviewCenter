---

title: git
date: 2019-10-13 22:19:09
---
[TOC]

https://blog.csdn.net/u011863024/article/details/118562748

# Git 概述

## 版本控制

版本控制。版本控制就是记录文件内容的变化，以便将来查阅特定版本修订情况的系统。版本控制其实最重要的是可以记录文件修改的历史记录，从而让用户能够查看历史版本。方便版本切换。比如提交毕业论文第一版给教授，教授进行指点点评，要求修改。那么能直接在打回来的版本上进行修改吗。因为你怎么能确定你的毕业论文越改越好呢，说不定改的还不如上一版本。所以我们一般不会基于上一版本直接改，而是复制了一个副本，在副本之上进行修改并重新命名。那这样一来，下次呢，就把新命名的版本1发送给老师就可以了。那么这样利用多副本的机制做了一个版本控制机制。

那么公司里为什么不可以用多副本机制呢，因为公司内都是团队合作。如果在在服务器上有论文版本1，小红和小蓝分别从服务器上下载论文版本1，并在论文版本1上不同地方进行了修改，如果是多副本，两个人在上传的时候，后上传的人会覆盖掉之前的人修改的部分。那么本来想合并保留两个人的修改，就无法实现了。所以说版本控制是从个人到团队协作的所必须的。

![img](https://img-blog.csdnimg.cn/img_convert/2abc5f18d46afc2d50cee7db484a460f.png)

## 集中式版本控制

集中化的版本控制系统诸如 CVS、 SVN 等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。多年以来，这已成为版本控制系统的标准做法。

这种做法带来了许多好处，每个人都可以在一定程度上看到项目中的其他人正在做些什么。而管理员也可以轻松掌控每个开发者的权限，并且管理一个集中化的版本控制系统， 要远比在各个客户端上维护本地数据库来得轻松容易。

事分两面，有好有坏。这么做显而易见的缺点是中央服务器的单点故障。如果服务器宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。

## 分布式版本控制

像 Git 这种分布式版本控制工具，客户端提取的不是最新版本的文件快照，而是把代码仓库完整地镜像下来（本地库）。这样任何一处协同工作用的文件发生故障，事后都可以用其他客户端的本地仓库进行恢复。因为每个客户端的每一次文件提取操作，实际上都是一次对整个文件仓库的完整备份。

分布式的版本控制系统出现之后,解决了集中式版本控制系统的缺陷：

**服务器断网的情况下也可以进行开发（因为版本控制是在本地进行的）**
每个客户端保存的也都是整个完整的项目（包含历史记录， 更加安全）

## 概述_发展历史

![img](https://img-blog.csdnimg.cn/img_convert/3f25cba01665ab8dcb63fcc79d05f04f.png)

所以git bash里面的命令和linux里面的命令是通用的。

## Git工作机制

**工作区**：本地磁盘的项目存放目录。

**暂存区**：当你对某些文件的修改感到满意并希望将它们包括在下一次提交中，你需要将这些文件添加到暂存区。这个过程在 SourceTree 中就是“Stage Files”。通过暂存文件，你可以选择性地提交某些文件的修改，而不是一次性提交工作目录中所有的更改。这使得你能够更精细地控制版本历史记录。

**本地库**：一旦提交到本地库，就有了历史版本记录。所有被追踪的文件都会被提交的。 那么一旦生成历史版本，**你的代码就会有记录了**。

![img](https://img-blog.csdnimg.cn/img_convert/1706b3553447ac9f8d95377d965d4fd2.png)

## 代码托管中心

代码托管中心是基于网络服务器的远程代码仓库，一般我们简单称为**远程库**。

- 局域网
  - GitLab
- 互联网
  - GitHub（外网）
  - Gitee 码云（国内网站）

# 基础操作

## 设置全局信息

```shell
设置开发者的用户名
	git config --global user.name Simon
设置开发者邮箱
	git config --global user.email nicolaslee5@foxmail.com
取得全部的全局信息
	git config --list
	git config -l
```

说明：**签名的作用是区分不同操作者身份**。用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次提交是谁做的。 **Git 首次安装必须设置一下用户签名，否则无法提交代码**。虽然您可以**随意设置**用户名称和邮箱地址，但是建议您使用与您身份相关的真实信息，这样可以更好地追踪和管理提交记录，以及在协作项目中识别提交的来源.

**注意**： 这里设置用户签名和将来登录 GitHub（或其他代码托管中心）的账号没有任何关系。

## Git 常用命令

- **创建目录当做仓库目录, 创建了一个名为.git非空的隐藏文件夹**

```shell
git init                 
```

- **观察仓库的状态**

```shell
git status
```

- **将文件添加到git暂存区**


```shell
git add filename
```

- **批量添加新文件到暂存区, 添加所有在仓库目录中创建的新文件到暂存区**


```shell
git add .
```

- **取消在暂存区追踪的某个文件，但不会删除工作区，那么这个文件可以重新add到暂存区**

```
git rm --cached 文件名
```

- **将文件提交到版本库中，现在才表示将新的文件提交到了GIT之中进行管理，每次提交时都会自动生成一个Commit ID（在日后版本恢复中使用）**这样切换版本的时候，本地工作区里文件才会根据版本的切换进行变化。

```shell
git commit -m "Comment"
```

- **自动增加并提交修改到版本库中，只要在提交的时候，给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存**
  **起来一并提交，从而跳过 git add 步骤：**

```shell
git commit -a -m "注释Comment"
```
- 使用 git reflog 命令可以查看本地仓库的**引用日志**，git reflog 可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）。例如，执行 git reset --hard HEAD~1，退回到上一个版本，用git log则是看不出来被删除的commitid，用git reflog则可以看到被删除的commitid，我们就可以买后悔药，将**工作区内容**恢复到被删除的那个版本。

```
git reflog
```

![image-20231201211258380](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231201211258380.png)

- **查看本本详细信息,`git log` 命令可以显示所有提交过的版本信息，如果感觉太繁琐，可以加上参数 `--pretty=oneline`，只会显示版本号和提交时的备注信息**

```
git log --pretty=oneline
```

- 穿梭到指定版本号，可以将工作区文件穿梭到任意一个版本号

```
git reset --hard 41f776b
```

**Git 切换版本，底层其实是移动的HEAD指针，git控制版本可不是创造了多个副本，而是在本地库的内存里面，记录了很多个日志和版本信息。通过调用指针来指向不同的版本。Head指针指向了master分支，master分支指向了某一个版本。**



![image-20231201212726779](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231201212726779.png)

- **比对暂存区和工作区中readme.txt文件的差异**

```shell
git diff
git diff 文件名
git diff --cached
```

### 总结：

- `git diff` 用于查看工作目录中未暂存(git add 之前)的更改。
- `git diff 文件名` 用于查看指定文件在工作目录中与最后一次提交之间的差异。
- `git diff --cached` 用于查看已暂存但尚未提交的更改。

# Git 分支操作



![img](https://img-blog.csdnimg.cn/img_convert/bcad650a512a72097b3391e00ecb8bbe.png)



## 什么是分支

在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来， 开发自己分支的时候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是一个**单独的副本**。（分支底层其实也是指针的引用）
![某项目有四条分支](https://img-blog.csdnimg.cn/img_convert/f1d0659ed000e9dfa295fc696a58cf74.png)

## 分支好处

同时并行推进多个功能开发，提高开发效率。

各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。**失败的分支删除重新开始即可**。

## 分支的操作

- **查看当前仓库中可用的分支**

```git
git branch
```

```bash
git branch
iss53
* master
testing
```

注意 master 分支前的 * 字符：它代表现在检出的那一个分支（也就是说，当前 HEAD 指针所指向的分支）。

`--merged` 与 `--no-merged` 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。

如果要查看哪些分支已经合并到当前分支（master），可以运行 git branch --merged：

```bash
$ git branch --merged
iss53
* master
```

因为之前已经合并了 iss53 分支，所以现在看到它在列表中。 在这个列表中分支名字前没有 * 号的分支通常可以使用 `git branch -d` 删除掉；因为你已经将它们的工作整合到了另一个分支，所以并不会失去任何东西。

- **创建新的分支**

  创建新分支时会基于当前所在的分支。新分支将包含创建时所在分支的所有提交历史和文件状态。并且是从 `当前` 分支当前状态的一个拷贝。

```shell
git branch 分支名
```

如果您希望在不同的分支基础上创建新分支，可以在 `git checkout` 命令后面指定基于的分支名称，例如在 `dev` 分支的基础上创建一个新分支 `feature`，并将 `dev` 分支上的提交历史和文件内容作为新分支的起点。`-b` 参数在 `git checkout` 命令中表示创建并切换分支。

```
// 在远程仓库的dev分支的基础上创建新的本地分支dev
git checkout -b dev origin/dev
// 然后在本地dev分支的基础上创建新的本地分支feature/task
git checkout -b feature/task dev
```

- **切换分支**

```shell
git checkout 分支名
```

要想进行开发，一定不能在master中开发，必须新建一个分支进行开发。在 Git 中，本地创建的分支默认情况下是存在于本地仓库中的，其他团队成员不会直接知道您在本地创建的新分支。只有在您将本地创建的分支推送到远程仓库后，其他团队成员才能看到该分支。

- ##### 删除本地分支

  ```
  git branch -d <branch_name>
  ```

## 合并分支

回到master进行合并分支， 将指定的分支合并到当前分支上。 如果dev分支是基于master基础分支之上做了一些修改，那么就会将修改合并到master分支上，这就是一个正常的合并。两个分支即使都有修改，也可以进行合并。当您尝试合并两个具有不同修改的分支时，Git会尝试自动合并这些更改。

```shell
git merge dev

git merge --no-ff myfeature

```

`git merge --no-ff` 是 Git 中用于合并分支的一个选项，它的作用是禁用 Fast-Forward 合并，强制创建一个新的合并提交（即无论是否可以快进更新都创建一个新的提交），即使当前分支能够直接快进到目标分支。也就是将dev分支的几个提交节点，打包作为一次修改挪到了master分支上，并且在master主分支创建了一个新的节点。

在默认情况下，Git 会尽可能地使用 Fast-Forward 合并。这种合并方式会直接将目标分支的指针移动到要合并的分支的最新提交，不会创建新的合并提交。这样的合并是没有明确的合并历史，可能导致在项目历史中难以追踪分支的合并情况。

![img](https://nvie.com/img/merge-without-ff@2x.png)

## 合并分支(冲突合并)

#### 冲突产生的原因

合并分支时， 如果dev分支是从master生出来的分支，那么本应该就是在master分支基础上进行的修改，如果此时合并到master分支，并不会有任何问题，就会是一次正常的合并。可是如果在master生出dev分支之后，master分支也在原有的基础上进行了修改，并且是针对同一位置进行了修改，那么导致master合并dev分支的时候，发现此时dev分支并不是在原有的master分支上的修改了，那么就会造成冲突合并，因为 Git 无法替我们决定使用哪一个版本，因此，必须**人为决定**新代码内容。那么如果是针对不同位置的修改，那么git会分别保留两个版本对不同位置的修改，进行合并，这样也并不会造成冲突。

以上就好比论文1版本被导师退回，好朋友拿到copy了我们的论文1版本，帮助我们进行修改，然后返回给我们版本2，如果本人并没有对论文1进行任何修改的话，那么在拿到论文版本2的时候，可以直接合并到论文1的基础之上。可是如果本人也在论文1的基础上进行了修改，且针对同一位置进行了修改，那么论文2在合并到论文版本1的时候，就会产生冲突。因为git发现，两个人都对论文同一位置进行了修改，无法决定应该保留哪一部分进行保存。如果是不同位置，那么git会自然地保留两个人分别对论文的修改。

### 撤销修改

#### 撤销add操作——暂存区到工作区

如果您已经使用 `git add` 命令将文件添加到 Git 暂存区，但是希望撤销添加操作，您可以使用以下命令将文件从暂存区中移除：

```shell
git reset <file>
```

其中，`<file>` 是您想要撤销添加操作的文件名或文件路径。

如果您想要**撤销所有已添加到暂存区的文件**，可以使用以下命令：

```
git reset
```

这将移除所有已添加到暂存区的文件。请注意，这不会影响您已经提交的文件，只会将它们从暂存区中移除。

#### 撤销工作区修改——还原文件的所有更改

**撤销对某个文件的修改，而不是撤销添加操作**，可以使用以下命令：

```
git checkout -- <file>
```

这将**还原指定文件的最新提交版本，并覆盖暂存区和工作目录中的任何更改**。请注意，这将永久删除您在该文件上进行的所有未提交更改。

显示那些文件发生了改变

```shell
git checkout
```

恢复单个文件

```shell
git checkout 文件名
```

恢复多个文件

```shell
git checkout .
```

#### 修改已在暂存区

将暂存区的文件撤回到工作区

```shell
git reset HEAD <file>...
```

**将暂存区的文件撤回到工作区 :`.`-多个文件**

```
git reset HEAD .
```

文件删除

```shell
del 文件名
```

删除工作区中的文件
恢复

```shell
git checkout -- 文件名
```

## 移除文件

`git rm` 

要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。可以用 `git rm` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

git rm 命令后面可以列出文件或者目录的名字，**也可以使用 glob 模式**。比如：

```
$ git rm log/\*.log
```

注意到星号 * 之前的反斜杠 \， 因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 shell 来帮忙展开。
此命令删除 log/ 目录下扩展名为 .log 的所有文件。 类似的比如：

```
$ git rm \*~
```

该命令会删除所有名字以 ~ 结尾的文件。

该命令会删除所有名字以 ~ 结尾的文件。

注意到星号 * 之前的反斜杠 \， 因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 shell 来帮忙展开。
此命令删除 log/ 目录下扩展名为 .log 的所有文件。 类似的比如：

```
$ git rm \*~
```

该命令会删除所有名字以 ~ 结尾的文件。

该命令会删除所有名字以 ~ 结尾的文件。

# 远程仓库的使用

### 查看远程仓库

```shell
git remote [-v]

$ git remote -v
origin  https://ITDevSimon@bitbucket.org/newaim/newaim-zendesk-report-frontend.git (fetch)
origin  https://ITDevSimon@bitbucket.org/newaim/newaim-zendesk-report-frontend.git (push)
```

选项 `-v`，会显示需要读写远程仓库使用的 Git 保存的别名与其对应的 URL。你的远程仓库 `origin` 对应着一个 Bitbucket 仓库，它有一个 URL 用于拉取（fetch）和一个 URL 用于推送（push）。

### 添加远程仓库

`git remote add <shortname> <url>`  为本地仓库，添加一个新的远程 Git 仓库

```shell
git remote add origin https://github.com/codeOflI/rep.git
```

### Clone远程仓库

```
git clone （HTTPS）https://github.com/codeOflI/ssm-crud
```

注：**==git clone默认只会克隆master==**

克隆远程仓库后，本地会有一个与远程仓库的默认分支（如 `master`）相对应的本地分支，并且这个本地分支会自动设置为远程分支的追踪分支.

### 从远程仓库中抓取和拉取

以下这个命令会访问远程仓库，从中拉取所有你还没有的数据。

```console
git fetch [origin]
```

如果你使用 `clone` 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为别名简写。 所以，`git fetch origin` 会抓取克隆（或上一次抓取）远程仓库后新推送的所有工作,它只是将远程仓库中的新提交和分支信息拉取到本地，更新本地缓存的远程仓库的引用信息。比如远程仓库有那些branch，做了哪些变动。  必须注意 `git fetch` 命令会将数据拉取到你的本地仓库——但不会自动合并或修改您当前所在的分支。它只是将远程仓库的变化拉取到本地仓库，您可以稍后根据需要执行合并操作

```
git pull
```

如果你有一个分支设置为跟踪一个远程分支，可以使用 `git pull` 命令来自动的抓取然后合并远程分支到当前分支。 这对你来说可能是一个更简单或更舒服的工作流程；**默认情况下，`git clone` 命令会自动设置本地 master 分支跟踪克隆的远程仓库的 master 分支**（或不管是什么名字的默认分支）。 运行 `git pull` 通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。如果本地分支与远程分支之间已经建立了追踪关系，那么在使用 `git pull` 命令时，通常情况下是**不需要显式**指定远程分支的名字的。

### 查看远程仓库和本地仓库的所有分支

```
git branch -a 
```

a means all.

### 依据远程仓库分支生成本地分支

当我们 git clone + 远程仓库地址 下来代码之后，git branch 发现只有master分支，而我们大多时候都是在其它分支处理事情的，所以我们用git branch -a 查看所有分支，包括远程和本地所有的分支。如果我们想建立对应远程分支的本地分支，可使用以下命令。

```
git checkout -b dev origin/dev
```

这个操作会将 `origin/dev` 的内容复制到本地的 `dev` 分支，并将本地的 `dev` 分支与远程的 `origin/dev` 建立**追踪**关系，即设置本地分支跟踪远程分支, 并立即切换到这个新创建的分支.

### 拉取远程分支

将拉取远程的 `origin/dev` 分支的最新更改并合并到你当前所在的分支（假设你当前在 `dev` 分支上). 如果建立了追踪关系，可以省略远程分支的名字。直接使用git pull就可以将远程分支更新拉取到本地分支，并进行合并操作。

```
git pull origin dev
```
### 列出本地分支与远程分支追踪关系

```
git branch -vv
```
这会列出所有本地分支以及它们与远程跟踪分支之间的关联的追踪关系.

### 推送到远程仓库

`git push [remote-name:远程仓库] [local-branch-name][remote-branch-name]`。 当你想要将 本地master 分支推送到 `origin` 服务器时

```shell
git push origin master(这个master表示local master)

git push origin dev (local branch name)

git push origin Feature_Blue -u(Feature_Blue is local branch name) 
```

这种匹配是 Git 默认的行为，会尝试将本地的同名分支推送到远程仓库的对应分支。如果远程仓库中没有对应的分支，而是会创建一个新的远程分支并将本地分支推送到远程。这意味着 Git 会将本地分支内容推送到远程仓库，并在远程仓库中创建一个同名的新分支。

-u will set up tracking between our local feature branch and the branch on the shared repository.



### 设置git push和pull的默认分支

```
git branch --set-upstream-to=origin/<远程分支> <本地分支>
```

更为简洁的方式是在push时，使用-u参数

```
git push -u origin <远程分支>
```

-u参数会在push的同时会指定**当前分支**的默认上游分支；`-u` 参数（或者 `--set-upstream`）告诉 Git 记住推送到的远程分支，并将本地分支设置为该远程分支的上游分支。这样，在以后的 `git pull` 和 `git push` 命令中，Git 将知道从哪个远程分支拉取更新，以及向哪个远程分支推送更改。

### 查看某个远程仓库

使用 `git remote show [remote-name]` 命令

可以通过 `git remote show` 看到更多的信息。

### 远程仓库的移除与重命名

如果想要重命名引用的名字可以运行 `git remote rename` 去修改一个远程仓库的简写名。 例如，想要将 `pb` 重命名为 `paul`，可以用 `git remote rename` 这样做：

```console
$ git remote rename pb paul
$ git remote
origin
paul
```

 移除一个远程仓库——你已经从服务器上搬走了或不再想使用某一个特定的镜像了，又或者某一个贡献者不再贡献了——可以使用 `git remote rm` ：

```console
$ git remote rm paul
$ git remote
origin
```

### 删除远程分支

```shell
git remote rm origin
```

### 利用bare 参数下载所有远程分支

这就带来一个问题，如果代码分支数量少还好说，如果分支比较多，就比较麻烦了。有什么简单的方法可以一次下载所有分支么？

通过我们在做Git迁移时，需要使用到一个命令。

```shell
git clone --bare https://github.com/pcottle/learnGitBranching
```

即 git clone --bare (需要注意这种方法下载的文件是不能直接使用的)。 关于git迁移，可以[查阅](https://www.jianshu.com/p/7932c715c138)

**那么现在我们也可以通过使用这种方式来进行全量分支的下载**。

```shell
# 创建一个空文件夹
mkdir repo
# 进入该文件夹
cd repo
# 使用bare方式clone代码。并把下载后的文件夹重命名为 .git
git clone --bare path/to/repo.git .git
# 使用该命令(不用担心core.bare是否存在) 或 git config --bool core.bare false
git config --unset core.bare
# 上面的命令执行完,再执行该命令,就可以看到仓库里面的内容了
git reset --hard
```

```
git clone --bare git@github.com:DerekYRC/mini-spring.git .git
```

之后你就可以通过

```
git branch
```

命令查看本地所有分支。你会发现本地有所有的分支。

这里有几点需要注意：

1.是使用bare的形式去下载

2.下载完后重命名文件夹

3.将重命名后的文件夹，放到一个空文件中(这一步不是必须的,但是有必要,因为如果不这么做并且所在文件夹的文件数量有很多的的话,后续的两个命令恢复的代码,会搞的比较乱)

目前来说这种方式是最好的下载git所有分支的办法了。网上的其他方法并不好用。



# 忽略文件.gitignore的操作

## 详见pro git忽略文件规则

下面是一些.gitignore文件忽略的匹配规则：

```

*.a       # 忽略所有 .a 结尾的文件
!lib.a    # 但 lib.a 除外
/TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/    # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```



目的是忽略指定类型的文件或者某个文件夹

1. a、新建.gitignore文件：
2. b、输入要忽略的文件（可用通配符）
3. 利用`git status` 查看，可以看出排除了写入的文件，避免了其提交

```
HELP.md
target/
!.mvn/wrapper/maven-wrapper.jar
!**/src/main/**
!**/src/test/**

### xxx ###
application.properties

### STS ###
.apt_generated
.classpath
.factorypath
.project
.settings
.springBeans
.sts4-cache

### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr
.mvn/
mvnw
mvnw.cmd

### NetBeans ###
/nbproject/private/
/nbbuild/
/dist/
/nbdist/
/.nb-gradle/
build/

### VS Code ###
.vscode/
```



## .gitignore不生效问题解决方法

https://blog.csdn.net/Saintmm/article/details/120847019

# 删除git上已经提交的文件

1.先查看有哪些文件可以删除,但是不真执行删除

```sh
git rm -r -n job-executor-common/target/*
```

-r 递归移除目录

-n 加上这个参数，执行命令时，是不会删除任何文件，而是展示此命令要删除的文件列表预览，所以一般用这个参数先看看要删除哪些文件，防止误删，确认之后，就去掉此参数，真正的删除文件。

上面这个命令就是先查看 job-executor-common/target/* 下有哪些可以删除的内容

2.执行删除

```
git rm -r  job-executor-common/target/*
```

此时,就把指定目录下所有内容从本地版本库中删除了

如果只想**从版本库中删除**,但是本地仍旧保留的话,加上 --cached 参数

```sh
git rm -r --cached job-executor-common/target/*
```

3.**删除远程版本库中的文件**

再执行提交操作即可

```
git commit -m "移除target目录下所有文件"
git push origin dev其中origin dev为分支名称
```

## Supporting Branches 辅助分支

辅助分支只有有限的生命周期，通常在完成使命后会被删除。可能用到的辅助分支分类有：

- Feature branches
- Release branches
- Hotfix branches

## Git Workflows

https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow

### Feature branches

Firstly，we have to clone the source code to the local computer

`git clone （HTTPS）https://github.com/codeOflI/ssm-crud`

// 在远程仓库的dev分支的基础上创建新的本地分支dev, 因为clone只会将master分支clone到本地
`git checkout -b dev origin/dev`
// 然后在本地dev分支的基础上创建(checkout)新的本地分支feature/task
`git checkout -b feature/task dev`

`git push origin feature/task -u(Feature_Blue is local branch name)` 

然后在remote上，create pull request， 将feature 分支合并到dev分支上，并删除feature分支

然后将dev分支合并到master分支上面。

### Release branches

在远程的dev分支上，直接建立release分支，此时仍然可以有一些微小的调整和改动（建立本地分支，然后更新推送）。然后使用pull request，将此release分支合并进入master分支上。还需要使用pull request，将release分支合并到dev分支上

### Hotfix branches

在远程的master分支上，直接建立hotfix分支，依据remote hotfix建立本地hotfix分支，修改后然后更新推送。再使用pull request，将此hotfix分支合并进入master分支上。在远程的dev分支上，直接建立release分支，此时仍然可以有一些微小的调整和改动（建立本地分支，然后更新推送）。然后使用pull request，将此release分支合并进入master分支上。还需要使用pull request，将hotfix分支合并到dev分支上

# SourceTree

https://testerhome.com/articles/33470

## 时空穿梭

- 回顾历史

穿越回去，只看过去的历史现场

在不同的commit上面双击，就可以随时切换到当时的历史现场，查看当时的代码情况。只是看到了当时版本的保存的内容，但之前的commit的版本还是保存着的。

- 改变历史

穿越回去，改变历史，重塑未来

在想穿越的版本点right click，选择Reset current branch to this commit.(重置当前的分支到这次提交)，弹出对话框，里面有3种模式可供选择，去重置到这次提交，分别是

1. Soft-keep all local changes(回到暂存状态)
2. Mixed-keep working copy but reset index（撤销仓库的版本内容，但是工作区内容仍然保存，但是并不在暂存区）
3. Hard-discard all working copy changes（撤销版本内容，工作区内容也不再保存）

建议选择Mixed模式，这样工作区内容仍然保存，在File Status处仍然可以看到工作区中的修改，如果此时确认不想要修改的代码，那么就可以右击文件，选择revert（还原），依次将文件内容还原。会和Hard mode达到一样的效果。

## 平行宇宙

主宇宙（主分支）

平行宇宙（平行分支）

只需要双击，就可以在平行宇宙之间互相切换。平行宇宙的提交不会在graph上产生分叉，但会显示提前了几个版本。

如果想将平行宇宙的内容合并到主宇宙，那么就要切换到主宇宙master分支，然后点击Merge按钮，选中要合并的平行宇宙（分支）。选择以下选项（普通合并）。点击ok，会将分支合并到主分支。此时会有graph上面会产生分叉。可以看到分支是从何时被创建，又何时被merge

![1707794528278_F424E503-4FB2-4718-8FFA-23C45E7268F6](C:\Users\simon.cheng\AppData\Roaming\DingTalk\2278316978_v2\ImageFiles\1707794528278_F424E503-4FB2-4718-8FFA-23C45E7268F6.png)

- [x] Commit merge immediately(if no conflicts)
- [x] Create a new commit even if fast-forward is possible

如果选择使用变基代替合并，就会将平行宇宙中节点，一个节点一个节点地挪到主宇宙当中，然后再把主宇宙上的提交放到变基的后面来。所谓变基rebase，是指原来使用master作为我们的基准，那么现在是将平行宇宙作为我们的基准。

![1707794260306_C1120D2B-1AA1-45ee-AB4A-2A917098036D](C:\Users\simon.cheng\AppData\Roaming\DingTalk\2278316978_v2\ImageFiles\1707794260306_C1120D2B-1AA1-45ee-AB4A-2A917098036D.png)

交互式变基：对提交的版本次数进行压缩。？？

## Graph显示

当主干在有新的分支创建之后，又有新的commit（提交节点）的时候，那么分支图会出现变化，不然的话，主干的节点和分支的节点仍然是在一条线上的，体现不出来分支的效果。 所以一般在合并之后才会出现分支，因为没有合并的时候，master上没有新的commit，无法显示分叉。

# WorkFlow

- [ ] 首先拉取从远程仓库拉取dev分支到本地仓库，这样本地仓库的dev分支就是最新的内容。

- [ ] 然后依赖dev分支，创建新的feature分支。在feature分支上进行feature的开发。

- [ ] 如果feature开发完毕，在本地运行没有任何问题，直接将feature分支push到远程仓库，并生成远程仓库的feature分支。

  >1，如果有ch此时可以看到落后dev几个版本，Sync now的时候，会提示我们使用git 命令将远程的dev合并到本地的feature分支
  >
  >2，合并的时候如果有冲突，在本地的feature分支里面，就可以看到冲突的地方，看看保留哪些内容，将新的内容整理好后再次commit
  >
  >3，然后再将本地的feature分支推送到远程，然后在远程仓库中将，将feature分支合并到dev分支。

- [ ] 然后在远程仓库中，将feature分支合并到dev分支，这样在线dev环境中，可以查看新增的功能。

- [ ] 如果在线dev环境没有任何问题，那么可以将远程dev的分支合并到远程master分支，并进行发布。

- [ ] 在本地的dev分支，再次拉取远程dev到本地分支。
