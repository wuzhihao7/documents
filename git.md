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

