1. 简介
  之前的CVS及SVN是集中式的版本控制系统，而Git是分布式版本控制系统。Git有极其强大的分支管理。CVS和SVN都是开源且免费的集中式版本控制系统，CVS本身设计的问题，会造成提交文件不完整，版本库莫名其妙损坏的情况。SVN修正了CVS的一些稳定性问题，是目前用得最多的集中式版本控制系统。
  查看系统是否安装git
  * $ git
2. 新建仓库，初始化仓库，添加文件到仓库
  * $ mkdir xxx           (新建路径）
  * $ cd xxx
  * $ git init            (初始化仓库)
  * 在xxx(子路径也可）新建要添加的文本文件yyy.md
  * $ git add yyy.md      (将文件添加到仓库，可以一次添加多个文件(以空格隔开),也可执行多次该语句添加多个文件)
  * $ git commit -m "help message"  (把文件提交到仓库)
3. 修改文件，查看状态，查看修改
  * $ git status         (随时查看仓库的状态，是否有修改的文件未add，是否有add的文件未commit)
  * $ git diff xxx.y     (xxx.y为查看status后发现修改过未add或commit的文件名，此命令可以显示未提交的文件修改了哪些地方)
4. 查看修改历史
  * $ git log            (打印出从近到远的修改历史)(有一个问题：一个仓库里有多个文件的话，只显示每次log的描述，并没有打印出是哪个文件的log，这个有点不好，怎么解决？)
  * $ git log --pretty=oneline
5. 版本穿梭 (修改的只是本地版本库）
  * $ git reset --hard HEAD^  (或者HEAD~5, 该命令是回到前面的版本）
  * $ git reset --hard commit_id   (commit_id 是未来的后面的版本的commit id，如果当前terminal没有关掉可以从上面的打印中找到，如果terminal关掉，用下面的命令)
  * $ git reflog         (打印之前的commit)
  * git log 打印提交历史， git reflog 打印命令历史
6. 几个概念
  * 工作区
  * 版本库
  * 暂存区(stage),作用(所有修改必须用add先添加到stage，否则commit是无法添加到版本库的，Git跟踪的是修改而不是文件),理解stage及Git是如何跟踪修改的
  * 分支
  * HEAD
  * git add , git commit含义
  * git diff HEAD -- xxx.y (查看工作区和版本库里面HEAD指向的版本的区别)， git diff xxx.y (查看的是工作区和暂存区之间的区别)  (实验看到的)
  * 要查看当前工作区，暂存区和版本库HEAD之间的差别，从两个命令就可以看出来了，git diff xxx.y ：工作区和暂存区的区别； git diff HEAD -- xxx.y: 工作区与版本库HEAD之间的差别。这样就清楚了有没有工作区的修改没有add，暂存区有没有修改没有commit及具体内容是什么
7. 撤销修改
  * $ git checkout -- xxx.y
    * 如果修改未添加到stage，执行上述命令则文件返回到修改前的状态.直接从文件中删除也是一样的。
    * 如果修改已经被add到stage，执行上述命令则文件只会不保存未被add到stage的修改部分，已经在stage的修改不会撤销
    * 即将文件返回到最近一次git commit或者git add时的状态
    * 注意不能掉了--
  * $ git reset HEAD xxx.y         (撤销stage的修改，并将其退回到工作区，即stage的文件没有了，并且该stage的文件修改的内容又返给工作区的文件了)
8. 删除文件
  * $ rm xxx.y 删除文件，或者直接从文件管理器中删除是一样的
  * $ git rm xxx.y 从版本库中删除文件
  * $ git checkout -- xxx.y 将工作区删除的文件从版本库中恢复
9. .goutputstream-xxx文件
  在git的一个库中，中间不知道什么时候出现了几个.goutputstream-xxx的文件，解释：https://blog.csdn.net/xiajian2010/article/details/19985495 
10. 远程仓库
  * SSH
  * new remote repository
  * $ git remote add origin https://github.com/Happyna88/GitNote.git
  * $ git push -u origin master(第一次push 加上-u)
  * $ git push origin master
  
  * $ git clone https://github.com/Happyna88/ActionDetection.git
  * 从本地仓库到远程库：先建立好本地仓库，然后在Github新建一个空的仓库(不init with README.md, 这一步别忘了)，然后reomte add origin， push就可以了。从远程库克隆到本地库：不需要先在本地建好文件夹，cd到想要放的位置，直接clone就可以了。
  * git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快，也就是上面的地址ssh为git@github.com:Happyna88/GitNote.git
11. 本地与远程库的关联与取消
  * cd 到本地库所在位置，remote add origin执行一次就将该本地的工作区关联到了远程的位置，如果发现远程地址写错了，需要取消关联后再次关联，取消关联是git remote remove origin, 然后再次用git remote add origin ....就可以了。
问题：如果本地库和远程库已经关联好了也同步了，后来本地或远程某一方做了修改，如何再次同步，或者如何查看某一方修改后修改的具体内容是什么？或者两者有冲突时如何处理？
从本地到远程：git push git@github.com:Happyna88/xxx.git
从远程到本地：https://blog.csdn.net/hanchao5272/article/details/79162130 
12. 分支
  * $ git checkout -b dev ; git branch dev, git checkout dev
  * $ git branch 
  * $ git checkout master
  * $ git merge dev
  * $ git branch -d dev
  * $ git log --graph
  * $ git log --graph --pretty=oneline --abbrev-commit  (冲突)
  * $ git branch -d dev
  * 特别注意master和dev冲突时的处理，最简单的办法就是用git log --graph --pretty=oneline --abbrev-commit,这时文件中会出现两者不同的地方，这时修改为正确的，保存关闭，然后删掉dev就可以了。另外删除dev的时候要checkout到master上才可以删掉
  * $ git merge --no-ff --commit "xxx" dev
  * $ git stash 保存当前工作区和stage区现场，可以多次使用，保存多个现场
  * $ 建立分支修复bug
  * $ git stash list 查看保存的现场列表
  * $ git stash apply stash@{0}   回到现场所在的分支，回复现场
  * $ git stash drop ...  删除保存的stash现场
  * $ git stash pop
问题：新建分支后master也commit了几次，分支也commit了几次，即有了冲突，这时候最好的处理方法是什么？再看冲突那一节
  * $ git merge --abort
问题：git merge dev 和git merge --no-ff dev什么区别？没懂
13. 多人协作
  * $ git remote, git remote -v
  * $ git push origin dev; master分支，dev分支，bug分支，feature分支
  * $ git push origin dev, 先将自己本地的分支push到远程库
  * $ git branch --set-upstream-to=origin/dev dev, 如果提示本地分支未和远程分支关联，则先用此命令关联
     * $ git checkout -b dev origin/dev 如果首次从远程库clone下来，本地库是没有dev分支，这时先在本地库新建一个分支并和远程的origin/dev关联
  * 然后再次git push origin dev, 关联后将本地分支push到远程库
  * $ git pull， 如果push失败，则说明远程分支比自己本地分支更新，先pull，将远程库的新的更新同步到本地.
问题：如果冲突是发生在同一个文件里，git pull不会就把本地新做的工作给覆盖了吗？做实验看一下。
  * 如果pull后，本地库和远程库有冲突，则先解决冲突，在本地库修改并commit后，最后再push到远程库即可
14. 标签
  * $ git tag v1.0      打在最新提交的commit上
  * $ git log --pretty=oneline --abbrev-commit , git tag v0.9 commit_id 在指定的commit_id 上打上标签
  * $ git tag 打印出所有的标签，显示顺序是tag的名字而不是按照时间顺序
  * $ git show tag_name 打印tag_name 对应的版本的信息
  * $ git tag -a tag_name -m "description" commit_id
  * $ git tag -s tag_name -m "description" commit_id   私钥，PGP签名
  * $ git tag -d tag_name 删除本地标签
  * $ git push origin tag_name
  * $ git push origin --tags
  * $ git tag -d tag_name, git push origin :refs/tags/tag_name 删除远程库的标签,注意origin与后面的:refs之间有一个空格
15. git修改作者名字和邮箱
  * https://blog.csdn.net/u010953692/article/details/78585951 

  * $ git config user.name
  * $ git config user.email
  * $ git config user.name "name"
  * $ git config user.email "mailbox"
  * 修改后后面的commit的author和email都是变为新修改的，但前面的不会更新，还是原来的
16. 去掉push，pull每次都输入密码
  (下面的方法是remote地址从https改为ssh,改后每次push就不用每次都输入remote的username和密码了)
  * $ git remote -v
  * $ git remote rm origin
  * git remote add origin git@github.com:Happyna/GitNote
17. Mine
  * 一个远程仓库下建多个文件夹
  * git add/commit出现错误：另一个git进程在仓库运行。。。；解决：关掉所有IDE或编辑器，打开任务管理器关闭所有git进程，然后重新进行commit操作
18. 自定义git
  * $ git config --global colur.ui true
  * $ .gitignore 文件，忽略某些文件
  * $ git config --global alias.NewName "Command"
  * $ (mine)git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit" (--global参数)
  * $ .git/config文件  （问题：已经设置了上面的alias，为什么config文件里没有列出来？在主目录下的.gitconfig文件里，该文件为当前用户的git配置文件)
  * $ 可以直接修改.git/config或者.gitconfig文件，增加、修改、删除都可以
19. 搭建Git服务器
20. Markdown 语法
    
<https://blog.csdn.net/u014061630/article/details/81359144>
   


  


