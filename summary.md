# git

> 教程地址：廖雪峰 https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000；表严肃 https://www.bilibili.com/video/av17603446/
>
> 日期：2018年10月9日



## 基本使用

### 创建仓库

- 手动新建文件夹，进入文件夹后`cd test，用编辑器自带的teminal`git init`

  （VSCode终端开放方式ctrl+` ，或者【查看】-【集成终端】）

  上述操作可以直接在Git Bash中进行：`mkdir test`-`cd test`-`git init`

- 直接在根目录`git init test`

- 在github项目下复制项目地址，在teminal中输入`git clone https://...`



**注意**：Git作为版本跟踪系统，只能跟踪文本文件的改动，如TXT文件，网页，程序代码等

诸如图片、视频、Window系统下的Word，Git是无法跟踪它们的改动的

另外，不要使用Windows自带的记事本来编辑任何文本文件（因为记事本在编辑utf-8编码的文件时会在开头添加0xefbbbf）



### 常用命令

`git status` 查看仓库当前的状态

`git diff 文件名` 查看具体修改的内容

`git reflog` 记录每一次命令，从最近到最远

`git diff HEAD -- 文件名` 查看工作区和版本库里最新版本中对应文件的区别	



### 提交版本

修改文件后三步走

1. 保存文件（很重要）
2. `git add .` 将当前所有修改添加至暂存区，或者`git add 文件名`
3. `git commit -m "描述"` 提交版本



当文件修改到一定程度时，可以保存一个版本，称为***commit***；一旦把文件改乱了或是误删了文件，还可以从最近的一个commit恢复



### 查看历史版本

`git log` 查看历史版本，按从最近到最远的顺序显示

`git log --pretty=oneline` 更间接地显示历史版本

一大串数字是版本号，称为***commit id***，使用前7位就可以定位一个版本



### 回退版本

在git中用`HEAD`表示当前版本，即最新的版本，上一个就是`HEAD^`，上上一个就是`HEAD^^`，往上100个可以写成`HEAD~100`；

实际上HEAD是指向当前版本的一个**指针**，在回退版本时实际上仅仅是移动这个指针



回到上一个版本： `git reset --hard HEAD^` 

回到指定版本：

1. 用`git log`得到commit id
2.  `git reset --hard commit id`，commit id可以不需要写完整，前几位即可



**注意**：一旦回到某个版本后，在这个版本之后的所有版本（“未来版本”）都不会在`git log`中显示，即回退到的版本成为最新版本

如果回退之前有用`git log`查看过历史版本，那么还可以得到“未来版本”的版本号，再指定回到未来某个版本



### 工作区和暂存区

![工作区和暂存区](C:\Users\apple\Desktop\myproject\git\pic\工作区和暂存区.png)

​							（注：图片来自廖雪峰老师的网站）

**工作区**就是进行文件修改的目录，例如test文件夹；**版本库**是test文件夹下一个隐藏的文件夹.git

版本库中有：

- `stage/index`，暂存区
- `master`，Git自动创建的第一个分支
- `HEAD`，指向master分支的指针



在修改工作区中的文件后，需要进行三步走将修改的文件保存成一个新的版本

1. `git add` ，将修改的文件添加进暂存区，可以分多次添加

   ```
   git add 1.txt
   git add 2.txt 3.txt
   ```

   

2. `git commit`，将暂存区中的所有内容提交到当前分支



文件修改后未添加到暂存区：

```

```

文件修改后，将修改添加到暂存区

```
git status

...
Changes to be commited
	modified 文件名（绿色显示）
```



### 管理修改

git管理的是**修改**，例如插入一行，删除一行，修改一行中的几个字符，新建文件等等

如果按照这样的操作顺序：

第一次修改-`git add`-第二次修改-`git commit`，那么只把第一次修改的内容提交，第二次修改的内容没有被提交

可以把两次修改合并提交，或者第二次修改后再提交一次



### 删除/撤销修改

`git checkout -- 文件名`，撤销该文件在工作区内的修改

有如下**2种情况**

- 文件在上一次提交版本后，只进行了自身修改，还没有将任何修改添加到暂存区，那么checkout后回到最新的版本状态，即撤销了自身所有的修改

- 文件修改后，已经添加到暂存区（`git add`），之后又做了进一步修改，那么checkout后回到最新一次添加修改到暂存区后的状态，即只撤销了第二次还未添加进暂存区的修改

  ```
  //第一次修改
  Git is a distributed version control system.
  Git is a free software.
  Git has a mutable index called stage.
  Git tracks changes.
  my stupid boss ...
  --------------------------------------------------------------------------------------------
  //将第一次修改添加进暂存区
  git add 
  
  --------------------------------------------------------------------------------------------
  //第二次修改
  Git is a distributed version control system.
  Git is a free software.
  Git has a mutable index called stage.
  Git tracks changes.
  my stupid boss ...
  oh my god!!!
  --------------------------------------------------------------------------------------------
  //撤销修改
  git checkout -- 文件名
  
  //仅仅撤销了第二次，即还未添加进暂存区的修改
  Git is a distributed version control system.
  Git is a free software.
  Git has a mutable index called stage.
  Git tracks changes.
  my stupid boss ...
  
  ```

  

就是让这个文件回到最新一次`git commit`或`git add`后的状态



在第2种情况下，checkout只撤销了还未添加进暂存区的修改

但如果已经添加进暂存区的修改还没有提交，那还能拯救一下~

1. `git reset HEAD 文件名`，撤销暂存区中的修改，重新放回工作区（***unstaged***），现在所有的修改都在工作区中了，即从第2种情况转为第1种
2. `git checkout -- 文件名`，撤销自身未提交的所有修改



那如果把修改提交了呢（`git commit`），可以回退版本，不过前提是没有将stupid boss推送到远程库:)



### 删除文件

删除文件也是一种修改

通常我们直接在文件夹中删去一个文件，相当于在工作区中有了修改，但这个修改还没有添加到暂存区，所以`git status`后会出现

```
Changes not staged for commit
	deleted 文件名
```

现在有**2种情况**：

1. 这个文件你确实要删除，那么要将“删除文件”这一修改添加进暂存区后提交到版本库

   ```
   git rm 文件名
   git commit -m "delete 文件名"
   ```

   

2. 这个文件是你误删了，那么只需要撤销工作区中这一修改即可

   ```
   git checkout -- 文件名
   ```

   **注意**：设想有这样一种情况，你对文件修改后，再误删了它，那么“内容修改”和“删除修改”都是在工作区发生的，都没有添加进暂存区。这是checkout撤销修改，会回到最新一次提交后的状态，即“内容修改”前的状态，也就是说会丢失最新一次提交后你修改的内容



## 远程仓库

Git是**分布式**版本控制系统，同一个Git仓库，可以分布到不同的机器上

Github提供Git仓库托管服务



在实际情况中，找一台电脑充当**服务器**，每天24小时开机，其他每个人都从这个服务器中克隆一份仓库到自己的电脑上；在自己的电脑上对仓库做了一定修改后，再各自把各自的提交推送到服务器仓库中；还可以从服务器仓库中获取别人的提交



### 添加远程库

本地Git仓库和Github中Git仓库的**同步**

1. 在Github中创建一个新的仓库![1](C:\Users\apple\Desktop\myproject\git\pic\添加远程库\1.png)

2. 将Github中的仓库和本地仓库关联![1](C:\Users\apple\Desktop\myproject\git\pic\添加远程库\2.png)

   其中，***orgin***是远程库的名称，用`git push`命令将master分支的所有内容推送到远程

   在第一次推送版本到远程库时，`-u`参数将本地的master分支和远程的master分支关联起来，在以后推送或拉取时就可以简化命令

   ![1](C:\Users\apple\Desktop\myproject\git\pic\添加远程库\2.2.png)



今后，只要在本地做了提交，通过命令`git push orgin master`就可以把本地master分支的最新版本推送至Github中的远程库



### 克隆远程库

`git clone 仓库地址`

因为Git支持多种协议，包括https和ssh，但ssh的速度最快



## 分支管理

### 为什么要用分支？

“假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个**属于你自己的分支**，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。”



### 创建和合并分支

廖雪峰老师的教程中一系列图示非常清晰明了，推荐大家去看~

要注意`HEAD`指针指向当前分支，当前分支的指针指向最新版本

![1](C:\Users\apple\Desktop\myproject\git\pic\创建和合并分支\1.png)



https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000

1. 创建并切换分支，`git checkout -b dev`

   该行代码相当于

   ```
   //创建分支
   git branch dev
   
   //切换分支
   git checkout dev
   ```

   

   HEAD转而指向分支dev，dev和master都指向当前最新版本

   ![1](C:\Users\apple\Desktop\myproject\git\pic\创建和合并分支\2.png)

2. 在当前分支上完成修改，添加进暂存区，提交等操作![1](C:\Users\apple\Desktop\myproject\git\pic\创建和合并分支\3.png)

   在dev分支上操作时，dev分支的时间线不断向前延申，dev指针始终指向最新版本，而master分支的时间线“停留在原地”

   

3. **切换回master分支**，将dev分支的工作成果合并到master分支上（合并某个分支到当前分支）

   ```
   git merge dev
   Updating f312f68..ca87f78
   Fast-forward
    readme.txt | 3 ++-
    1 file changed, 2 insertions(+), 1 deletion(-)
   
   ```

   ![1](C:\Users\apple\Desktop\myproject\git\pic\创建和合并分支\4.png)

此次合并是快进模式Fast-forward，即直接把master指针指向dev指向的最新提交版本

由于只是改变指针指向，不需要改变工作区内容，所以非常快！



4.删除dev分支，`git branch -d dev`



**注意**：`git branch`可以查看所有分支，前面带*号的是HEAD指针指向的当前分支



### 解决冲突

假设下面的情境

1. 创建feature1分支，`git checkout -b feature1`

2. 修改readme.txt后提交

   ```
   Git is a distributed version control system.
   Git is a free software.
   Git has a mutable index called stage.
   Git tracks changes.
   Create a new branch dev
   Create a new branch is quick and simple
   
   --------------------------------------------------------------------------------------------
   git add readme.txt
   git commit -m "quick and simple"
   ```

3. 切换到master分支，也修改readme.txt后提交

   ```
   git checkout master
   --------------------------------------------------------------------------------------------
   Git is a distributed version control system.
   Git is a free software.
   Git has a mutable index called stage.
   Git tracks changes.
   Create a new branch dev
   Create a new branch is quick & simple
   --------------------------------------------------------------------------------------------
   git add readme.txt
   git commit -m "quick & simple"
   ```

   

   现在的分支情况如下图所示，master和feature1各自指向自己时间线中的最新版本

![1](C:\Users\apple\Desktop\myproject\git\pic\创建和合并分支\5.png)



在这种情况下合并分支，无法执行Fast-forward模式，会发生冲突

```
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```



使用`git status`可以查看冲突的文件

```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

```



这时打开readme.txt

![6](C:\Users\apple\Desktop\myproject\git\pic\创建和合并分支\6.png)



手动修改后保存，再尝试提交

```
git add readme.txt
git commit -m "conflict fixed"
```

现在的分支情况如下图所示

![6](C:\Users\apple\Desktop\myproject\git\pic\创建和合并分支\7.png)

用`git log --graph --pretty=oneline --abbrev-commit`查看分支合并情况

![8](C:\Users\apple\Desktop\myproject\git\pic\创建和合并分支\8.png)



**总结**：当合并分支时发生冲突

1. `git status`查看发生冲突的文件
2. 打开发生冲突文件，手动修改后再提交
3. 删除被合并的分支