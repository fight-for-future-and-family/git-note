```
git init
git config --system/--global/--local  --list
git config --global user.name   "wangliang"
git config --global user.email  "1010882388@qq.com"
git config --global core.editor vim
git config --global merge.tool vimdiff
git config --list

## 使用git config命令编辑配置文件
仓库级的config   git config –local -e
全局级的config   git config –global -e
系统级的config   git config –system -e
## 增加一个配置项
格式: git config [–local|–global|–system] –add section.key value(默认是添加在local配置中)  
     git config --add my.name wangliang
     git config --get my.name
## 删除一个配置项
格式：git config [–local|–global|–system] –unset section.key
     git config --unset my.name
```
##### 忽略某些文件 ,创建一个名为 .gitignore 的文件   
```
# 此为注释 – 将被 Git 忽略
*.a # 忽略所有 .a 结尾的文件
!lib.a # 但 lib.a 除外
/TODO # 仅仅忽略项目根目录下的 TODO 文件,不包括 subdir/TODO
build/ # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```
##### 要查看尚未暂存的文件更新了哪些部分,此命令比较的是工作目录中当前文件和暂存区域快照之间的差异,不加参数直接输入
> git diff
##### 若要看已经暂存起来的文件和上次提交时的快照之间的差异,
> git diff --staged  /   git diff --cached

#### 移除文件
> git rm 命令完成此项工作,从暂存区域移除,并连带从工作目录中删除指定的文件

#### 另外一种情况是,我们想把文件从 Git 仓库中删除(亦即从暂存区域移除),但仍然希望保留在当前工作目录中。
>git rm --cached readme.txt
#### 移动文件 -------  其实,运行就相当于运行了下面三条命令:
>git mv file_from file_to 
===  
mv README.txt README  
git rm README.txt  
git add README  

#### 查看提交历史,2.1 列出了常用的格式占位符写法及其代表的意义。
>选项 说明 
%H 提交对象(commit)的完整哈希字串  
%h 提交对象的简短哈希字串  
%T 树对象(tree)的完整哈希字串  
%t 树对象的简短哈希字串  
%P 父对象(parent)的完整哈希字串  
%p 父对象的简短哈希字串  
%an 作者(author)的名字  
%ae 作者的电子邮件地址  
%ad 作者修订日期(可以用 -date= 选项定制格式)  
%ar 作者修订日期,按多久以前的方式显示  
%cn 提交者(committer)的名字  
%ce 提交者的电子邮件地址  
%cd 提交日期  
%cr 提交日期,按多久以前的方式显示  
%s 提交说明  
git log --pretty=format:"%h %s" --graph

#### 修改最后一次提交
git commit --amend -m "to modify the last commit"
#### 取消已经暂存的文件
>$ git reset HEAD benchmarks.rb  
git reset --mixed HEAD git_常用命令.md
#### checkout强调，替换  
>git checkout [- -] filename 
\- 用暂存区的内容替换工作区的文件。比如working directory修改了，但是想放弃这些修改，那么使用git checkout -- a.txt 放弃对a.txt的修改   
git checkout head filename   
\- 用head指向的目录（版本库）替换暂存区和工作区的文件  



git remote add wlgit git@github.com:fight-for-future-and-family/git-note.git

git add file
git commit -m "the changes of this time"

git status
git branch
git log

---
#### the file is ~/.gitconfig
```
[alias]
        lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
        lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
        lg = !"git lg1"
```
---
```
git checkout master/bugFix/wl
git checkout C1
git branch wl
git checkout -b wl
git branch -f new-branch C9

git rebase master
git cherry-pick   C3 C8 C10
git rebase -i C5 C3 C2 C4            /      git rebase -i HEAD~6

## local
git reset bugFix          
## remote
git revert bugFix
```

### 删除受git控制的文件
##### 1. You want to keep the file locally
Amend the last commit to remove the file from the repository, and add it to .gitignore, to prevent it from being added by accident again.
```
git rm --cached $FILE
echo $FILE >> .gitignore
git add .gitignore
git commit --amend --no-edit
git reflog expire --expire=now --all && git gc --prune=now --aggressive
The git reflog expire and git gc commands force a garbage collection, to keep the file from dangling somewhere in your repository.
```
##### 2. You do not want to keep the file locally
Just amend the last commit.
```
git rm $FILE
git commit --amend --no-edit
git reflog expire --expire=now --all && git gc --prune=now --aggressive
```
