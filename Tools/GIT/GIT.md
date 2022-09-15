简介
===
  本文参考[GIT ](https://git-scm.com/book/zh/v2)官方教程[Book](https://git-scm.com/)。

# 常用命令
```shell
git config --global user.name <name>
git config --global user.email <email>

git clone <repo_url>
git clone <repo_url> <local_folder>
git clone -b <branch_name> <repo_url>
git clone --branch <tag_name> <repo_url>

git add file1 file2 file3...
git add -A
git commit -m "log"//提交备注信息

git status//显示状态
git log

git branch                                       //列出本地分支
git branch -a                                   //列出本地和远程分支
git branch new_branch_name        //新建本地分支
gti checkout branch_name             //切换本地分支
git checkout -b new_branch_name//切换到新建本地分支

git push origin local_branch_name:remote
git pull origin remote_branch_name:local

git reset branch_name
git reset --hard branch_name//全删
git fetch origin
git pull/git fetch origin,git merge remote_branch_name
git diff//显示修改，区分没有add暂存区
git rm files//删除文件
git checkout HEAD --file //取消文件修改
git reset HEAD --file//跟add相反
reset HEAD//退回工作区
clean -f//删除不需要
git init
cd(change directory) 切换目录 cd ..返回上一级目录
ls(list) 列出当前目录所有文件

git cherry-pick [commit id]：只合并另一个分支的某次commit，如果有冲突，解决后执行git cherry-pick --continue即完成
git cherry-pick [commit id1..commit id100]：合并commit id1（不包括）到commit id100
git cherry-pick [commit id1^..commit id100]：合并commit id1（包括）到commit id100

git config --global user.name/email显示当前用户名和邮箱
git config --global user.name/email name/email设置当前用户名和邮箱
git config --global credential.helper store保存账户密码
git remote add/rm origin repo-URL绑定/解除本地和远程仓库
git push origin --delete remote_branch _name

git rebase -i HEAD~n             //查看最近的n条commit记录，将要修改的commit前的pick改为edit
git commit --amend  --author="name <email>"             //接着修改作者名和邮箱
git commit --amend               //接着修改提交记录
git rebase --continue              //完成修改

git reset HEAD^             //撤销上一次commit
git reset --soft HEAD^             //撤销上一次commit，但是文件add到工作区
```

## 克隆（clone）

## 状态（status）

## 添加（add）

## 提交（commit）

## 日志（log）
```shell
git log --pretty=oneline <file_name> // 列出文件所有提交记录
```

## 显示（show）
```shell
git show <commit_id/tag> // 显示某次提交或标签的修改内容
```

## 标签（tag）
### 打标签
#### 轻量标签（lightweight）
  轻量标签是对某个特定提交的引用，将提交校验和存储到一个文件中——没有保存任何其他信息。
  使用git tag命令创建轻量标签。
```shell
$ git tag v1.4-lw
$ git tag
v0.1
v1.3
v1.4
v1.4-lw
v1.5
```

  使用git show命令，可以看到与之对应的提交信息。
```shell
$ git show v1.4-lw
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number
```

#### 附注标签（annotated）
  附注标签是存储在git数据库中的一个完整对象，可以被校验，其中包含打标签者的名字、电子邮件地址、日期时间，此外还有一个标签信息，并且可以使用GNU Privacy Guard（GPG）签名并验证。
  使用git tag命令时，指定-a选项指定创建的是附注标签，-m选项指定附注信息。

```shell
$ git tag -a v1.4 -m "my version 1.4"
$ git tag
v0.1
v1.3
v1.4
```

  使用git show命令，可以看到标签信息和与之对应的提交信息。
```shell
$ git show v1.4
tag v1.4
Tagger: Ben Straub <ben@straub.cc>
Date:   Sat May 3 20:19:12 2014 -0700

my version 1.4

commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number
```

  上面两个默认是为当前的commit打标签，也可以在最后指定commit id来为以前的提交打标签。
```shell
$ git tag -a v1.2 9fceb02

$ git tag
v0.1
v1.2
v1.3
v1.4
v1.4-lw
v1.5

$ git show v1.2
tag v1.2
Tagger: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Feb 9 15:32:16 2009 -0800

version 1.2
commit 9fceb02d0ae598e95dc970b74767f19372d61af8
Author: Magnus Chacon <mchacon@gee-mail.com>
Date:   Sun Apr 27 20:43:35 2008 -0700

    updated rakefile
...
```

  如果要重命名，使用git tag \<newtag> \<oldtag>命令实现。

### 列出标签
  输入git tag即可，以字母顺序列出标签。
```shell
$ git tag
v1.0
v2.0
```

  还可以使用参数-l或--list，按特定的模式查找标签。
```shell
$ git tag -l "v1.8.5*"
v1.8.5
v1.8.5-rc0
v1.8.5-rc1
v1.8.5-rc2
v1.8.5-rc3
v1.8.5.1
v1.8.5.2
v1.8.5.3
v1.8.5.4
v1.8.5.5
```

### 推送标签
  默认情况下git push命令不会推送标签到远程，需要显示地推送。
```shell
$ git push origin v1.5
Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
Total 14 (delta 3), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.5 -> v1.5
```

  也可以使用--tags选项一次性推送很多标签，会将所有不在远程仓库服务器上的标签全部传送到哪里。
```shell
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.4 -> v1.4
 * [new tag]         v1.4-lw -> v1.4-lw
```

### 删除标签
  使用如下命令删除一个本地的轻量标签。
```shell
$ git tag -d v1.4-lw
Deleted tag 'v1.4-lw' (was e7d5add)
```

  如果要删除远程仓库的标签，可以使用git push \<remote> :refs/tag/\<tagname>命令，将冒号前面的空值推送到远程标签名，从而高效地删除它。
```shell
$ git push origin :refs/tags/v1.4-lw
To /git@github.com:schacon/simplegit.git
 - [deleted]         v1.4-lw
```

  也可以使用下面的命令，更直观的删除远程标签。
```shell
$ git push origin --delete <tagname>
```

## 拉取（push）和推送（pull）分支

## 补丁（patch）

# 问题
* file name too long
  [如何解决Windows中git “file name too long”错误？](http://www.360doc.com/content/21/0331/15/41344223_969938758.shtml)

参考
===
* [git查看某个文件的修改历史及具体修改内容](https://blog.51cto.com/u_15127605/4788522)
* [git clone 使用账号密码 windows](https://blog.csdn.net/sunbrightness/article/details/122304775)
* [使用 git 创建补丁和打补丁](https://blog.csdn.net/weixin_44794688/article/details/123333252)