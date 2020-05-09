[TOC]
# 1. 初始设置
## 1.1. 设置用户名
    $ git config --global user.name "John Doe"
    $ git config --global user.email johndoe@example.com
# 2. 仓库操作
## 2.1. 克隆远端仓库到本地
    git clone -l url
## 2.2. 更改git路径
    git remote set-url origin git@git.wx.bc:/rhine/source/client.git
## 2.3. 查看分支
* 查看所有分支
    
        git branch -a
* 查看本地分支

        git branch

* 查看远端分支

        git branch -r

        
## 2.4. git拉取远程分支并创建本地分支
    git checkout -b newBrach origin/master
## 2.5. 新建本地分支
    git checkout -b dbg_lichen_star
## 2.6. 本地分支push到远端
    git push origin dbg_lichen_star:dbg_lichen_star
## 2.7. 分支重命名<sup>[3]</sup>
1. 切换到分支test1
git checkout test1
2. 在本地新建分支test2
git branch test2
3. 切换到新分支
git checkout test2
4. 把本地分支push到远程
git push origin test2
5. 删除远端test1分支
git push origin :test1
6. 删除本地test1分支
git branch -d test1
7. 查看所有分支(确认本地及远端test1已经删除，并且已经新增test2)
git branch -a

## 2.8. gitflow拉研发分支场景
### 方案1

#### 2.8.2. 新建本地分支
    git checkout -b dbg_lichen_star
#### 2.8.3. 本地分支push到远端
    git push origin dbg_lichen_star:dbg_lichen_star
#### 2.8.3. 建立远程分支连接
    git branch --set-upstream-to=origin/dev/cns dev/cns
        
### 2.8.1. git拉取远程分支并创建本地分支
    git checkout -b newBrach origin/master

### 方案2<sup>[4]</sup>
这里共展示两类三种方式。

1.git pull：获取最新代码到本地，并自动合并到当前分支
命令展示

//查询当前远程的版本
$ git remote -v
//直接拉取并合并最新代码
$ git pull origin master [示例1：拉取远端origin/master分支并合并到当前分支]
$ git pull origin dev [示例2：拉取远端origin/dev分支并合并到当前分支]

分析：不推荐这种方式，因为是直接合并，无法提前处理冲突。

2.git fetch + merge: 获取最新代码到本地，然后手动合并分支
2.1.额外建立本地分支
代码展示

//查看当前远程的版本
$ git remote -v 
//获取最新代码到本地临时分支(本地当前分支为[branch]，获取的远端的分支为[origin/branch])
$ git fetch origin master:master1  [示例1：在本地建立master1分支，并下载远端的origin/master分支到master1分支中]
$ git fetch origin dev:dev1[示例1：在本地建立dev1分支，并下载远端的origin/dev分支到dev1分支中]
//查看版本差异
$ git diff master1 [示例1：查看本地master1分支与当前分支的版本差异]
$ git diff dev1    [示例2：查看本地dev1分支与当前分支的版本差异]
//合并最新分支到本地分支
$ git merge master1    [示例1：合并本地分支master1到当前分支]
$ git merge dev1   [示例2：合并本地分支dev1到当前分支]
//删除本地临时分支
$ git branch -D master1    [示例1：删除本地分支master1]
$ git branch -D dev1 [示例1：删除本地分支dev1]
1


备注：不推荐这种方式，还需要额外对临时分支进行处理。

2.2.不额外建立本地分支
代码展示

//查询当前远程的版本

    $ git remote -v
//获取最新代码到本地(本地当前分支为[branch]，获取的远端的分支为[origin/branch])
    
    $ git fetch origin master  [示例1：获取远端的origin/master分支]
    $ git fetch origin dev [示例2：获取远端的origin/dev分支]
//查看版本差异

    $ git log -p master..origin/master [示例1：查看本地master与远端origin/master的版本差异]
    $ git log -p dev..origin/dev   [示例2：查看本地dev与远端origin/dev的版本差异]
//合并最新代码到本地分支

    $ git merge origin/master  [示例1：合并远端分支origin/master到当前分支]
    $ git merge origin/dev [示例2：合并远端分支origin/dev到当前分支]
    
备注：推荐这种方式
--------------------- 
作者：hanchao5272 
来源：CSDN 
原文：
## 2.9. 关联本地分支与远程分支
git branch --ser-upstream-to=origin/phase-0 phase-0

## 2.10. 删除本地分支

git branch -d <BranchName>
# 3. 对比
## 3.1. 文件状态
    git status
## 3.2. 文件更改
    git diff
# 4. 回滚
## 4.1. 强制覆盖本地的
本地的替换成远程的，用下面的命令

git fetch --all
git reset --hard origin/master (这里master要修改为对应的分支名)
git pull
## 4.2. 本地回滚<sup>[1]</sup>
* git reset

    1、回退到某次提交（例：22f8aae 为某次提交的提交号）

        $ git reset --hard   22f8aae

    2、回退n次提交（例：n=3）
    
        $ git reset --hard HEAD~3
* git revert
    
    1、仅回退以前的某次提交（例：c011eb3c20ba6fb38cc94fe5a8dda366a3990c61为某次提交的hash值）
 
        $ git revert c011eb3c20ba6fb38cc94fe5a8dda366a3990c61
## 4.3. 远程回滚<sup>[1]</sup>
1、先进行本地回滚   
2、提交到远程 
    需先去除master的保护（protected branch）

    git push -u origin master -f 

# 5. 更改历史
## 5.1. 查看本地已经commit，但未push的版本

git cherry -v
## 5.2. git 查看 本地仓库的commit记录：
    git  log -n 1 --stat
## 5.3. 查看提交历史
    git log
## 5.4. 更改最近一次commit记录
$ git commit --amend

# 6. tag
列显已有的标签
列出现有标签的命令非常简单，直接运行 git tag 即可：

$ git tag
v0.1
v1.3
显示的标签按字母顺序排列，所以标签的先后并不表示重要程度的轻重。

我们可以用特定的搜索模式列出符合条件的标签。在 Git 自身项目仓库中，有着超过 240 个标签，如果你只对 1.4.2 系列的版本感兴趣，可以运行下面的命令：

$ git tag -l 'v1.4.2.*'
v1.4.2.1
v1.4.2.2
v1.4.2.3
v1.4.2.4

[1] http://blog.csdn.net/lovesummerforever/article/details/71526900
[2] https://mp.weixin.qq.com/s/oQR6KYaT-YuTNAnOOjjiFQ
[3]https://git-scm.com/book/zh/v1/Git-分支-远程分支
[4]https://blog.csdn.net/hanchao5272/article/details/79162130 