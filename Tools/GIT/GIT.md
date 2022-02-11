# 常用命令
git config --global user.name <name>
git config --global user.email <email>

git clone <地址>
git clone -b <分支地址>

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