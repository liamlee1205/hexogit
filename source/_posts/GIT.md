# GIT-By LJY

廖雪峰-git教程-个人笔记，教程链接：

 https://www.liaoxuefeng.com/wiki/896043488029600 



## 一、基操



1. 创建版本库目录 

   ```
   mkdir <file>
   ```

    然后

   ```
   cd <file>
   ```

    生成 .git 目录（ls -ah查看）

   ```
   git init
   ```

2.   添加到仓库

   ```
   git add <file>
   ```

3.  提交到仓库，注意加注释

   ```
   git commit -m “message” 
   ```

## 二、版本回退

1. ```
   git status
   ```

    查看当前状态、建议在git commit 前查看

2. ```
   git diff <file>
   ```

    查看修改

3. ```
   git log
   ```

    显示提交日志（可选参数 --pretty=oneline ）,commit id很复杂。

4. HEAD表示当前版本，后面接一个^表示版本-1，^^ 为-2， HEAD~n表示往上n个版本

   1. ```shell
      $ git reset --hard HEAD^
      HEAD is now at e475afc add distributed
      ```

      --hard

5. 回滚后，若想回到当前版本之后的版本，需要commit id版本号。

   ```
   git reset --hard 1094a
   ```

6. 查找历史命令，可查看版本号。

   ```
   git reflog
   ```

## 三、工作区和暂存区

​		1. 工作区即本地可见目录，mkdir创建的目录。

​		2. 暂存区（stage）

![git add 操作图](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119151835999.png)

​		

  3. ```
     git diff HEAD -- readme.txt
     ```

     用于查看工作区和版本库中最新版本的区别

 4. ```
    git checkout -- <file>
    ```

    撤销工作区<file>的修改，撤回到暂存区版本（上一次是add）/版本库版本(上一次是commit)。

    ```
    git reset HEAD <file>
    ```

    撤销暂存区<file>的修改，撤回到工作区。

	5. 回退小结![image-20191119154400998](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119154400998.png)

6. 删除

   ```
   git rm <file>
   git commit -m "rm <file>"
   ```

   恢复

   ```
   git checkout -- <file>
   ```

    git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。 

## 四、远程仓库

1. 在本地git仓库运行

   ```
   git remote add origin git@github.com:username/learngit.git
   ```

   username注意更改，远程仓库名称 origin

2. 本地库当前分支推送到远程库

   ```
   git push -u origin master
   ```

   -u 用于第一次关联本地master与远程master分支，之后可以省略。

   ```
   git push origin master
   ```

3. ```
   git clone 地址
   ```

   ![image-20191119161222891](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119161222891.png)

   博客地址如图repo，可克隆public文件夹

## 五、分支管理

参考链接： https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424 

1. master

   ​							![image-20191119161858030](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119161858030.png)

   ​	master指向最新的提交，HEAD指向master

2. 新分支 ： dev

   ![image-20191119162128754](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119162128754.png)

   

   创建新指针dev，指向和master相同的提交，再把HEAD指向dev，表示当前分支为dev。

   之后针对工作区的修改与提交都针对dev，每次提交dev移动一步，而master保持不变。

   ![image-20191119162355339](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119162355339.png)

3. 合并分支

   直接将master指向dev，然后将HEAD指向master，可删除dev。

   ![image-20191119162517089](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119162517089.png)

![image-20191119162615390](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119162615390.png)

4. 相关命令

   ```
   git branch dev		//创建分支dev
   git checkout dev	//切换分支dev
   git checkout -b dev //上面两条命令的合并，创建与切换分支dev
   ```

   ```
   git branch //查看当前分支，带*
   ```

   然后是修改文件，add、commit命令，此时切回master并不会发生改变。

   ![image-20191119163553334](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119163553334.png)

   ```
   git merge dev 		//合并分支，此处一般要切回master再merge,暂未搞清楚。
   git branch -d dev   //删除分支
   ```

   针对 git checkout操作的优化，如今使用以下命令进行切换分支

   ```
   git switch -c dev //创建并切换
   git switch dev 	  //切换到已有
   ```

5. 解决冲突

   git status可以告知冲突的文件，冲突时，会在原文件中出现

   ![image-20191119164242508](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119164242508.png)

   ​											HEAD版本与featuer1版本冲突示例

   ```
   git log --graph //可查看合并分支图
   ```

   ```
   git merge dev //默认merge使用Fast forward,删除分支后会丢掉分支信息
   git merge --no-ff -m "merge with no-ff" dev  //可禁用Fast forward，保留分支信息
   ```

   分支策略：master不允许在上面直接工作（稳定版），master创建dev用于集体合并（集体开发），dev下创建个人分支用于干活（个人开发）。

   ![image-20191119165954998](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119165954998.png)

   PS:

   突然产生的问题，待实践

   创建分支，应该是在当前分支下创建吧？即需要切换到需要的，再在下面创建。

6. Bug分支(未看)

7. Feature分支

   开发新功能最好创建新分支

   ```
   git branch -D <name> //大D可以强行删除未合并分支，小d只能删除合并过的。
   ```

8. 本地分支与远程分支

   ![image-20191119171607605](C:\Users\weitianle\AppData\Roaming\Typora\typora-user-images\image-20191119171607605.png)

   ```
    git push origin master			//推送master
   ```

   ```
    git push origin dev			//推送分支dev
   ```

9. 协作开发

   其他人：

   ```
   git clone git@github.com:michaelliao/learngit.git //克隆仓库，分支为master
   ```

   ```
   git checkout -b dev origin/dev  //创建本地dev分支,并关联远程dev分支
   ```

   ```
   git add <file>
   git commit -m "message"
   git push origin dev
   ```

   你：

   ```
   git add <file>
   git commit -m "message"
   git push origin dev
   ```

   此时产生冲突

   ```
   git pull
   ```

