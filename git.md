[TOC]

# 创建版本库

```shell
$ git init
Initialized empty Git repository in C:/job/learngit/.git/
```

# 添加文件到版本库

把文件修改添加到暂存区；

```shell
$ git add readme.txt
warning: LF will be replaced by CRLF in readme.txt.
The file will have its original line endings in your working directory
```

# 提交到版本库

把暂存区的所有内容提交到当前分支。

```shell
$ git commit -m "add readme.txt"
[master (root-commit) e4ad803] add readme.txt
 1 file changed, 1 insertion(+)
 create mode 100644 readme.txt
```

# 查看仓库状态

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

# 查看difference

```shell
$ git diff readme.txt
warning: LF will be replaced by CRLF in readme.txt.
The file will have its original line endings in your working directory
diff --git a/readme.txt b/readme.txt
index ec5d73e..6ca5d5e 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1,2 @@
-this is version
+this is version 1
+this is version 2
\ No newline at end of file
```

# 查看提交记录

```shell
$ git log
commit d24b6fed6f00f1cf49d219243e1b12972329b448 (HEAD -> master)
Author: wuzhihao7 <wuzhihao7@outlook.com>
Date:   Wed May 29 11:34:41 2019 +0800

    version3

commit 72b87bdc8f32fdbd1640fafa64da5924d7342e09
Author: wuzhihao7 <wuzhihao7@outlook.com>
Date:   Wed May 29 11:33:03 2019 +0800

    version2

commit e4ad8034eb7f687db521e87a9d0e17300d81ed24
Author: wuzhihao7 <wuzhihao7@outlook.com>
Date:   Wed May 29 10:59:37 2019 +0800

    add readme.txt
    
$ git log --pretty=oneline
d24b6fed6f00f1cf49d219243e1b12972329b448 (HEAD -> master) version3
72b87bdc8f32fdbd1640fafa64da5924d7342e09 version2
e4ad8034eb7f687db521e87a9d0e17300d81ed24 add readme.txt
```

# 版本回退

Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

```shell
//把暂存区的修改撤销掉（unstage），重新放回工作区
$ git reset HEAD readme.txt
Unstaged changes after reset:
M       readme.txt
//回退到上一个版本
$ git reset --hard HEAD^
HEAD is now at 72b87bd version2
//回退到最新提交版本
$ git reset --hard d24b6fed6f00f1cf49d219243e1b12972329b448
HEAD is now at d24b6fe version3
```

# 查看命令历史记录

```shell
$ git reflog
d24b6fe (HEAD -> master) HEAD@{0}: reset: moving to d24b6fed6f00f1cf49d219243e1b12972329b448
72b87bd HEAD@{1}: reset: moving to HEAD^
d24b6fe (HEAD -> master) HEAD@{2}: commit: version3
72b87bd HEAD@{3}: commit: version2
e4ad803 HEAD@{4}: commit (initial): add readme.txt
```

# 查看工作区和版本库里面最新版本的区别

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ git diff HEAD -- readme.txt
diff --git a/readme.txt b/readme.txt
index fc40206..3617c08 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,3 +1,4 @@
 this is version 1
 this is version 2
 this is version 3
+this is version 4
```

# 丢弃工作区的修改

让这个文件回到最近一次`git commit`或`git add`时的状态

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ git checkout -- readme.txt
```

# 删除文件

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

```shell
$ git rm test.txt
rm 'test.txt'

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        deleted:    test.txt

$ git commit -m "rm test.txt"
[master 01e4329] rm test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 test.txt
```

# 添加远程仓库

```shell
$ git remote add origin git@github.com:wuzhihao7/learngit.git
```

# 把本地库内容推送到远程仓库

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

```shell
$ git push -u origin master
Enumerating objects: 13, done.
Counting objects: 100% (13/13), done.
Delta compression using up to 8 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (13/13), 1019 bytes | 203.00 KiB/s, done.
Total 13 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To github.com:wuzhihao7/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

从现在起，只要本地作了提交，就可以通过命令：

```shell
$ git push origin master
```

# 克隆仓库

```shell
$ git clone git@github.com:wuzhihao7/learngit.git learngit2
Cloning into 'learngit2'...
remote: Enumerating objects: 13, done.
remote: Counting objects: 100% (13/13), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 13 (delta 1), reused 13 (delta 1), pack-reused 0
Receiving objects: 100% (13/13), done.
Resolving deltas: 100% (1/1), done.
```

# 创建并切换分支

```shell
$ git checkout -b dev
Switched to a new branch 'dev'
//相当于以下两条命令：
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

# 查看当前分支

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

```shell
$ git branch
* dev
  master
```

# 合并指定分支到当前分支

```shell
$ git merge dev
Updating 01e4329..c5dcd57
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
 
$ git log
commit c5dcd578aaa5f1ef323cb82231762a749af27626 (HEAD -> master, dev)
Author: wuzhihao7 <wuzhihao7@outlook.com>
Date:   Wed May 29 15:53:14 2019 +0800

    version4 at dev

commit 01e4329d253aeb75d4799594b88589301aa5ccf0 (origin/master)
Author: wuzhihao7 <wuzhihao7@outlook.com>
Date:   Wed May 29 14:16:54 2019 +0800

    rm test.txt
```

用带参数的`git log`也可以看到分支的合并情况：

```shell
$ git log --graph --pretty=oneline --abbrev-commit
*   2513abd (HEAD -> master) merge with no-ff
|\
| * d7ee6d2 (dev) version 6 at dev
|/
*   deb15a8 conflict fixed
|\
| * 0a93f24 version 5 at feature1
* | f6b1271 version 5 at master
|/
* c5dcd57 version4 at dev
* 01e4329 (origin/master) rm test.txt
* 978cffb add test.txt
* d24b6fe version3
* 72b87bd version2
* e4ad803 add readme.txt

```

# 删除分支

```shell
$ git branch -d dev
Deleted branch dev (was c5dcd57).
```

# 解决冲突

```shell
wuzhihao@WuZhihao MINGW64 /c/job/learngit (master)
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.

wuzhihao@WuZhihao MINGW64 /c/job/learngit (master|MERGING)
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

wuzhihao@WuZhihao MINGW64 /c/job/learngit (master|MERGING)
$ less readme.txt

wuzhihao@WuZhihao MINGW64 /c/job/learngit (master|MERGING)
$ less readme.txt

wuzhihao@WuZhihao MINGW64 /c/job/learngit (master|MERGING)
$ vi readme.txt

wuzhihao@WuZhihao MINGW64 /c/job/learngit (master|MERGING)
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

wuzhihao@WuZhihao MINGW64 /c/job/learngit (master|MERGING)
$ git add readme.txt

wuzhihao@WuZhihao MINGW64 /c/job/learngit (master|MERGING)
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

        modified:   readme.txt


wuzhihao@WuZhihao MINGW64 /c/job/learngit (master|MERGING)
$ git commit -m "conflict fixed"
[master deb15a8] conflict fixed

wuzhihao@WuZhihao MINGW64 /c/job/learngit (master)
```

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

```shell
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)

$ git log
commit 2513abd9036b00e047b56b547db9c2d1f7fea2ab (HEAD -> master)
Merge: deb15a8 d7ee6d2
Author: wuzhihao7 <wuzhihao7@outlook.com>
Date:   Wed May 29 16:41:35 2019 +0800

    merge with no-ff

commit d7ee6d22841778acedd353b9ac74d8f41f22bfed (dev)
Author: wuzhihao7 <wuzhihao7@outlook.com>
Date:   Wed May 29 16:40:28 2019 +0800

    version 6 at dev
```

# 把当前工作现场“储藏”起来，等以后恢复现场后继续工作

```shell
wuzhihao@WuZhihao MINGW64 /c/job/learngit (dev)
$ git status
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

wuzhihao@WuZhihao MINGW64 /c/job/learngit (dev)
$ git stash
Saved working directory and index state WIP on dev: d7ee6d2 version 6 at dev

wuzhihao@WuZhihao MINGW64 /c/job/learngit (dev)
$ git status
On branch dev
nothing to commit, working tree clean

$ git stash list
stash@{0}: WIP on dev: d7ee6d2 version 6 at dev

wuzhihao@WuZhihao MINGW64 /c/job/learngit (dev)
$ git stash apply stash@{0}
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

wuzhihao@WuZhihao MINGW64 /c/job/learngit (dev)
$ git stash list
stash@{0}: WIP on dev: d7ee6d2 version 6 at dev

wuzhihao@WuZhihao MINGW64 /c/job/learngit (dev)
$ git stash drop stash@{0}
Dropped stash@{0} (8510bcf5ce3ccc1cac49ef860ce48707a0600d8e)

wuzhihao@WuZhihao MINGW64 /c/job/learngit (dev)
$ git stash list

wuzhihao@WuZhihao MINGW64 /c/job/learngit (dev)
$ git status
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了

可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```
$ git stash apply stash@{0}
```

# 强行删除未合并的分支

```shell
wuzhihao@WuZhihao MINGW64 /c/job/learngit (master)
$ git branch -d dev
error: The branch 'dev' is not fully merged.
If you are sure you want to delete it, run 'git branch -D dev'.

wuzhihao@WuZhihao MINGW64 /c/job/learngit (master)
$ git branch -D dev
Deleted branch dev (was 62270ec).
```

# 查看远程库的信息

```shell
wuzhihao@WuZhihao MINGW64 /c/job/learngit (master)
$ git remote
origin

wuzhihao@WuZhihao MINGW64 /c/job/learngit (master)
$ git remote -v
origin  git@github.com:wuzhihao7/learngit.git (fetch)
origin  git@github.com:wuzhihao7/learngit.git (push)
```

# 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```shell
$ git push origin master
```

如果要推送其他分支，比如`dev`，就改成：

```shell
$ git push origin dev
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

# 拉取分支

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：

```shell
$ git clone git@github.com:michaelliao/learngit.git
Cloning into 'learngit'...
remote: Counting objects: 40, done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 40 (delta 14), reused 40 (delta 14), pack-reused 0
Receiving objects: 100% (40/40), done.
Resolving deltas: 100% (14/14), done.
```

当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的`master`分支。不信可以用`git branch`命令看看：

```shell
$ git branch
* master
```

现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：

```shell
$ git checkout -b dev origin/dev
```

现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

```shell
$ git add env.txt

$ git commit -m "add env"
[dev 7a5e5dd] add env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   f52c633..7a5e5dd  dev -> dev
```

你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

```shell
$ cat env.txt
env

$ git add env.txt

$ git commit -m "add new env"
[dev 7bd91f1] add new env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
To github.com:michaelliao/learngit.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

```shell
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

`git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

```shell
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

再pull：

```shell
$ git pull
Auto-merging env.txt
CONFLICT (add/add): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.
```

这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push：

```shell
$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   7a5e5dd..57c53ab  dev -> dev
```

# rebase

多人在同一个分支上协作时，很容易出现冲突。即使没有冲突，后push的童鞋不得不先pull，在本地合并，然后才能push成功。

每次合并再push后，分支变成了这样：

```
$ git log --graph --pretty=oneline --abbrev-commit
* d1be385 (HEAD -> master, origin/master) init hello
*   e5e69f1 Merge branch 'dev'
|\  
| *   57c53ab (origin/dev, dev) fix env conflict
| |\  
| | * 7a5e5dd add env
| * | 7bd91f1 add new env
| |/  
* |   12a631b merged bug fix 101
|\ \  
| * | 4c805e2 fix bug 101
|/ /  
* |   e1e9c68 merge with no-ff
|\ \  
| |/  
| * f52c633 add merge
|/  
*   cf810e4 conflict fixed
```

在和远程分支同步后，我们对`hello.py`这个文件做了两次提交。用`git log`命令看看：

```
$ git log --graph --pretty=oneline --abbrev-commit
* 582d922 (HEAD -> master) add author
* 8875536 add comment
* d1be385 (origin/master) init hello
*   e5e69f1 Merge branch 'dev'
|\  
| *   57c53ab (origin/dev, dev) fix env conflict
| |\  
| | * 7a5e5dd add env
| * | 7bd91f1 add new env
...
```

注意到Git用`(HEAD -> master)`和`(origin/master)`标识出当前分支的HEAD和远程origin的位置分别是`582d922 add author`和`d1be385 init hello`，本地分支比远程分支快两个提交。

现在我们尝试推送本地分支：

```
$ git push origin master
To github.com:michaelliao/learngit.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

很不幸，失败了，这说明有人先于我们推送了远程分支。按照经验，先pull一下：

```
$ git pull
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:michaelliao/learngit
   d1be385..f005ed4  master     -> origin/master
 * [new tag]         v1.0       -> v1.0
Auto-merging hello.py
Merge made by the 'recursive' strategy.
 hello.py | 1 +
 1 file changed, 1 insertion(+)
```

再用`git status`看看状态：

```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

加上刚才合并的提交，现在我们本地分支比远程分支超前3个提交。

用`git log`看看：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   e0ea545 (HEAD -> master) Merge branch 'master' of github.com:michaelliao/learngit
|\  
| * f005ed4 (origin/master) set exit=1
* | 582d922 add author
* | 8875536 add comment
|/  
* d1be385 init hello
```

输入命令`git rebase`：

```
$ git rebase
First, rewinding head to replay your work on top of it...
Applying: add comment
Using index info to reconstruct a base tree...
M	hello.py
Falling back to patching base and 3-way merge...
Auto-merging hello.py
Applying: add author
Using index info to reconstruct a base tree...
M	hello.py
Falling back to patching base and 3-way merge...
Auto-merging hello.py
```

输出了一大堆操作，到底是啥效果？再用`git log`看看：

```
$ git log --graph --pretty=oneline --abbrev-commit
* 7e61ed4 (HEAD -> master) add author
* 3611cfe add comment
* f005ed4 (origin/master) set exit=1
* d1be385 init hello
...
```

原本分叉的提交现在变成一条直线了！这种神奇的操作是怎么实现的？其实原理非常简单。我们注意观察，发现Git把我们本地的提交“挪动”了位置，放到了`f005ed4 (origin/master) set exit=1`之后，这样，整个提交历史就成了一条直线。rebase操作前后，最终的提交内容是一致的，但是，我们本地的commit修改内容已经变化了，它们的修改不再基于`d1be385 init hello`，而是基于`f005ed4 (origin/master) set exit=1`，但最后的提交`7e61ed4`内容是一致的。

这就是rebase操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

最后，通过push操作把本地分支推送到远程：

```
Mac:~/learngit michael$ git push origin master
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 576 bytes | 576.00 KiB/s, done.
Total 6 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To github.com:michaelliao/learngit.git
   f005ed4..7e61ed4  master -> master
```

再用`git log`看看效果：

```
$ git log --graph --pretty=oneline --abbrev-commit
* 7e61ed4 (HEAD -> master, origin/master) add author
* 3611cfe add comment
* f005ed4 set exit=1
* d1be385 init hello
...
```

远程分支的提交历史也是一条直线。

# 标签管理

## 创建标签

在Git中打标签非常简单，首先，切换到需要打标签的分支上：

```
$ git branch
* dev
  master
$ git checkout master
Switched to branch 'master'
```

然后，敲命令`git tag <name>`就可以打一个新标签：

```
$ git tag v1.0
```

可以用命令`git tag`查看所有标签：

```
$ git tag
v1.0
```

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

```
$ git log --pretty=oneline --abbrev-commit
12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
4c805e2 fix bug 101
e1e9c68 merge with no-ff
f52c633 add merge
cf810e4 conflict fixed
5dc6824 & simple
14096d0 AND simple
b17d20e branch test
d46f35e remove test.txt
b84166e add test.txt
519219b git tracks changes
e43a48b understand how stage works
1094adb append GPL
e475afc add distributed
eaadf4e wrote a readme file
```

比方说要对`add merge`这次提交打标签，它对应的commit id是`f52c633`，敲入命令：

```
$ git tag v0.9 f52c633
```

再用命令`git tag`查看标签：

```
$ git tag
v0.9
v1.0
```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

```
$ git show v0.9
commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:56:54 2018 +0800

    add merge

diff --git a/readme.txt b/readme.txt
...
```

可以看到，`v0.9`确实打在`add merge`这次提交上。

还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

```
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```

用命令`git show <tagname>`可以看到说明文字：

```
$ git show v0.1
tag v0.1
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 22:48:43 2018 +0800

version 0.1 released

commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

diff --git a/readme.txt b/readme.txt
```

## 操作标签

如果标签打错了，也可以删除：

```
$ git tag -d v0.1
Deleted tag 'v0.1' (was f15b0dd)
```

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令`git push origin <tagname>`：

```
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
 * [new tag]         v1.0 -> v1.0
```

或者，一次性推送全部尚未推送到远程的本地标签：

```
$ git push origin --tags
Total 0 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
 * [new tag]         v0.9 -> v0.9
```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

```
$ git tag -d v0.9
Deleted tag 'v0.9' (was f52c633)
```

然后，从远程删除。删除命令也是push，但是格式如下：

```
$ git push origin :refs/tags/v0.9
To github.com:michaelliao/learngit.git
 - [deleted]         v0.9
```

要看看是否真的从远程库删除了标签，可以登陆GitHub查看。