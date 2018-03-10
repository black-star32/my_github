1.安装git
  sudo dnf -install git
2.创建版本库
  mkdir my_github
  cd my_github
  将这个目录变成git可以管理的仓库
  git init
  编写一个readme.tex
  把文件添加到仓库 git add readme.txt
  把文件提交到仓库 git commit -m "wrote a readme file"
  注:git添加文件分两步 add,commit
     commit可以一次性提交很多文件，所以可以多次add不同的文件
     如：
	git add file1.txt
	git add file2.txt file3.txt
	git commit -m "add 3 files"
