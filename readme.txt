1.git init
2.git add <document>
3.git commit -m "name"
4.git reset --hard HEAD^ 返回上一个版本的修改
5.git clone "git address"

可以上传到github上。
PS ： 在第一次用时，需要创建一个SSH key。

上传到github的地址是： https://github.com/roger0960/learngit

第一次与github的地址建立连接。
git remote add origin https://github.com/roger0960/learngit.git

之后就推送更新到自己的github
git push -u origin master

第二次推送到自己的github
直接： git push -u origin master，就可以完成github的更新了

推送的github地址可以在 .git/config的文件下看到。

git branch
git checkout <name>
git merge <name>
git branch -d <name>

<new>
