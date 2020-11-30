### what is Git?
Git 是一种分布式版本的版本控制系统
1. 优点：
   - 免费开源
   - 速度快，文件体积小：记录文件内容快照（snapshot）
   - 分布式系统；非专用服务器
### 终端命令（windows）
```
cd D:\???  绝对路径
cd ???     相对路径
cd ..      向上一级
cd         显示当前路径
```
#### 安装确认
```
$ git --version
git version 2.28.0.windows.1
```
#### 几个基本命令
```
$ ls -al 全显示列表
$ touch index.html 创建index文件
$mkdir demo 创建一个demo目录（文件夹）
$ cp index.html about.html 复制为about
$ mv index.html info.html 改名
$ rm info.html 删除

```
#### 用户设置
```
$ git config --global user.name "albert NIU" 用户名
$ git config --global user.email "albertniu.5@qq.com"邮箱
$ git config --list    检查
```
#### 设置命令缩写
```
$git config --global alias.co checkout  // git co=git checkout
```
#### 开始操作
```
$ mkdir git_practice   # 新建目录
$ cd git_practice      
$ git init             #初始化目录并让Git进行版控
$ git status

```
### No commits yet#TODO
```
$ echo "second trial" > second.txt  # 新建second.txt文件并写入second trial
$ git status 显示状态
$ git add second.txt # 将second.txt加入到暂存区（Staging Area）
$ git commit -m "init commit" # 提交到仓库并说明init commit
$ git commit --allow-empty -m "空的"   # 无内容的commit，但是有说明
```
```
$ git log  # 查看commit记录
commit c4c996fc8c55d5581ad40f05b0739d8f9f981761 (HEAD -> master)
Author: albert NIU <albertniu.5@qq.com>
Date:   Wed Aug 5 11:27:16 2020 +0800

    create index page

commit b24456ec58a89993da2cb8d950448646b0d23b07 (origin/master)
Author: albert NIU <albertniu.5@qq.com>
Date:   Wed Aug 5 11:17:55 2020 +0800

    Create second.txt


$ git log --oneline --graph # 加入更多参数的查询
* c4c996f (HEAD -> master) create index page
* b24456e (origin/master) Create second.txt

$ git log --oneline --author="albert NIU"  # 查询某人
c4c996f (HEAD -> master) create index page
b24456e (origin/master) Create second.txt

$ git log --oneline --grep="some" commit关键字查询
$ git log --oneline --since="9am" --until="12am" --after="2020-8-5" #时间查询
```

#### 在Git中删除和更改
1. 直接删除
```
$ rm world.html
```
2. 使用Git删除
```
$ git rm second.txt
```
3. 文件不再受Git控制
```
$ git rm third.html --cached
```
4. 变更文件名
```
$ git mv third.html third3.html
```
文件名称不重要，重要的是内容 #TODO 文件的对象

#### 修改Commit记录
1. 只改动最新的记录
```
$ git commit --amend -m "oh this is brand new one"
```

2. 改动更早的记录：
```
rebase in 7.1 #TODO
```
#### 追加文件到Commit
```
$ git commit --amend --no-edit
```
由于Git是按照文件的内容进行计算的，因此不能处理单独的新增目录

#### 忽略某些文件
需要在文件目录中新建和一个.gitignore文件，将文件名输入；
本规则仅对规则设置后存入的文件有效

#### 查看单独文件的Commit记录
```
$ git log xxx.html
$ git log -p xxx.html  # 查看每次的commit做出了怎样的改动
```

#### 查看代码的提交识别码
```
$ git blame -L 5,10 index.html # 只显示5到10行
$ git blame index.html         # 全部显示
```

#### 使用rm删除文件之后的恢复
```
$ git checkout xxx.html
$ git checkout .                # 恢复所有文件
```
#### Reset重做
```
$ git reset XXXXXX(sha-1码)
$ git reset XXXXX^(退回前一次)
$ git reset XXXXX^^(退回前两次)
$ git reset XXXXX~5(退回前五次)
```
reset的模式
1. --mixed  删除暂存区，不回影响工作目录
2. --soft   暂存区和工作目录都不删除，近HEAD移动
3. --hard   工作区和暂存区都删除

恢复reset前的记录
```
$ git reflog
$ git reset XXXXX(reset的记录sha-1码)
```
### What is HEAD
HEAD 是指向某个分支“当前所在分支”



















### 使用分支（branch）
1. create new branch
```
$ git branch neo
```
2. change the name of the branch, even the master branch
```
$ git branch -m neo new
```
3. delete the branch
```
$ git branch -d new
```
4. forced delete
```
 $ git branch -D new
```
5. change branch
```
$ git checkout new2
Switched to branch 'new2'
A       fourth.html
A       fourth2.html
D       third.html
D       third2.html
```
when your branch is changed, it means that your staging area and working directory will be "updated".

the changes in working directroy won't be changed by changing branches

6. 合并分支
```
$ git merge new2
```
7. 合并方式2：rebase
8. 取消rebase
(1) using reflog
(2) ORIG_HEAD记录危险操作状态之前的位置 