
在一台未部署git的电脑上，上传（push）代码的步骤：
（这个办法一定很笨，但是这是目前我实践成功的方法。希望之后能发现聪明的解法吧）

1、将已经存在于github上的代码下载到本地电脑上，进行解压。
删除 .git 文件

mac下删除步骤：
`ls -a`  //查看所有以及该目录下的隐藏文件
`rm -rf .git`  //强制删除 .git文件

2、开始上传push
`git init` //初始化，把这个目录变成Git可以管理的仓库

`git add .` //不但可以跟单一文件，还可以跟通配符，更可以跟目录。一个点就把当前目录下所有未追踪的文件全部add了 （`git add *`也是同样的效果）

`git commit -m "again"` //把文件提交到仓库，添加commit为again
Reinitialized existing Git repository in /Users/edrain/learngit/.git/  //提示这句是已经初始化成功了

`git remote add origin git@github.com:edrainann/learn_testing.git`  //关联远程仓库

`git push -u origin master` //把本地库的所有内容推送到远程库上

------
参考：
https://www.jianshu.com/p/8d26730386f3
https://blog.csdn.net/ivy_123ivy/article/details/51822741
https://blog.csdn.net/shiren1118/article/details/7761203
