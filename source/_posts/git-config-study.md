title: git学习笔记
date: 2016-05-01 08:24:45
tags: others
categories: 学习笔记
---
** git学习笔记：** <Excerpt in index | 首页摘要>
	git的常用操作，高级技巧都要哦
<!-- more -->
<The rest of contents | 余下全文\>

### 安装git
1. 下载安装包 ￼下载地址￼
2. 安装git
3. 进入命令行,输入git看看是否成功

### 配置git
1. 配置全局用户名和密码
	\`git config --global user.name "John Doe"
	git config --global user.email johndoe@example.com
	\`
2. 配置ssh公钥
	`cd ~/.ssh` 然后`ls`
	如果没有,直接生成,一路点击enter
	\`\`\`
	ssh-keygen
	cat \~/.ssh/id\_rsa.pub
	\`\`\`
	把公钥配置到github的个人设置

### 常用的命令
1. repository操作
	- 检出（clone）仓库代码：`git clone repository-url` / `git clone repository-url local-directoryname`
		+ 例如，clone jquery 仓库到本地： `git clone git://github.com/jquery/jquery.git`
		+ clone jquery 仓库到本地，并且重命名为 my-jquery ：`git clone git://github.com/jquery/jquery.git my-jquery`
	- 查看远程仓库：`git remote -v`
	- 添加远程仓库：`git remote add [name] [repository-url]`
	- 删除远程仓库：`git remote rm [name]`
	- 修改远程仓库地址：`git remote set-url origin new-repository-url`
	- 拉取远程仓库： `git pull [remoteName] [localBranchName]`
	- 推送远程仓库： `git push [remoteName] [localBranchName]`

2. 提交/拉取/合并/删除
	- 添加文件到暂存区（staged）：`git add filename` / `git stage filename`
	- 将所有修改文件添加到暂存区（staged）： `git add --all` / `git add -A`
	- 提交修改到暂存区（staged）：`git commit -m 'commit message'` / `git commit -a -m 'commit message'` 注意理解 -a 参数的意义
	- 从Git仓库中删除文件：`git rm filename`
	- 从Git仓库中删除文件，但本地文件保留：`git rm --cached filename`
	- 重命名某个文件：`git mv filename newfilename` 或者直接修改完毕文件名 ，进行`git add -A && git commit -m 'commit message'` Git会自动识别是重命名了文件

	- 获取远程最新代码到本地：`git pull (origin branchname)` 可以指定分支名，也可以忽略。pull 命令自动 fetch 远程代码并且 merge，如果有冲突，会显示在状态栏，需要手动处理。更推荐使用：`git fetch` 之后 `git merge --no-ff origin branchname` 拉取最新的代码到本地仓库，并手动 merge 。

3. 日志查看
	- 查看日志：`git log`
	- 查看日志，并查看每次的修改内容：`git log -p`
	- 查看日志，并查看每次文件的简单修改状态：`git log --stat`
	- 一行显示日志：`git log --pretty=oneline` / `git log --pretty='format:"%h - %an, %ar : %s'`
	- 查看日志范围：
		+ 查看最近10条日志：`git log -10`
		+ 查看2周前：`git log --until=2week` 或者指定2周的明确日期，比如：`git log --until=2015-08-12`
		+ 查看最近2周内：`git log --since=2week` 或者指定2周明确日志，比如：`git log --since=2015-08-12`
		+ 只查看某个用户的提交：`git log --committer=user.name` / `git log --author=user.name`
4. 取消操作
	- 上次提交msg错误/有未提交的文件应该同上一次一起提交，需要重新提交备注：`git commit --amend -m 'new msg'`
	- 一次`git add -A`后，需要将某个文件撤回到工作区，即：某个文件不应该在本次commit中：`git reset HEAD filename`
	- 撤销某些文件的修改内容：`git checkout -- filename` 注意：一旦执行，所有的改动都没有了，谨慎！谨慎！谨慎！
	- 将工作区内容回退到远端的某个版本：`git reset --hard <sha1-of-commit>`
		+ `--hard`：reset stage and working directory ,<commitid> 以来所有的变更全部丢弃，并将 HEAD 指向<commitid>
		+ `--soft`：nothing changed to stage and working directory ,仅仅将HEAD指向<commitid> ，所有变更显示在”changed to be committed”中
		+ `--mixed`：default,reset stage ,nothing to working directory ，这也就是第二个例子的原因

5. 比较差异
	- 查看工作区（working directory）和暂存区（staged）之间差异：`git diff`
	- 查看工作区（working directory）与当前仓库版本（repository）HEAD版本差异：`git diff HEAD`
	- 查看暂存区（staged）与当前仓库版本（repository）差异：`git diff --cached` / `git diff --staged`

6. 合并操作
	- 解决冲突后/获取远程最新代码后合并代码：`git merge branchname`
	- 保留该存在版本合并log：`git merge --no-ff branchname` 参数`--no-ff`防止 fast-forward 的提交
