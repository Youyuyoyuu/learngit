# 1. 版本库



## 1.1 创建版本库

> 1. 创建空目录`dir`
>
> 2. `git init`将目录变成git可管理的版本库（仓库）
>
>    此时自动添加了`.git`目录



## 1.2 添加文件

> 1. 编写一个文件readme.txt
>
> 2. `git add file_name`把文件添加到仓库
>
> 3. `git commit -m 'description'`把文件提交到仓库，并添加说明
>
>    （可多次add，仅需一次commit）



# 2. 时光机穿梭



## 2.1 修改文件

`git status`查看仓库状态，修改后会显示已修改且未提交，添加后显示需要提交的修改，提交后显示无需要提交的修改

`git diff`查看修改内容

修改后添加并提交，方法同**1.2**



## 2.2 版本回退

`git log`查看提交日志，从最近到最远，可以增加`--pretty=oneline`参数整合为一行显示，修改说明前为16进制的版本号

`HEAD`为当前版本的指针，`HEAD^`为前版本，`HEAD^^`为前前版本，`HEAD-100`为上100个版本

`git reset --hard HEAD^`回退前一版本

`git reset --hard commit_id`回退至版本号对应版本

`git reflog`查看每一次命令，可用于查看已被回退的版本号



## 2.3 工作区和缓存区

工作区(working directory)：可见的目录

版本库(repository)：隐藏目录`.git`，其中包括暂存区'stage'和自动创建的分支'master'

`git add`：将文件从工作区添加到暂存区

`git commit`：将文件从暂存区提交到仓库中`HEAD`所指分支



## 2.4 管理修改

git管理的是修改

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

仅提交了第一次修改

`git diff HEAD -- file_name`查看工作区和仓库最新版本的差别



## 2.5 撤销修改

`git checkout -- file_name`撤销文件在工作区的全部修改，使之退回到最后一次add或commit的状态

`git reset HEAD file_name`把暂存区的修改回退到工作区



## 2.6 删除文件

> 1. `rm file_name`删除文件
> 2. `git rm file_name`从版本库中删除文件
> 3. `git commit -m 'explanation'`提交至仓库

`git checkout -- file`把误删的文件恢复到仓库最新版本（其实就是用仓库里的最新版本替换工作区的版本）



# 3. 远程仓库



## 3.1 GitHub

> 1. 创建SSH Key。`ssh-keygen -t rsa -C 'email_adress'`，一路回车使用默认值，即创建完成`.ssh`目录及`id_rsa`、`id_rsa.pub`两个密钥文件，后者为公钥
> 2. 登陆GItHub，在'setting'的'SSH Keys' 界面将`id_rsa.pub`里的内容粘贴，即完成SSH加密，可以使用远程仓库



## 3.2 添加远程库

> 1. 在GitHub上创建repository，填入名称`repository_name`
>
> 2. 将本地仓库关联到GitHub的远程库。
>
>    `git remote add origin git@github.com:username/repository_name.git`
>
> 3. 将本地库的内容推送到远程库上。`git push -u origin master`。第一次使用时会出现SSH警告。

`git push origin master`此后将本地库推送至GitHub

`git remote -v`查看远程库信息

`git remote rm origin`删除origin远程库



## 3.3 从远程仓库克隆

> 1. 在GitHub上创建repository，勾选'Add a README file'，自动创建文件
> 2. 克隆至本地库。`git clone git@github.com:username/repository_name.git`



# 4. 分支管理



## 4.1 创建与合并分支

> 1. 创建并切换到分支。`git switch -c branch_name`
> 2. 修改后添加并提交，方法同**1.2**。
> 3. 切换回master分支。`git switch master`
> 4. 把新分支修改的内容快速合并到master分支上。`git merge branch_name`
> 5. 删除新分支。`git branch -d dev`

`git branch`查看当前分支

*注意：merge是将目标分支内容合并到当前分支*



## 4.2 解决冲突

当两个分支存在冲突时，不可自动进行快速合并

> 1. 直接查看文件内容并做修改，文件中会标记出不同分支的内容差别。
> 2. 添加并提交，方法同**1.2**。

`git log`查看分支的合并情况

`git log --graph`查看分支合并情况并显示分支合并图



## 4.3 分支管理

在分支冲突的情况下，仍然可以直接进行合并

`git merge --no-ff -m 'description' branch_name`直接将目标分支合并到当前分支，`--no-ff`表示禁用快速合并，合并同时会创建一个新的提交，故需要描述



## 4.4 Bug分支

修复分支上的Bug时，可单独创建一个新的Bug分支

但在修复前，还需将当前工作储存

> 1. 储存当前工作。`git stash`
> 2. 恢复当前工作并删除stash。`git stash pop`
>    1. 多次stash后，可先用`git stash list`查看
>    2. 恢复指定stash。`git stash apply stash@{stssh_number}`
>    3. 删除stash。`git stash drop`

现在可在Bug分支上修复Bug

> 1. 切换至出现Bug的分支。`git switch branch_name`
> 2. 创建临时的Bug分支。`git switch -c bug_branch`
> 3. 修改后添加并提交，方法同**1.2**
> 4. 切换回出现Bug的分支。
> 5. 完成合并。`git merge --no-ff -m 'description' bug_branch`

若出现Bug的分支另有一分支也有同样的Bug，可用`git cherry-pick commit_id`，将指定提交复制到当前分支（也就是另一个同Bug分支），`commit_id`由之前提交时提供。

