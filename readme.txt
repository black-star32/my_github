1.安装git
  sudo dnf -install git
2.创建版本库
  mkdir my_github
  cd my_github
  将这个目录变成git可以管理的仓库
  git init
  编写一个test.txt
  内容：
	Git is a version control system.
	Git is free software.
  把文件添加到仓库 git add test.txt
  把文件提交到仓库 git commit -m "wrote a test file"
  注:git添加文件分两步 add,commit
     commit可以一次性提交很多文件，所以可以多次add不同的文件
     如：
	git add file1.txt
	git add file2.txt file3.txt
	git commit -m "add 3 files"
3.查看仓库当前状态
  修改test.txt
  内容：
	Git is a distributed version control system.
	Git is free software.
  运行git status查看仓库当前状态
  运行git diff查看修改细节
  git add test.txt
  git commit -m "add distributed in test.txt"
4.版本退回
  修改test.txt
  内容：
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
  git add test.txt
  git commit -m "add GPL in test.txt"
  test.txt有三个版本被提交到Git仓库里：
	版本1：wrote a test file
		Git is a version control system.
		Git is free software.
	版本2：add distributed in test.txt
		Git is a distributed version control system.
		Git is free software.
	版本3：append GPL in test.txt
		Git is a distributed version control system.
		Git is free software distributed under the GPL.
  git log 可以告诉我们修改历史纪录:
	commit f844c4a1a1e4d98f037e263faaf7f8d5610d0526 (HEAD -> master)
	Author: black-star32 <fengbo1118@gmail.com>
	Date:   Sat Mar 10 21:29:40 2018 +0800

    	    append GPL in test.txt

	commit cbb0cc446055fdba08a3d97686f94e6b83277f4f
	Author: black-star32 <fengbo1118@gmail.com>
	Date:   Sat Mar 10 21:24:30 2018 +0800

	    add distributed in test.txt

	commit 721b430df337e8131aca564a56229a632f9b4fbb
	Author: black-star32 <fengbo1118@gmail.com>
	Date:   Sat Mar 10 21:10:15 2018 +0800

       	    wrote a test file
  git log显示从最近到最远的提交日志，我们可以看到test.txt的3次提交记录，如果输出信息过多，可以通过git log --pretty=oneline简化输出:
	f844c4a1a1e4d98f037e263faaf7f8d5610d0526 append GPL in test.txt
	cbb0cc446055fdba08a3d97686f94e6b83277f4f add distributed in test.txt
	721b430df337e8131aca564a56229a632f9b4fbb wrote a test file
  f844c4a1a1e4d98f037e263faaf7f8d5610d0526是commit id(版本号)
  首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交f844c4a1a1e4d98f037e263faaf7f8d5610d0526，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
  从当前版本“append GPL in test.txt”回退到上一个版本“add distributed in test.txt”
  git reset --hard HEAD^
  cat test.txt查看结果
  最新的append GPL已经看不到了，在终端没有关闭的情况下，可以通过commit id指定回到未来的某个版本：
  git reset --hard f844c4a1a
  如果关闭了终端使用 git reflog（记录每一次命令)
5.工作区和暂存区
  工作区：my_github文件夹就是一个工作区
  版本库：.git目录，不属于工作区，而是Git的版本库
  修改test.txt：
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
  在工作区新增加一个LICENSE文本文件
  使用两次命令git add，把readme.txt和LICENSE都添加后，用git status再查看一下
  git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支
6.管理修改
  add commit status
7.撤销修改
  命令git checkout -- test.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
  一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态
  一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态
  总之，就是让这个文件回到最近一次git commit或git add时的状态
  用命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作
8.删除文件
  先添加一个新文件test1.txt到Git并且提交：
  git add test1.txt
  git commit -m "add test1.txt"
  删除本地文件
  rm test1.txt
  git status
  现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：
  git rm test1.txt
  另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
  git checkout -- test1.txt
9.添加远程仓库
  第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key
  ssh-keygen -t rsa -C "youremail@example.com"
  你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
  第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：
  然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容
  登陆GitHub,创建一个新仓库
  git remote add origin https://github.com/black-star32/my_github.git
  git push -u origin master
  把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
  从现在起，只要本地作了提交，就可以通过命令：
  git push origin master
10.从远程仓库克隆
  从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆
  首先，登陆GitHub，创建一个新的仓库，名字叫gitskills
  我们勾选Initialize this repository with a README，这样GitHub会自动为我们创建一个README.md文件。创建完毕后，可以看到README.md文件
  git clone https://github.com/black-star32/gitskills.git
11.创建与合并分支
  在版本回退里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。
  首先，我们创建dev分支，然后切换到dev分支：
  git checkout -b dev
  git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
  git branch dev
  git checkout dev
  然后，用git branch命令查看当前分支：
  git branch命令会列出所有分支，当前分支前面会标一个*号。
  然后，我们就可以在dev分支上正常提交，比如对readme.txt做个修改，加上一行：
  Creating a new branch is quick.
  git add readme.txt 
  git commit -m "branch test"
  现在，dev分支的工作完成，我们就可以切换回master分支：
  git checkout master
  切换回master分支后，再查看一个readme.txt文件，刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变：
  现在，我们把dev分支的工作成果合并到master分支上：
  git merge dev
  git merge命令用于合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和dev分支的最新提交是完全一样的。
  注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。
  当然，也不是每次合并都能Fast-forward.
  合并完成后，就可以放心地删除dev分支了：
  git branch -d dev
  删除后，查看branch，就只剩下master分支了：
  git branch
  因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。
12.解决冲突
  准备新的feature1分支，继续我们的新分支开发：
  git checkout -b feature1
  修改readme.txt最后一行，改为：
  Creating a new branch is quick AND simple.
  在feature1分支上提交：
  git add readme.txt
  git commit -m "AND simple"
  切换到master分支：
  git checkout master
  Git还会自动提示我们当前master分支比远程的master分支要超前1个提交。
  在master分支上把readme.txt文件的最后一行改为：
  Creating a new branch is quick & simple.
  提交:
  git add readme.txt
  git commit -m "& simple"
  现在，master分支和feature1分支各自都分别有新的提交
  这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突:
  git merge feature1
  Git告诉我们，readme.txt文件存在冲突，必须手动解决冲突后再提交。git status也可以告诉我们冲突的文件
  git status
  我们可以直接查看readme.txt的内容
  <<<<<<< HEAD
  Creating a new branch is quick & simple.
  =======
  Creating a new branch is quick AND simple.
  >>>>>>> feature1
  Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，我们修改如下后保存：
  Creating a new branch is quick and simple.
  再提交：
  git add readme.txt
  git commit -m "conflict fixed"
  用带参数的git log也可以看到分支的合并情况
  git log --graph --pretty=oneline --abbrev-commit
  最后，删除feature1分支：
  git branch -d feature1
13.分支管理策略
  通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支，会丢掉分支信息。如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
  下面我们实战一下--no-ff方式的git merge：
  首先，仍然创建并切换dev分支：
  git checkout -b dev
  修改readme.txt文件，并提交一个新的commit：
  git add readme.txt
  git commit -m "add merge"
  现在，我们切换回master：
  git checkout master
  准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：
  git merge --no-ff -m "merge with no-ff" dev
  因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。
  合并后，我们用git log看看分支历史：
  git log --graph --pretty=oneline --abbrev-commit
14.bug分支
  当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交：
  git status
  并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？幸好，Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
  git stash
  现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。
  首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：
  git cheackout master
  git cheackout -b issue-101
  现在修复bug，需要把“bug”改为“there is no bug”，然后提交：
  git add readme.txt
  git commit -m "fix bug 101"
  修复完成后，切换到master分支，并完成合并，最后删除issue-101分支：
  git checkout master
  git merge --no-ff -m "merge bug fix 101" issue-101
  git branch -d issue-101
  接着回到dev分支干活
  git checkout dev
  git status
  工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：
  git stash list
  工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：
  一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
  另一种方式是用git stash pop，恢复的同时把stash内容也删了：
  git stash pop
  再用git stash list查看，就看不到任何stash内容了：
  git stash list
  你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
  git stash apply stash@{0}
15.Feature分支
  软件开发中，总有无穷无尽的新的功能要不断添加进来。
  添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。
  开发代号为Vulcan的新功能，该功能计划用于下一代星际飞船
  git checkout -b feature-vulcan
  开发完毕
  git add vulcan.py
  git status
  git commit -m "add featute vulcan"
  切回dev，准备合并：
  git checkout dev
  就在此时，接到上级命令，因经费不足，新功能必须取消！
  这个分支还是必须就地销毁
  git branch -d feature-vulcan
  销毁失败。Git友情提醒，feature-vulcan分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用命令git branch -D feature-vulcan。
  git branch -D feature-vulcan
16.多人协作
  当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。
  要查看远程库的信息，用git remote：
  git remote
  或者，用git remote -v显示更详细的信息：
  git remote -v
  origin	https://github.com/black-star32/gitskills.git (fetch)
  origin	https://github.com/black-star32/gitskills.git (push)
  上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址
  推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：
  git push origin master
  如果要推送其他分支，比如dev，就改成：
  git push origin dev
  但是，并不是一定要把本地分支往远程推送
  多人协作时，大家都会往master和dev分支上推送各自的修改。
  现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：
  git clone https://github.com/black-star32/gitskills.git
  当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的master分支。
  git status
  现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：
  git checkout -b dev origin/dev
  现在，他就可以在dev上继续修改，然后，时不时地把dev分支push到远程：
  git commit -m "add /usr/bin/env"
  你的小伙伴已经向origin/dev分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：
  git add hello.py
  git commit -m "add coding:utf-8"
  git push origin dev
  推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送：
  git pull
  git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：
  git branch
  再pull
  git pull
  这回git pull成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push：
  git commit -m "merge & fix hello.py"
  git push origin dev
17.创建标签
  首先，切换到需要打标签的分支上
  git branch 
  git checkout master
  然后，敲命令git tag <name>就可以打一个新标签：
  git tag v1.0
  可以用命令git tag查看所有标签：
  git tag
  默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？
  方法是找到历史提交的commit id，然后打上就可以了：
  git log --pretty=oneline --abbrev-commit
  比方说要对add merge这次提交打标签，它对应的commit id是6224937，敲入命令：
  git tag v0.9 6224937
  再用命令git tag查看标签：
  git tag
  注意，标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：
  git show v0.9
  可以看到，v0.9确实打在add merge这次提交上。
  还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
  git tag -a v0.1 -m "version 0.1 released" 3628164
  用命令git show <tagname>可以看到说明文字：
  还可以通过-s用私钥签名一个标签：
  git tag -s v0.2 -m "signed version 0.2 released" fec145a
  签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错：
  如果报错，请参考GnuPG帮助文档配置Key。
  用命令git show <tagname>可以看到PGP签名信息：
  git show v0.2
  用PGP签名的标签是不可伪造的，因为可以验证PGP签名。
18.操作标签
  如果标签打错了，也可以删除：
  git tag -d v0.1
  因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。
  如果要推送某个标签到远程，使用命令git push origin <tagname>：
  git push origin v1.0
  或者，一次性推送全部尚未推送到远程的本地标签：
  git push origin --tags
  如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
  git tag -d v0.9
  然后，从远程删除。删除命令也是push，但是格式如下：
  git push origin :ref/tags/v0.9
  要看看是否真的从远程库删除了标签，可以登陆GitHub查看。
以上为学习github使用时测试学习文件，原教程地址：https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000

