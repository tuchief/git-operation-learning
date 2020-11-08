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

**git和git flow实际场景：**

1、查看本地代码是否有变化：git status
清除本地代码的变更：git checkout -- src
      
2、项目提测，结束feature分支，拉取release分支：
// 初始化git flow
git flow init

```
// 结束feature/1.0.0分支，代码将自动合并到dev分支（确保本地的dev分支和远程dev分支已建立连接），此后会自动切换到dev分支
git flow feature finish 1.0.0
 
// dev分支代码推到远程
git push
 
// 创建新的release分支，此后自动切换到release分支
git flow release start 1.0.0
 
// 本地代码推向远程（会让你建立连接）
git push
 
// 本地和远程release分支建立连接
git push --set-upstream origin release/1.0.0
```
 
