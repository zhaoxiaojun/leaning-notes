###创建版本库
<hr>
<ol>
<li>初始化一个Git仓库，使用git init命令。</li>
<li>添加文件到Git仓库，分两步：
	<ul>
		<li>第一步，使用命令git add<file>,注意，可反复多次使用，添加多个文件；</li>
		<li>使用命令git commit，完成。</li>
	</ul>

</li>
</ol>

`git add file1.txt`<br />
`git add file2.txt file3.txt`<br />
`git commit -m "add 3 files."`

###时光穿梭机
<hr>
* 使用git status命令  随时掌握工作区的状态。
* 如果git status告诉你有文件被修改过，用git diff可以查看修改内容。

###版本回退
<hr>
<strong>git log命令显示从最近到最远的提交日志、如果日志信息太多、可以试试加上--pretty=oneline参数。</strong>

`HEAD`:标示当前版本。<br />`HEAD^`:表示上一个版本。<br />`HEAD^^`：上上一个版本。<br />
`git reset --hard HEAD^`<br />
`cat + file`

<strong>当会退到了某个版本后，想回到新版本：</strong><br/>
`git reflog`:查看命令历史，以便确定要回到未来的那个版本。

###工作区和暂存区
***
* 工作区就是你在电脑里能看到的目录
* 工作区有一个隐藏目录	`.git`，这个不算工作区，而是Git的版本库。
* 版本库里存在很多东西，其中最重要的就是称为stage（或者叫index）的暂存区。
`git add把文件添加进去，实际上就是把文件修改添加到暂存区。`<br/>
`git commit实际上就是把暂存区的所有内容提交到当前分支。`

###管理修改
***
`git diff HEAD --readme.txt命令可以查看工作区和版本库里面最新的区别.`

###分支修改
***
`场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。`

`场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，`
        `第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。`

`场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。`

###删除文件
***
<ol>
<li>删除文件：rm test.txt， 
	<ul>
	<li>确实要从版本库中删除该文件：<br/>1、git rm test.txt <br/>2、git commit -m "XXXXX"<br/></li>
	<li>误删,返回原文件：<br/>1、git checkout -- test.txt</br></li>
	</ul>
</li>
</ol>

###远程仓库
***
	git config --global user.email "your@email.com"
	git config --global user.name "Your Name"

`ssh-keygen -t rsa -C "youremail@example.com"创建公私钥,id_rsa和id_rsa.pub(公钥)`

`git remote add origin git@github.com:michaelliao/learngit.git`

`git push -u origin master 加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支,`
`还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。`

`git push origin master`


###分支管理
***
`查看分支：git branch`

`创建分支：git branch <name>`

`切换分支：git checkout <name>`

`创建+切换分支：git checkout -b <name>`

`合并某分支到当前分支：git merge <name>`

`删除分支：git branch -d <name>`

<p><b>注意：上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”</b></p>

###解决冲突
***
<strong>当主分支与需要合并的分支都发生文件修改，合并时，产生冲突，需要先解决冲突</strong>

* 查看分支合并情况：<br />
	git log --graph --pretty=oneline --abbrev-commit

###分支管理策略
***
`修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；`
`当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复`
`后，再git stash pop，回到工作现场。`

`git merge --no-ff -m "merged bug fix 101" issue-101`
`git stash list`
`git stash pop`

`如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。`

###多人协作
***
1、首先，可以试图用git push origin branch-name推送自己的修改；

2、如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

3、如果合并有冲突，则解决冲突，并在本地提交；

4、没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。

<p>小结</p>
* 查看远程库信息，使用git remote -v;
* 本地新建的分支如果不推送到远程，qui其他人就是不可见的；
* 从本地推送分支，使用git push origin branch-name,如果推送失败，先用git pull抓取远程的新提交；
* 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
* 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
* 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。