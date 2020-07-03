# Git 的结构和状态

1. Git 的3层结构
   - working directory 工作区
   - staging index 暂存区
   - git directory (Repository) 版本库
2. Git 中文件的4种状态
   - Untracked 未被追踪
   - Modified（红色Modified） 表示工作区修改了某个文件但是还没有添加到暂存区
   - Staged（绿色Modified） 表示把工作区修改的文件添加到了暂存区但是没有提交版本库
   - Committed 表示数据被安全地存储在本地库中

# Git命令

## git init
- 在当前文件夹下初始化git仓库：`git init`



## git config

- 查看用户配置：`git config --list`
  - 当前项目配置：`cat .git/config`
  - global配置：`cat ~/.gitconfig`
- 配置用户名：`git config --global user.name 用户名`
- 配置用户邮箱：`git config --global user.email 邮箱`



## git status
- 查看状态：`git status`



## git add
- 工作区添加到暂存区：`git add 文件名`、`git add .`



## git commit
- 暂存区提交到版本库：
  - `git commit [文件名]`
  - `git commit -m '版本描述' [文件名]`
- 将工作区中被跟踪过的文件添加到版本库：
  - `git commit -am '版本描述'`
- 撤销上一次提交，并将暂存区的文件重新提交到版本库：`git commit --amend`



## git log

- 备注：
  - 多屏显示控制方式：`空格`向下翻页，`b`向上翻页，`q`退出

- 查看日志：
  - 详细日志：`git log`
  - 简短索引值单行显示`git log --oneline`
  - 完整索引值单行显示`git log --pretty=oneline`
    - 设置范围
      - `git log -n --oneline`（n为最近n次提交的日志）
      - `git log HEAD^^..HEAD --oneline`
      - `git log 版本号..版本号 --oneline`
  - 带步数简短索引值单行显示：`git reflog`
    - `git reflog -n` （n为最近n次提交的日志）



## git reset

- 基于索引值操作：
  - 局部索引值：`git reset --hard a6ace91 [文件名]` 
  - 回退到版本库HEAD指针指向的版本：`git reset --hard HEAD`
  - 后退n步：`git reset --hard HEAD^` （n个`^`退n步）
  - 后退n步：`git reset --hard HEAD~n` 
- reset的三个参数
  - `--soft`：仅仅在本地库移动HEAD指针
  - `--mixed`：在本地库移动HEAD指针，重置暂存区
  - `--hard`：在本地库移动HEAD指针，重置暂存区，重置工作区
  - 无以上三个参数，只操作暂存区
    - 把该文件在版本库中HEAD指针指向的版本拉回到暂存区：`git reset HEAD -- 文件名`
    - 把该文件在版本库中指定的版本拉回到暂存区：`git reset 版本号 文件名`
- 备注
  - 本地库和暂存区不一致，暂存区不显示
  - 工作区和暂存区一致，暂存区绿色
  - 工作区和暂存区不一致，暂存区红色



## git rm

- 删除文件：
  - 取消对文件的跟踪（tracked→untracked）：`git rm --cached 文件名`
  - 若该文件已经在版本库中，文件从工作区中删除，并在暂存区中产生删除记录：`git rm 文件名`
  - 若该文件不在版本库中，强制在工作区和暂存区中删除该文件：`git rm -f 文件名`
  - 只将文件从工作区中删除，删除记录未add到暂存区：`rm 文件名`
- 备注
  - 工作区的删除操作，经过add到暂存区，再经过commit到本地版本库后，本地库会记录这条删除操作。但是只要曾经在版本库中存的文件就“永不磨灭”。意思是它只是记录了你删除的操作，但文件并没有真正从版本库中删除，所以可以随时找回来。

## git mv

- 文件重命名：`git mv 旧文件名 新文件名`
  - 相当于`git rm 旧文件名`、`git add 新文件名`



## git diff

- 与版本库比较：`git diff [文件名]`
- 与版本库HEAD指向的版本比较：`git diff HEAD [文件名]`
- 与版本库指定版本号指向的版本比较：`git diff 版本号 [文件名]`



## git branch

- 查看所有分支：`git branch -v`
- 新建分支：`git branch 分支名`
- 删除分支：`git branch -d 分支名`



## git checkout

- 切换分支：`git checkout 分支名`
- 用另一分支的文件覆盖当前分支下的同名文件：git checkout 另一分支名 -- 文件名`
- 当前分支下的指定文件被版本库HEAD指向的版本的同名文件覆盖：`git checkout -- 文件名`



## git merge

- 合并分支
  - 第一步，切换到接受修改的分支上（被合并）
  - 第二步，执行merge命令，把指定分支合并到当前分支上：`git merge 分支名`
- 解决合并冲突
  - 第一步：编辑文件，删除特殊负号，保存退出
  - 第二步：`git add [文件名]`
  - 第三步：`git commit -m '日志信息'`



## git push

- 向远程库推送：`git push 地址或地址别称 本地某分支名称` 



## git remote

- 查看已保存的地址：`git remote -v`
- 添加地址：`git remote add 地址别称 地址`
- 重命名地址：`git remote rename 旧地址别称 新地址别称`
- 删除地址：`git remote remove 地址别称`



## git clone

- 克隆远程库：`git clone 远程库地址`
  - 完整地把远程库下载到本地
  - 创建origin远程地址别称
  - 初始化本地库



## git pull

- pull = fetch + merge：`git pull 远程库别名 远程库分支名 `
  - fetch：从远程库下载分支，并开辟一个命名为`远程库别名/远程库分支名`的分支，不改变工作区文件：`git fetch 远程库别名 远程库分支名`
    - 转到新开辟的分支：`git checkout 远程库别名/远程库分支名`
    - 回到本地原分支：`git checkout 远程库分支名`
  - merge：把下载下来的远程库合并到工作区：`git merge 远程库别名/远程库分支名` `





## 解决冲突

- 当远程库的修改与本地库的修改冲突时，是不能直接`push`到远程库的
  - 先`pull`下来
  - 在本地解决冲突
  - 再`push`到远程库



# 其它

将默认编辑器设置为vim：`git config --global core.editor vim`

将默认编辑器设置为nano：`git config --global core.editor nano`

显示帮助文档：`git help`

凭据管理器删除原GitHub账号：控制面板→凭据管理器

> 在Git中，origin / master与origin master之间有什么区别？
>
> https://blog.csdn.net/u014599371/article/details/90055737

> git merge一个指定文件
>
> https://www.jianshu.com/p/3b464a848538

- 以下用法有待商榷
  - `git checkout -- 文件名`
    - 如果暂存区中没有该文件，则把该文件从版本库中拉回到工作区
    - 如果暂存区有该文件，则把该文件从暂存区中拉回到工作区

> 命令行说明中格式 尖括号 中括号的含义
>
> https://blog.csdn.net/x356982611/article/details/84852039

> 修改日志
>
> https://www.cnblogs.com/chucklu/p/4721404.html

> git merge 之后文件被删除
>
> https://blog.csdn.net/chent86/article/details/83033585


