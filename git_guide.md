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
> 3. `git commit -m 'explanation'`把文件提交到仓库，并添加说明
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

`git push origin master`此后本地推送至GitHub

`git remote -v`查看远程库信息

`git remote rm origin`删除origin远程库



## 3.3 从远程仓库克隆

> 1. 在GitHub上创建repository，勾选'Add a README file'，自动创建文件
> 2. 克隆至本地库。`git clone git@github.com:username/repository_name.git`



# 4. 分支管理



## 4.1 创建与合并分支

> 1. 创建分支。
