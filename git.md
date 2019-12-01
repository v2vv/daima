Title         : git.md
Author        : You
Logo          : True

# 配置Git的全局设置

在安装完成之后，使用git config命令对Git进行全局设置：

``` javascript
git config --global user.name "your name" 
git config --global user.email "your email address"
```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。

# 克隆远程库

创建本地库，在本地创建一个文件夹，从远程库克隆一个本地库：

``` javascript
cd d:/GitTest   //指定存放的目录
git clone https://git.oschina.net/name/test.git   //你的仓库地址 
```

# 远程仓库相关命令 

``` javascript
检出仓库：        $ git clone git://github.com/jquery/jquery.git

查看远程仓库：$ git remote -v

添加远程仓库：$ git remote add [name] [url]

删除远程仓库：$ git remote rm [name]

修改远程仓库：$ git remote set-url --push [name] [newUrl]

拉取远程仓库：$ git pull [remoteName] [localBranchName]

推送远程仓库：$ git push [remoteName] [localBranchName]
```



# 常用指令

``` javascript
git add：是将当前更改或者新增的文件加入到Git的索引中，加入到Git的索引中就表示记入了版本历史中，这也是提交之前所需要执行的一步，例如'git add app/model/user.rb'就会增加app/model/user.rb文件到Git的索引中，该功能类似于SVN的add

git rm：从当前的工作空间中和索引中删除文件，例如'git rm app/model/user.rb'，该功能类似于SVN的rm、del

git commit：提交当前工作空间的修改内容，类似于SVN的commit命令，例如'git commit -m story #3, add user model'，提交的时候必须用-m来输入一条提交信息，该功能类似于SVN的commit

git remote add origin：关联到远程库

git pull：从其他的版本库（既可以是远程的也可以是本地的）将代码更新到本地，例如：'git pull origin master'就是将origin这个版本库的代码更新到本地的master主枝，该功能类似于SVN的update

git push：将本地commit的代码更新到远程版本库中，例如'git push origin'就会将本地的代码更新到名为orgin的远程版本库中

git log：查看历史日志，该功能类似于SVN的log

git revert：还原一个版本的修改，必须提供一个具体的Git版本号，例如'git revert bbaf6fb5060b4875b18ff9ff637ce118256d6f20'，Git的版本号都是生成的一个哈希值

```


![git指令](http://qiniu.v2vv.cn/359884-20171128123923050-1074438610.png)