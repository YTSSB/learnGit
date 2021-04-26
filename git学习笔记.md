# git学习笔记

### 创建版本库

##### 新建文件夹 创建版本库
```c
$ mkdir learnGit				//创建文件夹
$ cd learnGit					//进入文件夹
$ pwd							//显示文件夹所在目录
/d/Linux/learnGit			
```
```c
$ git init						//进入该文件下，把目录变成Git可以管理的仓库
```

##### 把文件添加并提交到仓库
```c
$ git add readMe.txt
$ git comit -m "wrote a readMe file" //“”内信息为备注信息
```
##### 查看git当前状态
```c
$ git status					//提示哪些文件修改了没有添加或者提交
```
##### 查看文件变化
```c
$ git diff readMe.txt		    //diff为difference的缩写
```

### 时光穿梭机
#### 版本回退
```c
$ git log						//显示全部日志信息
$ git log --pretty=oneline		//显示单行的日志信息
$ git reflog					//如果已经退回到过去版本,想回到未来版本,可通过这个查找未来版本的id
```
```c
$ git reset --hard HEAD^		//退回到上一个版本
$ git reset --hard HEAD^^		//退回到上两个版本
$ git reset --hard HEAD~100		//退回到上100个版本
$ git reset --hard 1094a		//退回到commit id号为1094a的版本,其中commit id号可通过log查找
```
### 工作去和暂存区

![image-20210426102908893](C:\Users\ignoreY\AppData\Roaming\Typora\typora-user-images\image-20210426102908893.png)

![image-20210426103008289](C:\Users\ignoreY\AppData\Roaming\Typora\typora-user-images\image-20210426103008289.png)

![image-20210426103019145](C:\Users\ignoreY\AppData\Roaming\Typora\typora-user-images\image-20210426103019145.png)

### 撤销修改
##### 没有add,没有commit
```c
$ git checkout -- readMe.txt	//丢弃工作区的修改,“--”前后都有空格
```
##### 已经add,没有commit
```c
$ git reset HEAD readMe.txt		//撤销暂存区的修改
$ git checkout -- readMe.txt	//丢弃工作区的修改
```
##### 已经add,并且commit了
```c
$ git reset --hard HEAD^		//退回到上一个版本
```
### 删除文件
##### 仅删除本地文件
```c
$ rm test.txt
```
##### 同时删除本地文件和版本库中的文件
```c
$ git rm test.txt
```
##### 本地删错了，版本库李还有，用版本库替换工作区的版本
```c
$ git checkout -- test.txt		//“--”两边都有空格
```
### 远程仓库
##### 添加远程仓库
```c
/*YTSSB是GitHub的账户名,origin是远程库的名字，可以改成别的，但是origin一看就知道是远程库*/
$ git remote add origin git@github.com:YTSSB/learnGit.git  
```
##### 把本地库上的所有内容推送到远程库上
```c
/*把master分支推送到远程库上 -u把本地的master分支和远程的master分支关联起来，之后推送或拉去可省略-u*/
$ git push -u origin master
```
##### 删除远程库
```c
$ git remote 					//查看远程库及其信息
$ git remote -v					//查看远程库
/*删除远程库。本质是解除了本地和远程的绑定关系，并不是物理上的删除远程库*/
$ git remote rm origin	
```
##### 从远程库克隆
```c
$ git clone git@github.com:YTSSB/gitskills.git	//ssh协议，有些是http协议
$ git pull 						//把最新的提交从远程库中抓取下来并在本地合并
```
### 分支管理
##### 创建与合并分支
```c
$ git branch					//查看分支
$ git branch <name>				//创建分支
$ git branch -d <name>			//删除分支
$ git switch <name>				//切换分支
$ git switch -c name			//创建并切换到新的name分支
$ git merge <name>				//将分支name合并到当前分支
/*通常git会用Fast forward模式合并分支,这种模式删除分支会丢掉分支信息*/
$ git merge --no-ff -m "merge with no-ff" <name> //禁用Fast forward模式合并分支
$ git rebase 					//把本地未push的分叉整理成一条直线，使合并分支时更加明了
```
##### 解决冲突
```c
$ git log --graph --pretty=oneline --abbrev-commit	//查看分支合并情况
```
##### bug分支
```c
$ git stash						//将当前的工作现场“储存”起来
$ git stash list				//查看stash内容
$ git stash pop					//解决bug后恢复现场，同时吧stash内容删除
$ git cherry-pick 4c805e2		//在dev上有相同的bug，把已经修复好的bug分支4c805e2做的修改复制								  到devs
```
### 标签管理
##### 创建标签
```c
$ git switch dev
$ git tag v1.0
$ git show v1.0
/*创建带有说明的标签 -a指定标签名,-m指定说明文字,1094abd指定的被创标签的commit id号*/
$ git tag -a v0.1 -m "v0.1 released" 1094adb	
```
##### 操作标签
```c
$ git tag -d v0.1				//删除本地标签
$ git push :refs/tags/v0.9		//删除本地标签之后，在push，删除远程库中的标签
$ git push origin v1.0			//推送某个标签到远程库
$ git push origin --tags		//一次性推送所有尚未推送到远程的本地标签
```