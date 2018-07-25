

#### 1、Git的作用，与SVN的区别

- Git是软件版本控制管理系统，其特性是分布式架构，使用于多人不同迭代的协同工作

- SVN是集中式的版本控制工具，适用于多人对于同一迭代的开发


#### 2、Git管理的基本原理

- 包括了工作目录，暂存区和本地仓库
- 工作目录就是我们执行命令git init时所在的地方，也就是我们执行一切文件操作的地方，
- 暂存区和本地仓库都是在.git目录

![pictures](https://github.com/Dammyouh/Android-/blob/master/pictures/1531359518791.png?raw=true)





#### 3、Git的安装

##### ①对于Git的安装，初始化，配置

安装：https://git-scm.com/

初始化：git init --->生成 .git 文件夹，其中存在一些配置文件，用于管理版本控制

配置：分为三级，分别对单个仓库，用户，整个系统设置进行配置，优先级依次下降，如果优先级高的设置了，则以优先级高的设置为准。

- /.git/config – 特定仓库的设置。

- ~/.gitconfig – 特定用户的设置。这也是 `--global` 标记的设置项存放的位置。

  ```java
  git config --global user.name "yxy"
  git config --global user.email yf-yangxiaoyu@sunlands.com
  
  $ git config --list
  core.symlinks=false
  core.autocrlf=true
  core.fscache=true
  color.diff=auto
  color.status=auto
  color.branch=auto
  color.interactive=true
  help.format=html
  rebase.autosquash=true
  http.sslcainfo=E:/Git/Git/mingw64/ssl/certs/ca-bundle.crt
  http.sslbackend=openssl
  diff.astextplain.textconv=astextplain
  filter.lfs.clean=git-lfs clean -- %f
  filter.lfs.smudge=git-lfs smudge -- %f
  filter.lfs.process=git-lfs filter-process
  filter.lfs.required=true
  credential.helper=manager
  user.name=yangxy
  user.email=yf-yangxiaoyu@sunlands.com
  
  配置编辑器：（以后打开git commit --amend,git commit -a ,就会弹出编辑器,可以输入提交信息）
  git config --global core.editor "'C:\Program Files (x86)\Notepad++\notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
  
  ```

  

- $(prefix)/etc/gitconfig – 含有对系统上所有用户及所拥有仓库都生效的配置值，全局配置，需要传 git config --system进行配置

忽略文件：.gitignore,所有想要被忽略的文件单独一行，*字符用作通配符。



#### 4、Git的基本命令：

##### git add：（保存修改） 将工作目录的变化添加到缓存区

- git add  <filename> ：添加一个文件

- git add <directory>:添加一个文件夹

- git add . = git add --all : 添加当前工作目录的所有更改

  

##### git status : 查看工作目录和缓存区的状态



##### git commit :（提交修改）将缓存区的变换添加到HEAD提交历史

- git commit -m "<Message>" : message是提交的备注，建议用英文,否则在查看提交历史的时候容易出现乱码。

- git commit -am  "<Message>" :可以不用 git add,直接提交到HEAD

- git commit  : 会弹出一个文本编辑器（#是注释，可以通过config修改文件编辑器软件），输入提交备注

- git commit --amend:可以将缓存的修改和之前的提交合并到一起，而不是提交一个全新的提交，編輯的信息会在原来提交信息的后面。（用与上一个commit相同的commit Id)

- git commit --amend --no-edit：--no-edit标记会修复提交但是不提交信息（重用上次的commit message，本次提交没有信息)。（用与上一个commit相同的commit Id)

  

##### git log: 查看HEAD的提交历史

- git log :超过一屏幕用空格键来滚动，按q退出。(包含commit ID,Auther,Date，提交信息)

- git log -n <limit>:用limit来限制提交的数量。（git log -n 3:显示3次commit 信息，-n可以省略)

- git log --oneline:将每个提交压缩到一行(包含commit Id的一部分，commit信息。如果是用amend,则显示第一次的commit信息）。

- git log --graph --decorate --oneline ：（包含：提交的commit 演进图,commit Id，commit 信息）

- git log --stat:(包含commit ID,Auther,Date，提交信息，哪些文件被更改了）

- git log -p:显示每一个提交的差异(包含commit ID,Auther,Date，提交信息，哪些文件被更改了，修改的具体内容）

  

##### git reflog:可以查看整个的命令历史（commit ,rabase,checkout 等命令）



##### git branch:可以创建，列出，重命名，删除分支。

- git branch :列出仓库中所有分支

- git branch --list bug/*:会显示所有以bug/开头的分支

- git branch <new-branch-name>:以当前branch为基，创建一个名为<new-branch-name>的分支，不会自动切换到那个分支去

- git branch <new-branch-name> <old-branch-name>/commitId/Tag:以<old-branch-name>/commitId/Tag为基，创建一个名为<new-branch-name>的分支，不会自动切换到那个分支去

- git branch -d <branch>:删除指定分支，这是一个安全的操作，Git会阻止删除包含未合并更改的分支

- git branch -D <branch>:强制删除指定分支

- git branch -m <oldBranch> <newBranch>:将<oldBranch>重新命名为<newBranch>。（如果没有<oldBranch>,则将当前分支命名为<newBranch>)

- git checkout -b <new-branch-name>:以当前branch为基础，创建一个名为<new-branch>的分支，并自动切换到那个分支去

- git checkout -b <new-branch-name> <old-branch-name> :以<old-branch-name>为基，创建一个新的分支，并切换到那个新的分支。

- git checkout -b <new-branch-name> <old-tag>:以tag名为<old-tag>为基，创建一个新的分支，并切换。

- git checkout -b <new-branch-name> <old-commitId>:以<old-commitId>为基，创建一个新的分支，并切换。

  

##### git pull :将远程的更改合并到本地仓库。

- git pull <remote>:拉取当前分支对应的远程副本中的更改。

- git pull --rebase <remote>:git rebase用来合并远程和本地分支，而不是git merge,使得合并后的更线性，将本地的修改放到远程commit的后面。

  

##### git fetch:会将远程仓库的提交导入到本地仓库，拉取下来的提交储存为远程分支，而不是普通的本地分支

- git fetch <remote>:拉取仓库中所有的分支，同时会存储从远程所有需要的提交和文件，但是本地文件未做改变。

- git fetch <remote> <branch>:拉取远程仓库特定的分支

  

##### git merge:可以将两个branch的代码进行合并。

- git merge <branch>:将指定分支并入当前分支

- git merge <branch> <otherBranch>:将otherBranch合并到branch

- git merge --no-ff <branch>:将指定分支并入当前分支，但总是生成一个合并提交。（3-way-merge)

  

##### git remote:可以创建，查看，删除和其他仓库之间的连接

- git remote:列出和其他仓库之间的远程连接

- git remote -v : 列出和远程仓库的连接，同时还展示每个连接的URL。

- git remote add <name><url>:创建一个新的远程仓库连接，在添加之后可以将<name>作为<url>来使用

- git remote rm <name>:移除名为得远程仓库的连接

- git remote rename <old-name> <new-name>:将远程连接从<old-name>重命名为<new-name>




##### git push :将本地的仓库中的提交转移到远程仓库中要做的事情，与git fetch 正好相反。

- git push <remote><branch>:将指定的分支推送到<remote>上。

- git push <remote> --force:强制推送

- git push <remote> --all: 将所有本地分支推送到指定的远程仓库

- git push <remote> --tags:将所有的本地标签推送到远程仓库中去

  

##### 本地提交推送到中央仓库标准作法:

​	git fetch origin master

​	git rebase -i origin/master

​	git push origin master



##### git checkout :用于检出文件，检出提交，检出分支

- git checkout <commit> <file>:查看文件之前的版本，将工作目录的<file>文件变成与<commit>中的那个文件一致，并将其加入缓存区，仅需要执行git commit 进行提交。

- git checkout <commit>:更新工作目录中的所有文件，使得其和某个特定的提交中的文件一致，即回到某个特定的分支。但是当前会变成detached HEAD，即将HEAD 指向该commit节点，形成游离的HEAD的状态。用git status查看没有变化。

- git checkout <commit> . :不会更改HEAD的状态，当切换到commit节点后，用git status查看会有变化。

  

##### git revert :用来撤销一个已经提交的快照，其并不是从历史项目中移除这个提交，而是撤销了更改的新提交。可以针对历史中的任何一个提交。（更加安全）

- git revert <commit>：生成了一个撤销了<commit>引入的修改的新提交,即生成新的commitId，然后应用到当前分支。
- git revert 遇到冲突后：①放弃revert：git revert --abort    ②解决冲突，git add后，继续revert: git revert --continue

​    

##### git reset:回滚一个单独的提交，没有移除后面的提交，然后回到项目之前的状态。后面的提交也需要重新提交一遍。只能从当前提交向前回溯。（比较危险，不要在共享仓库上执行git reset）

- git reset <file>:对于已经add的文件，执行该命令之后，从缓存区移除该文件，但是不改变工作目录。（需要再git add 一遍）

- git reset :对于已经add的多个文件，从缓存区移除，重新设置缓存区（原来add到缓存区的需要全部再git add 一遍）

- git reset -- hard:重新设置缓存区和工作目录，匹配最新的一次提交。（并且工作目录和缓存区的修改都不存在了，清除了所有未提交的更改）

- git reset --soft:

- git reset --默认：

- git reset <commit>:将当前分支的末端移到<commit>,将缓存区重设到这个提交，其后面的提交信息没有了，但是不改变工作目录。（后面的commit需要再全部git add ,git commit 一次）

- git reset --hard <commit>:将缓存区和工作目录所有的文件都重新设置到这个提交，并且后面的提交以及更改都会被删除(危险）。 

  

##### git rebase:变基，将分支移到一个新的基提交的过程，主要是为了保持一个线性的项目历史。还可以重写项目历史。

- git rebase <base>:<base>可以是任何类型的提交引用（ID，分支名，标签，HEAD等）

- git rebase -i <branch>:可以修改branch之后的提交的历史

  - 注意顺序是上面的表示最新的提交在最上面。
  - 可以通过pick来进行挑选，可以改变顺序来进行pick
  - 可以通过edit某次提交来进行編輯这次提交，然后rebase停止，可以进行編輯再用git commit --amend进行保存提交，再用git rebase --continue进行继续rebase.
  - 如果想要删除某次提交，则可以不用pick,或者用drop
  - 可以用squashing某次提交将本次提交与上一次压制为单一提交
  - 可以用reword来編輯某次提交的提交信息

- 不要rebase那些已经推送到公共仓库的提交，rebase会用新的提交替换旧的提交。

  

##### git clean:将未跟踪的文件从工作目录中移除，但是无法撤销

- git clean -n :查看如果执行git clean ，哪些文件会被移除，而不是真的删除它

- git clean -f : 强制移除当前目录下未被跟踪的文件

- git clean -f <path>:强制移除某个路径下的未被跟踪的文件

- git clean -xf: 移除当前目录下未跟踪的文件，以及Git一般忽略的文件。

  









