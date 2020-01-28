# Git 学习笔记

## Git简介

1. Git 是一个分布式版本控制系统
   - 集中式版本控制系统：有一个中央服务器，所有的文件存储在中央服务器中，要修改文件时先从中央服务器获取文件，修改文件后再把改动合并到中央服务器中，必须联网。
   - 分布式版本控制系统：没有中央服务器，每一台电脑都有完整的文件，改动文件时，只需把改动发送给组内的其他成员，其他成员将改动合并到自己的文件中，不需要下载整个文件，局域网内不需要联网。
2. Git 是一个开源免费的软件

## Git使用

1. 初始化一个Git仓库，使用`git init` 命令
2. 添加文件到git 仓库，分两步：
   1. 使用命令`git add<file>` ，可以反复使用多次，添加多个文件。
   2. 使用命令`git commit` 完成。
3. Git 添加修改与添加文件使用的命令一样，`git add<file>` 和`git commit` 
4. `git status` 可以查看仓库的状态，`git diff <file>` 可以查看修改的具体内容

## Git 版本回退 

1. HEAD指向的版本就是当前版本，`git reset --hard HEAD^` 可以回退到上一个版本，`HEAD^^` 可以回退到上上个版本，可以使用`HEAD～n` 指回退到上n个版本.使用`git reset --hard commit_id` 可以在各个版本之间切换。
2. 用`git log`可以查看提交历史，`git reflog` 可以查看命令历史，可以看到commid_id 
3. 工作区和暂存区
   - 工作区就是电脑中的目录。
   - 目录中的.git文件是Git的版本库，版本库中最重要的是stage，即暂存区，使用`git add` 命令的时候就是将修改添加到暂存区。
   - 使用`git commit` 命令时就是将暂存区的所有文件提交到当前分支。
4. 管理修改
   - Git 跟踪管理的是修改，而不是文件，如果没有将修改add 到暂存区，`git commit` 命令不能将修改提交到版本库中，版本库中的文件没有改变
5. 撤销修改
   - 修改了文件之后，还没有添加到暂存区，可以使用`git checkout --file` 命令来撤销修改，文件会回到最近一次修改前的状态。
   - 修改了文件之后，已经添加到暂存区，可以使用`git reset HEAD file` 命令来丢弃修改，把暂存区的修改退回工作区。
6. 删除文件
   - 要删除版本库中的文件，使用命令`git rm file` ，然后再使用`git commit` 提交修改
   - 如果想从版本控制库中恢复删除的文件到本地目录中，可以使用命令`git checkout -- file` 来恢复文件


## 远程仓库

1. 关联远程仓库（GitHub，要注册github帐号）
   - 创建SSH Key。使用命令`ssh-keygen -t rsa -C"yomail@example.com` ，然后一直回车无需设置密码，成功后可以在用户主目录中找到.ssh目录，里面有`id_rsa` 和 `id_rsa.pub` 两个文件，这是SSH Key的密钥对。
   - 登录GitHub，进入Account seetings， SSH Key页面，点击Add SSH Key，填上Title，然后将`id_rsa.pub` 文件的内容粘贴到Key文本框中。（GitHub需要识别推送确实是属于某个人的，而不是别人冒充的，Git支持SSH协议，GitHub只需知道SSH Key的公钥就可以确认推送的人是否是本人）
   - 在GitHub新建仓库，然后在本地仓库使用命令`git remote add origin git@github.com:xxx/xxx.git` 就可以将本地仓库关联到远程仓库了。
   - 将本地库的所有内容推送到远程库上，使用命令`git push -u origin master` 之后只要本地作了提交，就可以使用命令`git push origin master` （没有-u参数）将本地master分支的修改推送到GitHub上。
2. 克隆远程仓库
   - 可以通过`git clone <address>` 来克隆一个远程仓库到本地目录。
   - Git支持https、ssh协议，ssh协议最快

## 分支管理

> 可以在Git中创建一个属于自己的分支，在自己的分支上工作、提交对其他人是透明的，直到开发完毕，再一次性合并到原来的分支上，这样既安全又不影响别人工作。

1. 创建和合并分支
   - Git把每次提交串成时间线，这条时间线就是一个分支，第一条时间线是主分支，即master分支，HEAD指针指向的就是当前分支。每次提交，master指针指向最新的提交，HEAD指针指向master。
   - 当创建新的分支时，Git创建一个新的指针dev，指向master相同的提交，再把HEAD指向dev，表示当前分支为dev，之后对工作区的提交和修改都是针对dev分支的，每提交一次，dev指针向前移动一步，而master指针不变。
     - 使用命令`git checkout -b branchName` 可以创建并切换到新的分支，也可以分两步执行：
       - `git branch branchName` 创建分支
       - `git checkout branchName` 切换分支
     - `git branch` 可以查看所有分支，当前分支前面会有一个`*` 号
   - 在dev的工作完成后，可以把dev分支合并到master上，即把master指向dev的当前提交，然后可以删除dev分支。
     - 使用命令`git merge dev` 可以将dev分支合并到当前分支
     - `git branch -d dev` 命令可以删除dev分支

2. 解决冲突 
   - 当两个分支都有新的提交而没有同步时，合并两个分支会出现冲突，此时必须手动解决冲突之后再提交
   - Git用`<<<<<<<` ，`======` ，`>>>>>>>` 标记出不同分支的内容。
   - `git log --graph` 命令可以查看分支合并图

3. 分支管理策略
   - 合并分支时，Git会默认使用`Fast forward` 模式，但这种模式下，删除分支后会丢掉分支信息。

   - 强制禁用`Fast forward` 模式，Git 在merge时会生成一个新的commit，这样从分支历史就能看到分支信息。
     
- 使用命令`git merge --no-ff -m 'text' branch` 可以强制禁用`Fast forward` 模式
     
   - 分支策略
     - master分支应该是非常稳定的，应该仅用来发布新版本，平时不能在上面干活。
  - 平时应该在dev分支上工作，每个人都有自己的分支，需要时将自己的分支合并到dev上就可以了。
   
- Bug 分支
   
     - Git 提供一个stash 功能，能够将当前工作现场存储起来，等以后恢复现场继续工作。用命令`git stash` 来保存现场
     - 新建一个分支来修复bug，修复bug之后合并，然后删除，修复bug之后要恢复之前的工作现场
     - 用命令`git stash list` 查看保存的工作现场
     - 恢复现场有两种方法：
       - 一种是使用`git stash apply stash@{xx}` 来恢复一个特定的现场，但是恢复之后stash内容不删除，需要使用`git stash drop` 来删除。
    - 另一种是使用`git stash pop` 来恢复，恢复之后stash内容也删除了
   
- feature分支
   
     - 要开发新功能时，最好新建一个feature分支;
  - 要丢弃一个没有被合并的分支，使用命令`git branch -D <name>` 强行删除
   
- 多人协作
   
  - 推送分支
   
       - `git remote` 可以显示远程库信息，`git remote -v` 可以显示更详细的信息
    - `git push origin branch-name` 可以推送分支，不只是master分支
   
  - 抓取分支
   
    - 使用`git clone <address>` 可以从github上抓取项目，但是默认只会抓取master分支的内容。创建远程分支到本地要用命令`git checkout -b dev origin/dev`  
   
    - 工作模式：
   
         - 首先，可以用`git push origin branch-name`推送自己的修改；
         - 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
         - 如果合并有冲突，则解决冲突，并在本地提交；
      - 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！
   
      >  如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to=origin/branch-name branch-name`
   
4. 标签管理

   > 标签与commit绑定在一起，是一个容易记住而且有意义的名字。

   - 创建标签
     - 用命令`git tag <name>` 可以创建一个标签，默认为HEAD，也可以在后面家上commit_id。
     - `git tag -a <name>  -m ‘说明’` 命令可以创建一个有说明的标签。
     - `git tag -s <name> -m ‘xxx’` 命令可以创建pgp签名的标签
     - `git tag` 可以查看所有的标签
     - `git show <name>` 可以显示name标签的具体信息
   - 操作标签
     - 使用命令`git tag -d <tagname>` 可以删除标签
     - 使用命令`git push origin <tagname>` 可以将标签推送到远程
     - 要删除远程的标签，首先要用`git tag -d<tagname>`删除本地的标签，然后用命令`git push origin :refs/tags/<tagname>` 删除远程的标签
   - 使用GitHub
     - 在GitHub上可以fork任意的开源项目
     - 自己拥有Fork后的仓库的读写权限
     - 可以推送pull request给官方仓库来贡献代码

5. 自定义Git

   - 需要忽略仓库中的某些文件时，需要编写`.gitignore` 

   - `.gitignore` 本身要放到版本库里，做版本管理

   - 可以搭建自己的Git服务器，具体看[这里](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000 )   

     