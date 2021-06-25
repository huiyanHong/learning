# Git学习笔记

## Git介绍

**分布式版本控制系统**（Distributed Version Control System，简称 DVCS）

客户端并不只提取最新版本的文件快照，而是把代码仓库**完整地镜像**下来。 这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。

直接**记录快照**，而**非差异比较**

近乎所有操作都是**本地执行**

保证**完整性**（用SHA-1散列哈希计算校验和）

一般只**添加**数据

## Git基本概念

### 四个工作区域

![img](C:\Users\HUIYAN~1\AppData\Local\Temp\企业微信截图_16242428818073.png)

工作目录、暂存区域、Git仓库、远端仓库

Git仓库目录： Git 用来保存项目的元数据和对象数据库的地方。从其它计算机克隆仓库时，拷贝的就是这里的数据。

工作目录：对项目的**某个版本**独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在**磁盘**上供你使用或修改。

暂存区域：是一个文件，保存了下次将提交的文件列表信息

#### Git基本工作流程

1. 在工作目录中修改文件
2. 暂存文件，将文件的快照放到暂存区
3. 将更新从暂存区提交到仓库，将快照永久性存储到 Git 仓库目录。
4. 从本地仓库推送到远端仓库

### 三种状态

**已提交(committed)**: 数据已经安全的保存在本地数据库中

**已修改(modified)**:修改了文件，但还没保存到数据库中

**已暂存(staged)**:对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

#### 状态转移命令行

![img](C:\Users\HUIYAN~1\AppData\Local\Temp\企业微信截图_16242432734716.png)

## 安装与配置

### 首次安装后的配置

设置用户名：git config --global user.name "用户名"

设置邮箱：git config --global user.email "用户邮箱"

查看设置：git config --list

删除某项设置：git config --global --unset user.email

### 创建一个Git仓库

本地仓库：git init

远程仓库：git.code.oa.com

## Git常用命令与操作

1. **克隆仓库**：git clone <远程仓库URL> [本地仓库名]

   举例: git clone http://git.code.oa.com/scm/cmo_doc.git cmdoc

2. **更新仓库**：git pull -r （增加-r项目库log更干净）

3. **状态查看**：git status [-s]

   ??:新添加的未跟踪文件

   A: 新添加到暂存区中的文件

   M:修改过的文件.出现在右边的 `M` 表示该文件被修改了但是还没放入暂存区，出现在靠左边的 `M` 表示该文件被修改了并放入了暂存区。

   

4. **新增与修改**：

   新增文件或文件夹到暂存区：git add <文件或目录>

   修改文件到暂存区：git add <文件>

   添加所有文件：git add.

5. **提交暂存区的文件**：git commit -m

6. **忽略文件**：

   创建一个名为.gitignore的文件，列出要忽略的文件模式。

   ```
   $ cat .gitignore
   *.[oa]
   *~
   ```

   第一行告诉 Git 忽略所有以 `.o` 或 `.a` 结尾的文件。

   所有空行或者以 `＃` 开头的行都会被 Git 忽略。

   要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（`!`）取反。

   可以使用标准的 glob 模式匹配。（所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号（`*`）匹配零个或多个任意字符；`[abc]` 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；问号（`?`）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 `[0-9]` 表示匹配所有 0 到 9 的数字）。 使用两个星号（`*`) 表示匹配任意中间目录，比如`a/**/z` 可以匹配 `a/z`, `a/b/z` 或 `a/b/c/z`等。）

7. **查看已暂存和未暂存的修改**：git diff

   对比工作空间和暂存区：git diff

   对比工作空间和特定提交：git diff <commit>

   对比暂存区和特定提交：git diff --staged <commit>

   ​                                        git diff --cached <commit>

   对比两次提交：git diff <commit1> <commit2>

   要查看尚未暂存的文件更新了哪些部分: git diff (比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来的变化内容。)

   查看已暂存的将要添加到下次提交里的内容: git diff --cached

8. **跳过使用暂存区域,自动把所有已经跟踪过的文件暂存起来一并提交**，从而跳过 `git add` 步骤： git commit -a

9. **移除文件**，从工作目录中删除并记录在暂存区：git rm <文件或目录>

   如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 `-f`（译注：即 force 的首字母）。 

   从暂存区域移除，但保留在当前工作目录：git rm --cached 

10. **移动或重命名**： git mv <文件或目录> <新名称>

11. **查看提交历史**： git log

     `-p`，用来显示每次提交的内容差异。 

    `-2` 来仅显示最近两次提交。

    `--stat` 选项显示每次提交的简略的统计信息

     `--pretty` 可以指定使用不同于默认格式的方式展示提交历史。比如用 `oneline` 将每个提交放在一行显示，查看的提交数很大时非常有用。git log --pretty=online 另外还有 `short`，`full` 和 `fuller` 可以用，展示的信息或多或少有些不同。 format，可以定制要显示的记录格式。

     `--since` 和 `--until` ：按照时间作限制的选项。如`--since=2.weeks`。

    `--author`:仅显示指定作者相关的提交。

    `--committer`:仅显示指定提交者相关的提交。

    `--grep`:仅显示含指定关键字的提交。

12. **搜索内容**：

    git grep <待搜索字符串>：查找字符串出现的文件

    git grep  -n <待搜索字符串>：查找字符串出现的文件及次数

13. **撤销修改**

    回滚未添加到暂存区的文件,本地未提交的内容都丢弃掉：git checkout --<文件名>

    回滚已添加到暂存区的文件,撤销到工作区： git reset HEAD <文件名>、

    回滚已添加到本地仓库的文件： git reset <要回滚到的commitID> （回滚提交记录，但本地代码不回滚）；git reset --hard <要回滚到的commitID>（回滚提交记录和本地代码）

    回滚已推送到远端的记录：git revert <要回滚的commitID>

    **回退本地代码到当前最新提交** git reset --hard HEAD

    回退到上次提交 git reset --hard HEAD^

    回退到上n次提交 git reset --hard HEAD~n

14. **本地分支创建**：git branch <分支名> 这会在当前所在的提交对象上创建一个指针。

    git checkout -b <分支名>（新建分支并切换）

15. **查看各个分支当前所指的对象**： git log --decorate

16. **分支切换**：git checkout <分支名> 分支切换会改变你工作目录中的文件。

17. **新增远端仓库的分支**：在git.code.oa.com上的仓库主页面申请创建，然后本地同步：git fetch -p；本地创建，然后git push origin <分支名>

18. **查看分支**：git branch -a 显示本地和远端的所有分支；git branch 只显示本地分支

    查看分支状态：git log --graph

19. **删除分支**：

    删除已经合并过的分支：git branch -d <分支名>

    强制删除未合并的分支：git branch -D <分支名>

20. **分支对比**：

    dev分支相比主干有哪些不同提交：git log [-p] master..dev （仅列出dev中不同）

    dev分支和主干有哪些不同提交：git log [-p] master...dev （列出dev和主干中的不同）

21. **分支合并**：

    git checkout <需要同步主干的分支名>

    git merge master      #如无冲突，则自动完成提交

    git add .       #如有冲突，解决后提交

    git commit -m "提交备注"

    **撤销合并**（仅限有冲突未自动提交时）：

    git merge --abort

    **处理冲突**：文本文件冲突：编辑文本内容，然后保存（分隔符上部分为当前分支内容，下部分为远程分支内容）；非文本文件冲突：选择其中一方的版本，采用远程分支：git checkout --theirs lib/base.dll；采用当前分支：git checkout --ours lib/base.dll

    **分支合并的注意事项**：

    merge和rebase的区别：

    merge：形成一个新的节点；

    rebase：把分支的commit”剪“下来，然后追加到主干。

    **公共分支的代码合并禁用rebase。**

    merge和rebase优点：

    merge：优点：处理冲突更直接；适用于公共分支的代码同步和合并。

    rebase：优点：能够保证清晰地commit记录；适用于个人未提交远端的commit记录的优化。

22. **查看远端仓库**：

    查看从哪里clone的代码：git remote -v

23. **推送到远端仓库**：

    推送到远端：git push；git push <remote> <远程分支>

    删除远程分支：git push <remote>: <远程分支>

24. **代码版本tag**

    **查看tag**：本地tag： git tag；远程tag： git tag -r

    **添加tag**：给当前版本加tag： git tag 标签名；给历史版本加tag： git tag 标签名 commitid

    **删除tag**：删除本地标签：git tag -d 标签名；删除远程标签：git push origin -d 标签名

    **推送到远端仓库**：git push origin 标签名；推送所有未提交的tag： git push origin --tags

    **更新到本地**：git pull origin --tags



















