# Git

**git和svn的区别：**

- git是分布式版本控制系统，不需要中央服务器，svn是集中式版本控制系统，需要中央管理服务器。
- git在本地就存在一个完整的版本库，可以不需要网络就进行开发，有网的时候就进行代码上传即可，而svn需要在中央服务器中拉取代码进行开发，必须联网，对网络的带宽要求高。
- 在进行开开发时，上传代码之后可以很明显的看到别人修改的代码，git可以直接看到更新了哪些代码和文件。
- 由于git在本地存在版本也导致可能会存在一些安全隐患。

## 1、版本控制

> 版本控制

版本控制是一种在开发的过程之中用于管理我们的文件、目录或工程等内容的修改历史信息，可以方便我们查看历史记录，对之前的版本进行备份，可以在代码丢失后进行回复的软件工程技术。

- **管理多人协同开发项目**

- 追踪和挤在一个多个文件的历史记录
- 组织和保护源代码与文件

- 可以统计工作量

- 并行开发、提高开发效率

- 跟踪记录整个软件的开发过程
- 减轻开发人员的负担，节省时间，降低人为错误。

## 2、git的使用

git分四层管理代码。

- 1、你目录中的文件是第一层

- 2、缓存区，每次add之后，当前目录中要追踪的文件会作为一个版本会存放在缓存区。注意不是所有的文件。一般一个文件生成之后，会标记为“未追踪”，但是否对其做版本管理还是要选择的。例如一些编译文件就没有必要追踪。对需要做版本管理的问件，用add添加，不需要的用clean删除。

- 3、本地仓库，每次commit之后，缓存区最新的版本就会存放在本地仓库。这里要提及一个HEAD的概念。HEAD是当前的版本指向，每次更新或者回退都会修改HEAD的指向，但对仓库中每一个版本并不会删除。所以即使回退到过去还是有机会回到现在的版本的

- 4、远程仓库，每次push之后，会将本地仓库中HEAD所指向的版本存放到远程仓库

### 2.1、git命令

![image-20200929213220432](/home/tan/桌面/Typora文件/Typora图片/Git/image-20200929213220432.png)

### 2.2、本地仓库信息设置

````
git config --global user.name "xxx"  # 名称
git config --global user.email "xxx" # 邮箱
````

### 2.3、git代码测试

````
#1.git status  查看仓库当前的状态
	#查看本地目录和仓库之间是否同步
````

````
#2.git diff    查看具体修改的内容 
	#different,不同，只能查看add之前本地文件的修改
````

### 2.4、版本回退

````
#1.查看提交日志
	git log
 	git log --pretty=oneline 
    #包括commit id,提交的用户，提交日期，提交日志
````

````
#2.回退版本
#HEAD指向的是最新版本，上一个版本HEAD^,上上一个版本HEAD^^,上到100个版本HEAD~100
#一般情况下，只有上一个版本使用HEAD^表示，其他的版本统统使用commit id
	#回退到上一个版本
  	git reset --hard HEAD^
    #回到某个具体的版本
    git reset --hard commitid   
    #回到未来的版本
    git reset --hard 未来的commitid
````

````
#3.查看git中执行过的命令
	git reflog
    
#问题说明：如果不知道版本号，可以通过git reflog查询
#回退版本之前，尽量先使用git log或者git log --pretty=oneline 查看历史版本
#要重返未来，用git reflog查看在git中执行过的命令
````

### 2.5、版本库/工作区/暂存区

````bash
演示命令：
tan@dark:~/Desktop/learngit$ touch file1.txt
tan@dark:~/Desktop/learngit$ vim file1.txt 
tan@dark:~/Desktop/learngit$ git status
位于分支 master
未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

	file1.txt

提交为空，但是存在尚未跟踪的文件（使用 "git add" 建立跟踪）
tan@dark:~/Desktop/learngit$ git add file1.txt 
tan@dark:~/Desktop/learngit$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	新文件：   file1.txt

tan@dark:~/Desktop/learngit$ vim text.txt 
tan@dark:~/Desktop/learngit$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	新文件：   file1.txt

尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     text.txt

tan@dark:~/Desktop/learngit$ git add text.txt 
tan@dark:~/Desktop/learngit$ git commit -m "modify text and crate file1"
[master 2eaebd1] modify text and crate file1
 2 files changed, 2 insertions(+)
 create mode 100644 file1.txt
tan@dark:~/Desktop/learngit$ git status
位于分支 master
无文件要提交，干净的工作区
````

### 2.7、撤销修改

**1) 改动了工作区，没有add，也没有commit**

````
#git checkout -- file1.txt  撤销工作区
````

````
演示命令：
tan@dark:~/Desktop/learngit$ vim file1.txt 
tan@dark:~/Desktop/learngit$ git status
位于分支 master
尚未暂存以备提交的变更：
（使用 "git add <文件>..." 更新要提交的内容）
（使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     file1.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
tan@dark:~/Desktop/learngit$ cat file1.txt 
ryeury
stupid boss
tan@dark:~/Desktop/learngit$ git checkout -- file1.txt
tan@dark:~/Desktop/learngit$ cat file1.txt 
ryeury
12345678910111213141516171819
````

**2) 改动了工作区，进行了add，但是还没有commit**

````
#1.git reset HEAD  file1.txt   撤销暂存区
#2.git checkout -- file1.txt   撤销工作区
````



````
演示命令：
tan@dark:~/Desktop/learngit$ vim file1.txt 
tan@dark:~/Desktop/learngit$ cat file1.txt 
ryeury
stupid boss
tan@dark:~/Desktop/learngit$ git add file1.txt 
tan@dark:~/Desktop/learngit$ git status
位于分支 master
要提交的变更：
（使用 "git reset HEAD <文件>..." 以取消暂存）

	修改：     file1.txt

tan@dark:~/Desktop/learngit$ git reset HEAD file1.txt
重置后取消暂存的变更：
M	file1.txt
tan@dark:~/Desktop/learngit$ git status
位于分支 master
尚未暂存以备提交的变更：
（使用 "git add <文件>..." 更新要提交的内容）
（使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     file1.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
tan@dark:~/Desktop/learngit$ git checkout -- file1.txt
tan@dark:~/Desktop/learngit$ cat file1.txt 
ryeury
12345678910111213141516171819202122232425262728293031
````

**3) 改动了工作区，进行了add，也进行了commit**

````
#git reset --hard HEAD^     回退版本
````

````
演示命令：
tan@dark:~/Desktop/learngit$ vim file1.txt 
tan@dark:~/Desktop/learngit$ git add file1.txt 
tan@dark:~/Desktop/learngit$ git commit -m "boss"
[master 07714d4] boss
1 file changed, 1 insertion(+)
tan@dark:~/Desktop/learngit$ git reset --hard HEAD^
HEAD 现在位于 41decc7 222
tan@dark:~/Desktop/learngit$ cat file1.txt 
ryeury
123456789101112
````

### 2.8、删除文件

删除文件对于git而言，被认为是一个修改

````bash
#1.删除工作区中的文件
	rm -rf filename
#2.删除版本库中的文件
	git rm filename
#3.将删除的修改提交到版本库
	git commit -m "xxx"
#4.查看版本库的状态
	git status
````

````bash
演示命令：
tan@dark:~/Desktop/learngit$ rm file1.txt 
tan@dark:~/Desktop/learngit$ ls
text.txt
tan@dark:~/Desktop/learngit$ git status
位于分支 master
尚未暂存以备提交的变更：
（使用 "git add/rm <文件>..." 更新要提交的内容）
（使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	删除：     file1.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
tan@dark:~/Desktop/learngit$ git rm file1.txt
rm 'file1.txt'
tan@dark:~/Desktop/learngit$ git status
位于分支 master
要提交的变更：
（使用 "git reset HEAD <文件>..." 以取消暂存）

	删除：     file1.txt

tan@dark:~/Desktop/learngit$ git commit -m "delete file1.txt"
[master aabf263] delete file1.txt
1 file changed, 1 deletion(-)
delete mode 100644 file1.txt
tan@dark:~/Desktop/learngit$ git status
位于分支 master
无文件要提交，干净的工作区
tan@dark:~/Desktop/learngit$ 
````

**需求：在工作区中删除了文件，误删了，要恢复**

````bash
演示命令：
tan@dark:~/Desktop/learngit$ touch newfile.txt
tan@dark:~/Desktop/learngit$ vim newfile.txt 
tan@dark:~/Desktop/learngit$ ls
newfile.txt  text.txt
tan@dark:~/Desktop/learngit$ git status
位于分支 master
未跟踪的文件:
（使用 "git add <文件>..." 以包含要提交的内容）

	newfile.txt

提交为空，但是存在尚未跟踪的文件（使用 "git add" 建立跟踪）
tan@dark:~/Desktop/learngit$ git add newfile.txt 
tan@dark:~/Desktop/learngit$ git commit -m "create newfile"
[master da2afdd] create newfile
1 file changed, 1 insertion(+)
create mode 100644 newfile.txt
tan@dark:~/Desktop/learngit$ git status
位于分支 master
无文件要提交，干净的工作区
tan@dark:~/Desktop/learngit$ rm newfile.txt 
tan@dark:~/Desktop/learngit$ git checkout -- newfile.txt     #丢弃工作区的改动
tan@dark:~/Desktop/learngit$ ls
newfile.txt  text.txt
````

## 3、与github进行连接

**前提：有github账号。**

### 3.1、连接github

- 创建github账号
- 将本地仓库和远程仓库连接起来

```
1>生成ssh key 【建立了本机和远程服务器之间的连接】
	ssh-keygen   -t   rsa   -C  "git注册邮箱地址"
2>验证ssh key是否添加成功
	ssh -T git@github.com
```

- 在github中创建空远程仓库

### 3.2、添加远程仓库

- 建立了本地仓库和远程仓库之间的连接

````
#origin表示的是远程仓库中的主分支，对应的是本地仓库中的master，origin可以是别的名称，但是建议使用origin

git remote add origin git@github.com:用户名/远程仓库名.git  #远程仓库名最好和本地仓库名保持一致
````

- 将本地仓库中的内容推送到远程仓库

````
git push -u origin master
````

- 修改工作区中的内容，然后和远程仓库之间进行同步

```
git add
git commit
git push origin master
```

### 3.3、克隆

- 在github中创建一个仓库，含有一个readme.md文件

- 从github将远程仓库克隆到本地指定的路径下

````
git clone git@github.com:用户名/c远程仓库的名称.git
````

### 3.4、创建分支和合并分支

master：主分支，其他的都被称为xx分支

master分支是一条线，git用master指向最新的提交，再用HEAD指向master

````bash
#1.创建子分支并切换到子分支
	git checkout -b xxx
#2.在子分支下进行工作
	git add
	git commit
#3.查看当前正在工作的分支
	git branch 
#4.切换到主分支
	git checkout master
#5.将子分支合并到主分支
	git merge xxx
#6.删除子分支
	git branch -d xxx  
````

> 注意问题

1.当第一次创建子分支并且需要切换，添加-b选项

````
其他时候只需要切换分支，使用git checkout 分支名
````

2.想让主分支上内容和子分支上的内容同步，则需要将子分支合并到主分支

3.只有当确认子分支没用的时候，就可以删除子分支

4.推荐大量使用分支，尽量不要在master上直接开发

## 4、问题

待补充。。。