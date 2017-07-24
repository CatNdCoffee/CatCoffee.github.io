# Git入门

## 工作区/暂存区/版本库

- 仓库:Repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
- 工作区:WorkingDirectory,即能看到的目录
- 版本库:工作区下的一个隐藏目录(.git).版本库中有暂存区(stage/index)

## 基本命令

git init: 将某个目录创建为仓库(在目录路径下执行)
git add `<fileName>` :将某个文件添加进仓库
git commit: 提交
git status: 查看仓库当前状态
git diff: 查看difference,即修改内容
git log: 查看git日志(修改记录)
git reset (-hard) HEAD(^/^^/~100):版本回退
git reflog: 查看命令记录
git push: 将本地内容推送到远程仓库

## 远程仓库

一般远程仓库的默认名为origin

## 分支管理

git checkout -b dev: 创建并切换分支,相当于git branch dev + git checkout dev
git branch: 查看当前分支,该命令会列出所有分支,且当前分支会标有一个`*`
git beanch -d dev:删除分支
git merge: 合并指定分支到到当前分支

Git鼓励大量使用分支：

    查看分支：git branch

    创建分支：git branch <name>

    切换分支：git checkout <name>

    创建+切换分支：git checkout -b <name>

    合并某分支到当前分支：git merge <name>

    删除分支：git branch -d <name>
