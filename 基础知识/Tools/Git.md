# 常用命令



基本操作： 
git init                                     把文档结构变成可管理的本地仓库
git add                   添加可管理的文件
git commit -m '提交记录信息'         把本地文件提交到本地仓库
git status                  查看文件状态
git clone <url>                 					git克隆
git log                        可以显示所有提交过的版本信息
git reflog                  可以查看所有分支的所有操作记录（包括commit和reset的操作），包括已经被删除的commit记录，git log则不能察看已经删除了的commit记录



git rm<filename>       删除一个文件，作用类似手动删除在提交
git reset --hard HEAD^    回退文档状态到上一个状态 
git reset --hard HEAD^^      回退文档到上上一个状态
git reset --hard HEAD~100   回退文档到上100个状态
git reset --hard       查看文挡版本号
git reset --hard 6fcffc8 9     回退文档到指定版本号的状态
git checkout --filename     回退指定文档至未修改状态（撤销工作区修改）





ssh-keygen -t rsa -C "youremail@example.cm"  创建SSH key


git remote add orgin http://github/XXXXXXXXXXXX 关联远程仓库


git push orgin master          向远程仓库推送  注意：第一次推送时由于远程库是空的，我们第一次推送master分支时，加上了-u参数（git push -u origin master），Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。


git clone url                          克隆远程镜像
git remote           查看远程库的信息
gir remote -v           查看远程仓库详细信息



分支操作： 
git branch                          查看分支状态
git branch branch_name   创建分支
git checkout branch_name 切换到制定分值分支
git checkout -b branch_name  创建并切换至branch_name分支 
git merge branch_name    合并baanch_name分支到当前分支
git branch -d branch_name  删除指定名称的分支


拉取获取：
git fetch <远程主机名><远程分支名>:<本地分支名>    在本地新建一个分支，把远程库中的分支下载到本地分支中
git fetch origin master:tmp 
//在本地新建一个temp分支，并将远程origin仓库的master分支代码下载到本地temp分支

git pull  <远程主机名><远程分支名>:<本地分支名>   取回远程主机的某个分支，再与本地指定分支合并
git pull  orgin master b01    


注意：
 1，当远端有变化，本地库没有有变化，拉取操作远端会覆盖本地库（远端版本高于本地）
   2，当远端修改，本地库也有修改，拉取操作会产生冲突文件（远端版本和本地版本冲突）
 3，当本地库修改，而远端没有变化，拉取不会产生变化（本地版本高于远端版本）
 3，当本地库版本低于远端版本，则无法推送，必须先拉取在操作



标签：
git tag             查看标签
git tag<name>       切换到需要打标签的分支上,敲命令git tag <name>就可以打一个新标签
git tag <name> <commit id>    在指定的版本号上打标签
git show <tagname>                     查看指定标签的版本状态，commit id

git tag -d <tagname>                   删除指定标签
git push orgin --tags                    推送所有标签到远程仓库

设置Git的user name 和email

```
$ git config --global user.name "dengqiwei" // you name
$ git config --global user.email "itmandqw@163.com" //you email
```

配置完成，可以查看配置信息

```
$ git config --global  --list // 查看当前用户(global)配置
$ git config --system  --list // 查看系统config
$ git config --local  --list // 查看当前仓库配置信息
```

检查是否存在ssh  key

```
$ cd ~/.ssh
```

看一下有没有id_rsa和id_rsa.pub(或者是id_dsa和id_dsa.pub之类成对的文件)，有 .pub 后缀的文件就是公钥，另一个文件则是密钥。
假如没有这些文件，甚至连 .ssh 目录都没有，可以用 ssh-keygen 来创建。该程序在 Linux/Mac 系统上由 SSH 包提供，而在 Windows 上则包含在 MSysGit 包里。

生成密钥

```
$ ssh-keygen -t rsa -C "itmandqw@163.com"
```

直接按Enter就行。然后，会提示你输入密码，如下(建议输一个，安全一点，当然不输也行)：

```
Enter same passphrase again: [Type passphrase again]
```

完了之后，大概是这样：

Your public key has been saved in /home/you/.ssh/id_rsa.pub.The key fingerprint is: # 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db itmandqw@163.com



最后得到了两个文件：id_rsa和id_rsa.pub, 如果不是第一次，就选择overwrite。
到此为止，你本地的密钥对就生成了

.添加公钥（id_rsa.pub）到你的远程仓库（github、gitLab等)

查看你的公钥：cat ~/.ssh/id_rsa.pub

