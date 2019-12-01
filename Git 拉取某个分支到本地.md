## 方法一
1. 与origin master建立连接：`git remote add origin git@github.com:XXXX/nothing2.git`

2. 切换到其中某个子分支：`git checkout -b dev origin/dev`

> 可能会报这种错误：  
> fatal: Cannot update paths and switch to branch 'dev' at the same time.
> Did you intend to checkout 'origin/dev' which can not be resolved as commit?
> 原因是你本地并没有dev这个分支，这时你可以用git branch -a 命令来查看本地是否具有dev分支

3. 命令来把远程分支拉到本地：`git fetch origin dev`

4. 在本地创建分支dev并切换到该分支：`git checkout -b dev origin/dev`

5. 最后使用：`git pull origin dev`就可以把某个分支上的内容都拉取到本地了
    
## 方法二
使用Git下载指定分支命令
**`git clone -b 分支名 https://git.oschina.net/oschina/android-app.git`**

> -b表示要从分支下载，v2.8.1就是具体的某个分支的名称