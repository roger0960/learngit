
git工具的使用
以文件readme.txt文件为
$ touch readme.txt
$ git init	--> 把当前所处的目录设置成git的管理仓库
			--> 成功创建之后，可以在当前目录下看到一个 .git的隐藏文件夹
$ git status	--> 查看当前的状态
$ git add readme.txt	--> 把文件添加到暂存区
$ git commit -m "description"	--> 把当前暂存区的所有内容都提交到仓库上，description是指对当前版本的一个说明

-----------------------------------------------------------------------------
$ git log	--> 查看提交过的历史记录
			--> 并且显示作者、时间、description等内容
$ git log --pretty=onelibe	--> 只显示版本的commit id
$ git relog --> 查看执行过的命令记录

-----------------------------------------------------------------------------
$ git reset --hard HEAD^	--> 恢复上一版本的内容
$ git reset --hard <commit id>	--> 恢复对应commit id的版本
$ git diff readme.txt	--> 查看工作区与仓库里面最新版本的区别
$ git diff HEAD -- readme.txt	--> 同上
$ git checkout -- readme.txt	--> readme.txt在工作区就直接撤销全部的修改
							--> readme.txt在暂存区，就把暂存区的readme.txt撤销
$ git rm readme.txt	--> 从仓库上，把文件删除
					--> 会出现提示 使用 --cached 保留本地文件，或用 -f 强制删除
					
-----------------------------------------------------------------------------
添加到远程仓库（github为例）
第一次使用远程仓库。
第1步：创建SSH Key。
$ ssh-keygen -t rsa -C "youremail@example.com"
	--> 如果成功执行，会在主目录下生成一个 .ssh文件夹
	--> 里面有id_rsa和id_rsa.pub两个文件
	--> 这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥
第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面
	然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容
	点“Add Key”，你就应该看到已经添加的Key


$ git remote add origin https://github.com/roger0960/learngit.git	
	--> 与自己的远程仓库建立连接，初次建立需要。
$ git push -u origin master	--> 把本地仓库master分支上的内容推送到远程仓库。
							--> 第一次推送master分支时，加上了-u参数

$ git push origin master	--> 之后提交master分支上的内容

如果要改变远程仓库的地址
$ git remote rm origin
$ git remote add origin <git address>	--> 与初次建立连接，推送的方法一样

$ git clone <git address>	--> 从把远程仓库的内容下载到本地

-----------------------------------------------------------------------------
$ git checkout -b dev	--> 创建dev分支，此时远程库并没有dev分支，-b表示创建并切换
上面这条命令相当于以下两条
$ git branch dev		--> 创建dev分支
$ git checkout dev		--> 跳转到dev的分支
$ git branch			--> 查看当前分支的情况，标 * 的为当前分支
$ git checkout master	--> 切换到master分支
$ git merge dev			--> 把dev分支的工作结果合并到当前 master分支上（Fast forward模式）
$ git checkout -d dev	--> 合并完成后就可以放心删除dev分支了（强行删除 -D）

当出现分支合并冲突时，需要手动处理出现的冲突问题

$ git log --graph --pretty=oneline --abbrev-commit	--> 查看分支历史
$ git merge --no-ff -m "merge with no-ff" dev	--> 不使用Fast forward模式
												--> 这一次合并会创建新的commit，因此加上-m

$ git stash	--> 把当前工作现场“储藏”起来
$ git stash apply	--> 恢复statsh
$ git stash drop	--> 删除

$ git stash pop		--> 恢复并删除

-----------------------------------------------------------------------------
远程仓库的默认名称是origin
$ git remote	--> 查看远程仓库的信息
$ git remote -v	--> 显示更详细的远程仓库的信息

$ git push orgin master --> 推送master分支上的内容
$ git push orgin dev --> 推送dev分支上的内容

多人协作的工作模式通常是这样：
1.首先，可以试图用git push origin <branch-name>推送自己的修改；
2.如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3.如果合并有冲突，则解决冲突，并在本地提交；
4.没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！
如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

-----------------------------------------------------------------------------
标签就是取代commit id这一串复杂的数字，更通俗易懂	--> v1.0对应的标签名字
$ git tag v1.0	--> 打标签，默认标签是打在最新提交的commit上
$ git tag v1.0 f52c633	--> 给指定的commit id版本打标签
$ git tag			--> 查看所有的标签
$ git show v1.0	--> 查看标签的信息

$ git tag -d v0.1	--> 删除本地标签
$ git push origin v1.0	--> 推送指定标签到远程

$ git push origin :refs/tags/<tagname>	--> 可以删除一个远程标签

-----------------------------------------------------------------------------
$ git stash save "save message"  --> 执行存储时，添加备注，方便查找，只有git stash 也要可以的，但查找时不方便识别。
$ git stash list --> 查看stash了哪些存储
$ git stash show --> 显示做了哪些改动，默认show第一个存储,如果要显示其他存贮，后面加stash@{$num}，比如第二个 git stash show stash@{1}
$ git stash show -p --> 显示第一个存储的改动，如果想显示其他存存储，命令：git stash show  stash@{$num}  -p ，比如第二个：git stash show  stash@{1}  -p
$ git stash apply --> 应用某个存储,但不会把存储从存储列表中删除，默认使用第一个存储,即stash@{0}，如果要使用其他个，git stash apply stash@{$num} ， 比如第二个：git stash apply stash@{1}
$ git stash pop --> 命令恢复之前缓存的工作目录，将缓存堆栈中的对应stash删除，并将对应修改应用到当前的工作目录下,默认为第一个stash,即stash@{0}，如果要应用并删除其他stash，命令：git stash pop stash@{$num} ，比如应用并删除第二个：git stash pop stash@{1}
$ git stash drop stash@{$num} --> 丢弃stash@{$num}存储，从列表中删除这个存储
$ git stash clear --> 删除所有缓存的stash

-----------------------------------------------------------------------------
$ git commit --amend --> 覆盖自己本地仓库的最新的一次提交
$ git rebase -i 9ce58bdd9c5ee9bdedf8c57890ba5dadfb390363 --> 合并 9ce58bdd9c5ee9bdedf8c57890ba5dadfb390363 这个commit之前的所用提交
$ git rebase -i HEAD~2 --> 压缩合并最近两次提交


















