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
9.远程仓库
  第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key
  ssh-keygen -t rsa -C "youremail@example.com"
  你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
  第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：
  然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容
  登陆GitHub,创建一个新仓库
  git remote add origin https://github.com/black-star32/my_github.git
  git push -u origin master
