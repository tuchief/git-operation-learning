**推荐阅读：** [A Successful Git Branching Model](https://nvie.com/posts/a-successful-git-branching-model/)

**git flow主要分支**

**master**：永远处于production-ready状态
* 主分支，产品的功能全部实现后，最终在master分支对外发布；
* 只读分支，只能从release或hotfix分支合并，不能修改；
* 所有在master分支的推送应该做标签记录，方便追溯。

**develop**：最新的下次发布的开发状态
* 主开发分支，基于master分支克隆，发布到下一个release；
* 只读分支，feature功能分支完成，合并到develop（不推送）；
* develop拉取release分支，提测；
* release/hotfix分支上线完毕，合并到develop并推送。

**feature**：开发新功能都从develop分支出来，完成后merge回develop
* 功能开发分支，基于develop分支克隆，用于新需求的开发；
* 功能开发完毕后合并到develop分支（未正式上线之前不能推送到远程中央仓库）
* feature可以同时存在多个，用于团队多功能同步开发，属于临时分支，开发完毕后可以删除。

**release**：准备要release的版本，只修bug。从develop出来，完成后merge回master和develop
* 测试分支，feature分支合并到develop分支之后，从develop分支克隆；
* 只要用于提交给测试人员进行功能测试，测试过程中如果发现BUG在release分支修复，修复完成上线后合并到develop/master分支并推送完成，做标签记录；
* 临时分支，上线后可删除。

**hotfix**：等不及release版本就必须马上修复master上线。从master出来，完成后merge回master和develop
* 补丁分支，基于master分支克隆，主要用于对线上的版本进行BUG修复；
* 修复完毕后合并到develop/master分支并推送，做标签记录；
* 所有hotfix分支的修改会进入到下一个release；
* 临时分支，补丁修复上线后可以删除。

![git-flow工作流程图](git-flow.png)
 
**主要工作流程：**

* 初始化项目为git flow，默认创建master分支，然后从master拉取第一个develop分支；
* 从develop拉取feature分支进行功能开发（多个feature分支同时进行）；
* feature分支完成后，合并到develop（不推送，还未提测，会影响其他功能分支的开发），当前feature分支不可修改，必须从release分支继续编码修改；
* 从develop拉取release分支提测，提测过程中在release分支上修改BUG；
* release分支上线后，合并到develop和master并推送；
* 上线后发现bug，从master拉取hotfix进行bug修复；
* hotfix通过测试后上线；
 

**常用指令：**
```
开分支    git branch dev
切换分支   git checkout dev
开分支且切换   git checkout -b dev
切换回原分支   git checkout master
合并分支   git merge dev
查看本地分支列表   git branch -a
查看远程分支列表   git branch -r
向远程提交本地新开分支   git push origin dev
删除远程分支   git push origin -d feature/1.1.0
删除本地分支   git branch --delete feature/1.1.0
更新分支列表   git fetch -p
打tag标签  git tag 1.1.0
上传tag标签到远程  git push --tags
git flow新增分支  git flow feature start 1.1.0
将本地分支推向远程   git push --set-upstream origin feature/1.1.0
git flow结束分支 git flow feature finish 1.1.0
``` 

**git flow代码示例：**

```
a. 创建develop分支

git branch develop
git push -u origin develop 

b. 开始新Feature开发

git checkout -b some-feature develop
# Optionally, push branch to origin:
git push -u origin some-feature    

# 做一些改动    
git status
git add some-file
git commit 

c. 完成Feature

git pull origin develop
git checkout develop
git merge --no-ff some-feature
git push origin develop

git branch -d some-feature

# If you pushed branch to origin:
git push origin --delete some-feature   

d. 开始Relase

git checkout -b release-0.1.0 develop

# Optional: Bump version number, commit
# Prepare release, commit

e. 完成Release

git checkout master
git merge --no-ff release-0.1.0
git push

git checkout develop
git merge --no-ff release-0.1.0
git push

git branch -d release-0.1.0

# If you pushed branch to origin:
git push origin --delete release-0.1.0   

git tag -a v0.1.0 master
git push --tags

f. 开始Hotfix

git checkout -b hotfix-0.1.1 master    

g. 完成Hotfix

git checkout master
git merge --no-ff hotfix-0.1.1
git push

git checkout develop
git merge --no-ff hotfix-0.1.1
git push

git branch -d hotfix-0.1.1

git tag -a v0.1.1 master
git push --tags
```
**使用git flow工具示例：**
```
#初始化:   
git flow init

#开始新Feature:   
git flow feature start MYFEATURE

#Publish一个Feature(也就是push到远程):   
git flow feature publish MYFEATURE

#获取Publish的Feature:   
git flow feature pull origin MYFEATURE

#完成一个Feature:   
git flow feature finish MYFEATURE

#开始一个Release:   
git flow release start RELEASE [BASE]

#Publish一个Release:   
git flow release publish RELEASE

#发布Release:   
git flow release finish RELEASE  
#别忘了
git push --tags

#开始一个Hotfix:   
git flow hotfix start VERSION [BASENAME]

#发布一个Hotfix:   
git flow hotfix finish VERSION

```
 
