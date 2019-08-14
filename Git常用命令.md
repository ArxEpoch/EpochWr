# Git常用命令总结

## 1、git init（初始化）

​	在本地新建一个repository，进入一个项目目录，执行git init命令，会初始化一个repository，并在其目录下生成一个 .git 文件。

```
git init
```

## 2、git clone（远程克隆）

​	获取一个url对应的远程Git repository，创建一个local copy。clone下来的repository会以url最后一个斜线的名称命名。

```
git clone (url)
```

​	指定clone下来的repository到本地的新名称。

```
git clone (url) newName
```

## 3、git status（查看状态）

​	查询当前repository的状态。

```
git status
```

​	-s表示short，其输出标记会有两列，第一列是对staging区域而言，第二列是对working目录而言。

```
git status -s
```

## 4、git add（添加到暂存区）

​	将新添加的文件或者新加入的改动，放入Git暂存区（staging area）中。

​	注意：commit时提交的改动是上一次加入到暂存区的改动，而不是我们磁盘上的改动。

指定文件

```
git add 文件名.文件类型
```

当前目录下的所有文件

```
git add .
```

## 5、git commit（提交）

提交暂存区中的改动到repository。

```
git commit -m "your commit message"
```

先把所有已经track的文件的改动add进来，然后提交。对于没有track的文件，还需要git add 一下。

```
git commit -a
```

增补提交。会使用与当前提交节点相同的父节点进行一次新的提交，旧的提交将被取消。

```
git commit --amend
```

## 6、git log（分支日志）

​	显示分支的提交历史。

每条log只显示一行，显示number条。

```
git log --oneline --number
```

图形化地表示出分支合并历史

```
git log --oneline --graph
```

显示特定分支的log

```
git log 分支名
```

查看在分支1，却不在分支2中的提交。^ 表示排除这个分支（Windows下可能要给^branch2 加上引号）

```
git log --oneline branch1 ^branch2
```

显示tag信息

```
git log --decorate
```

指定作者的提交历史

```
git log --author=[author name]
```

根据提交时间筛选log

```
git log --since --before --until --after
```

将合并的提交排除在外

```
git log --no-merges
```

根据commit的信息过滤log

```
git log --grep
```





## 7、git diff（比较差异）

比较工作目录中当前文件和暂存区域快照之间的差异，也就是修改之后还没有暂存起来的变化内容。

```
git diff
```

已经暂存起来的文件和上次提交时的快照之间的差异

```
git diff --cached
```

```
git diff --staged
```

比较工作目录和上次提交之间的改动

```
git diff HEAD
```

看自从某个版本之后的改动

```
git diff [version tag]
```

比较两个分支

```
git diff [branchA] [branchB]
```

## 8、git reset（撤销更改）

HEAD关键字：指的时当前分支最末捎最新的一个提交，也就是版本库上的最新版本。

把不小心add进去的文件从staged状态取出来

```
git reset HEAD
```

指定单独某一个文件

```
git reset HEAD --filename
```

将最新版本移动到特定的提交名字，索引和暂存区不动

```
git reset --soft
```

取消阶段文件，并撤消自上次提交以来工作目录中的任何更改。

```
git reset --hard
```

​	使用git reset —hard HEAD进行reset,即上次提交之后,所有staged的改动和工作目录的改动都会消失,还原到上次提交的状态.

这里的HEAD可以被写成任何一次提交的SHA-1.

不带soft和hard参数的git reset,实际上带的是默认参数mixed.

​     **总结:**

​     git reset --mixed id,是将git的HEAD变了(也就是提交记录变了),但文件并没有改变，(也就是working tree并没有改变). 取消了commit和add的内容.

​	git reset --soft id. 实际上，是git reset –mixed id 后,又做了一次git add.即取消了commit的内容.

​	git reset --hard id.是将git的HEAD变了,文件也变了.

按改动范围排序如下:

​     soft (commit) < mixed (commit + add) < hard (commit + add + local working)

## 9、git revert（反转撤销提交）

反转撤销提交。只要把出错的提交的名字作为参数传给命令就可以了。

撤销最近的一个提交

```
git revert HEAD
```

git revert 会创建一个反向的新提交，可以通过参数-n来高数Git先不要提交。

## 10、git rm（移除）

从staging区域移除文件，同时也移除出工作目录

```
git rm file
```

从staging区域移除文件，但留在工作目录中

注：git rm --cached从功能上等同于 git reset HEAD，清除了缓存区，但不懂工作目录树。

```
git rm --cached
```

## 11、git clean（清除）

从工作目录中移除没有track的文件

-d 表示同时移除目录

-f 表示force，因为在Git的配置文件中，clean.requireForce = true,如果不加 - f ，clean将会拒绝执行。

```
git clean -df
```

## 12、git stash（栈）

把当前的改动压入一个栈。将会把当前目录和index中的所有改动（但不包括未track的文件）压入一个栈，然后留给你一个clean的工作状态，即处于上一次的最新提交处。

```
git stash
```

显示这个栈的list

```
git stash list
```

取出stash中的上一个项目（stash@{0}），并且应用于当前的工作目录。

```
git stash apply/git stash apply stash@{1}
```

如果你在应用stash中的项目的同时想要删除它

```
git stash pop
```

删除stash中的项目：

（1）删除上一个，也可指定参数删除指定的一个项目

```
git stash drop
```

（2）删除所有项目

```
git stash clear
```

## 13、git branch（列出、创建和删除分支）

列出本地所有分支，当前分支会被星号标示出来

```
git branch
```

查看每个分支的最后一次提交

```
git branch -v
```

常见一个新的分支（分支是基于你的上一次提交建立的）

```
git branch (brabchname)
```

删除一个分支

```
git branch -d (branchname)
```

删除remote的分支

完整形式：

```
git push remote-name local-branch:remote-branch
```

## 14、git checkout(切换、替换)

切换到一个分支

```
git checkout (branchname)
```

创建并切换到一个新的分支

```
git checkout -b (branchname)
```

替换本地改动。

```
git checkout filename
```

此命令会使用HEAD中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件不受影响。

注意：git checkout filename 会删除该文件中所有没有暂存和提交的改动，这个操作不可逆。

## 15、git merge(合并分支)

把一个分支合并到当前的分支

```
git merge [alias]/[branch]
```

如果出现冲突，需要手动修改，可以用git mergetool。

解决冲突的时候可以用的git diff，解决完之后用git add添加，即表示冲突已经被resolved。

## 16、git tag（建立标签）

在一个提交上建立永久性的书签，通常是发布一个release版本或者ship了什么东西之后加tag。

```
git tag v1.0
```

```
git tag -a v1.0
```

-a参数表示会允许添加一些信息。

当你运行git tag -a命令时，Git会打开一个编辑器让你输入tag信息。

注意：

push的时候是不包含tag的，如果想包含，可以在push的时候加上  --tags参数

fetch的时候，branch HEAD可以reach的tags是自动被fetch下来的，如果想确保所有的tags都被包含进来，需要加上--tags选项。

## 17、git remote（管理别名）

因为不需要每次都用完整的url，所有Git为每一个远程仓库的url都建立一个别名，然后使用git remote来管理。

列出所有的remote aliases

```
git remote
```

如果你clone一个project，Git会自动将原来的url添加进来，别名叫做：origin

看见每一个别名对应的实际url

```
git remote -v
```

添加一个新的远程仓库

```
git remote add [alias] [url]
```

删除一个存在的远程仓库

```
git remote rm [alias]
```

重命名

```
git remote rename [old-alias][new-alias]
```

更新url。可以加上-push和fetch参数，为同一个别名set不同的存取地址。

```
git remote set-url [alias] [url]
```

## 18、git fetch

​	fetch将会取到所有你本地没有的数据,所有取下来的分支可以被叫做remote branches,它们和本地分支一样(可以看diff,log等,也可以merge到其他分支),但是Git不允许你checkout到它们.

```
git fetch [alias]
git fetch --all
```

## 19、git pull

pull = fetch + merge FETCH_HEAD

​	git pull会首先执行git fetch,然后执行git merge,把取来的分支的

head merge到当前分支.这个merge操作会产生一个新的commit.  

如果使用--rebase参数,它会执行git rebase来取代原来的git merge.

## 20、git push

把当前分支merge到alias上的【branch】分支，如果分支已经存在，将会更新，如果不存在，将会添加这个分支。

```
git push [alias] [branch]
```

 		如果有多个人向同一个remote repo push代码, Git会首先在你

试图push的分支上运行git log,检查它的历史中是否能看到server上

的branch现在的tip,如果本地历史中不能看到server的tip,说明本地

的代码不是最新的,Git会拒绝你的push,让你先fetch,merge,之后再

push,这样就保证了所有人的改动都会被考虑进来.

## 21、git rebase

--rebase不会产生合并的提交,它会将本地的所有提交临时保存为补

丁(patch),放在”.git/rebase”目录中,然后将当前分支更新到最新的分

支尖端,最后把保存的补丁应用到分支上.

​     rebase的过程中,也许会出现冲突,Git会停止rebase并让你解决冲

突,在解决完冲突之后,用git add去更新这些内容,然后无需执行

commit,只需要:

​     git rebase --continue就会继续打余下的补丁.

​     git rebase --abort将会终止rebase,当前分支将会回到rebase之前的状态.

## 22、git reflog

​	  git reflog是对reflog进行管理的命令,reflog是git用来记录引用变

化的一种机制,比如记录分支的变化或者是HEAD引用的变化.

​     当git reflog不指定引用的时候,默认列出HEAD的reflog.

​     HEAD@{0}代表HEAD当前的值,HEAD@{3}代表HEAD在3次变化

之前的值.

​     git会将变化记录到HEAD对应的reflog文件中,其路径

为.git/logs/HEAD, 分支的reflog文件都放在.git/logs/refs目录下的子

目录中.
