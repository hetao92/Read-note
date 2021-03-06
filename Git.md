

[TOC]

# Git

**分布式版本控制系统**

由C语言编写



**集中式 vs 分布式**

前者将版本库集中存放在中央服务器，本地**必须联网**取得最新版本，修改后再上传到服务器

后者没有“中央服务器”，每一个本地都有完整的版本库，无需联网。当然实际也有一台充当中央服务器的电脑，用来方便交换大家的修改



所有的版本控制系统，只能跟踪文本文件的改动，比如TXT文件，所有的程序代码等等。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。



**设置参数**

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

**创建目录**

```
$ mkdir 目录名
$ cd 目录名
$ pwd  //显示当前目录路径
```
**将目录初始化 git 仓库**
`$ git init `
会在目录内生成`.git`的目录，用`ls -ah`查看

**添加文件并提交**
`$ git add 文件名`  add 是添加到本地暂存区
`$ git commit` commit 是将暂存区内容提交到仓库
或者`$ git commit -m "提交说明"` 

**查看仓库状态**
`$ git status` 是否存在文件变化
`$ git diff 文件名`查看具体文件修改内容

**版本回退**
原理是 Git 仅将指向当前版本的 HEAD指针指向别的版本而已
`$ git log` 查看提交的历史记录
返回内容包括 commit id(版本号)、author、date
`$ git reflog` 查看命令的历史记录
`HEAD`表示当前版本，`HEAD^`表示上一个版本，`HEAD~n`表示往上 n 个版本

`$ git reset --hard HEAD^`即回退到上一个版本
或者 `$ git reset --hard commit id（前几位即可）`

利用reflog 可以查看最新版本的 commit id 以恢复到最新版本

**撤销修改**
`$ git checkout --file`将文件在工作区的修改全部撤销
若修改并未放到暂存区（未被 add ），则会撤销到版本库的状态
若修改已经添加到暂存区，则会撤销回到添加入暂存区的状态

`--`这两个 - 很重要，没有的话就变成切换分支的命令了

`$ git reset HEAD file` 把暂存区的修改撤销并放回到工作区，此后可进一步用上一个方法把工作区修改撤销



`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。



**删除文件**
`$ rm file`删除文件
`$ git rm file` 并 `git commit -m''` 即从版本库删除文件



**添加远程库**

先在 GitHub 上Create repository
将本地库关联远程库
`$ git remote add origin git@XXX`
origin 是Git对远程库的默认叫法

再推送改动至远程库
`$ git push -u origin master`
第一次推送加`-u`参数，Git不但会把本地的master分支内容推送到远程新的master分支，还会把本地的master分支和远程的master分支关联起来

以后只要本地作了提交，就可以通过命令：`$ git push origin master`即可上传

`$ git push <远程主机名> <本地分支名>:<远程分支名>`

如果省略远程分支名，则表示将本地分支推送与之存在”追踪关系”的远程分支(通常两者同名)，如果该远程分支不存在，则会被新建。

`$ git push origin master`即将本地的master分支推送到origin主机的master分支。如果后者不存在，则会被新建。



如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。

```
$ git push origin
```

**从远程库克隆**
`$ git clone git@XXX`



**分支**
A --> B --> C--> D

默认主分支 master ，每次新的提交都是在 master 上。而 HEAD 就是指向 master ，再由 master 提交。
HEAD -> master -> A
HEAD -> master -> B
HEAD -> master -> C
接来下创建分支dev，并将 HEAD 指向 dev
此时 HEAD —> dev -> C
以后的提交改动即
HEAD -> dev -> D，此时的 master 还指向 C

合并分支即将 master 指向 dev 的当前提交 HEAD -> master， 此时可以将 dev 分支删除

用带参数的`git log --graph`可以看到分支的合并情况

```bash
$ git checkout -b dev   //创建dev分支，然后切换到dev分支
//git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
$ git branch dev    创建
$ git checkout dev  切换

git branch 可以查看当前所有分支，*当前分支

//切换到 master 并进行合并

$ git checkout master
$ git merge dev
//git merge命令用于合并指定分支到当前分支。即 dev合并到当前 master 上

//删除分支
$ git branch -d dev
//丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除
$ git branch -D dev
```
合并分支有模式区分，通常 Git 会用 `Fast forward` 模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
普通模式（禁用Fast forward）
`$ git merge --no-ff -m "merge with no-ff" dev`
-m""是此模式要求合并要创建commit加上去的 

A -- B(master) -- C(dev)
Fast forward 模式下：
A -- B -- C(master dev)
`--no-ff`普通模式下：
A -- B -- D(master)
B -- C(dev) -- D(master)
此时可以看出来



**stash**

当你在一个分支工作的时候需要改动一个 bug，而 bug 是在 master 主分支上的，手头的dev 分支工作还没做完。
此时：
1.`$ git stash`         把当前工作现场“储藏”起来
再用`git status`查看工作区，就是干净的
2.切换至 master 分支，再创建临时分支用于修改 Bug ，修复后提交
`$ git checkout master`
`$ git checkout -b issue-101`
add、commit
3.切换回 master 合并临时分支
`$ git checkout master`
`$ git merge --no-ff -m "merged bug fix 101" issue-101`
4.回到分支
`$ git stash list`可以查看之前存的工作现场
用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除;
或者用`git stash pop`，恢复的同时把stash内容也删了.



**拓展：**

Git存在工作区以及版本库

工作区就是电脑里可以看到的目录。

版本库：
工作区有隐藏目录`.git` Git 版本库，内部有一个暂存区（叫 stage 或 index），和第一个分支 master，以及指针 HEAD



```
git diff    #是工作区(work dict)和暂存区(stage)的比较
git diff --cached    #是暂存区(stage)和分支(master)的比较
```
```
git add -A   // 添加所有改动

git add *     // 添加新建文件和修改，但是不包括删除

git add .    // 添加新建文件和修改，但是不包括删除

git add -u   // 仅监控已经被add的文件.添加修改和删除，但是不包括新建文件
```

`$ git remote` 查看远程库的信息(默认 origin)
`$ git remote -v` 详细信息

建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`

`$ git pull`
从远程抓取分支



**标签管理**

无论是那个分支都会发生变化，所以可以对某个 commit 创建标签，就可以固定取那个点的版本，标签也是版本库的一个快照。

`$ git tag <name>` 默认标签是打在最新提交的commit上
若要对某个id 打标签，可以通过`git log`查看历史提交的 commit id，再使用
`$ git tag <name> <commit id>`

带说明，用`-a`指定标签名，`-m`指定说明文字
`$ git tag -a <name> -m "content" commit id`



`git show <tagname>`查看标签信息

标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。



**删除标签**

`git tag -d <tagname>`

创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。



`git push origin <tagname>`可以将标签推送到远程

`git push origin —tags `推送本地全部尚未推送到远程的本地标签



删除远程标签

1.先删除本地标签

2.删除远程 `git push origin :refs/tags/<tagname>`



**git rebase**

将提交到某一分支上的所有修改都移至另一个分支上

原理是首先找到两个分支(当前分支 experiment 和变基操作的目标基底分支 master)的最近共同祖先 C2，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件，然后将当前分支指向目标基底C3，然后以此将之前另存为临时文件的修改依序应用，然后回到 master 分支，进行快进合并

```
git checkout experiment
git rebase master
git checkout master
git merge experiment
// 这种整合方法和 merge 的最终结果没有任何区别，但把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。
```



`git rebase`还可以合并多次提交记录

`git rebase -i HEAD~4`  即合并最近的 4 次提交记录。然后会进入`vi`编辑模式进行确认 commit 的一些操作...



### 忽略特殊文件
在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。



给命令设置别名

`git config --global alias.<aliasname> 'operationname'`



**cherry-pick**

多分支的情况下，如果一个分支需要另一个分支的所有代码变动可以采用`git merge`，若是只需要部分代码变动（某几个提交），那么可以采用`cherry pick`

`git cherry-pick <commitHash>`

将指定的提交`commitHash`，应用于当前分支。这会在当前分支产生一个新的提交，当然它们的哈希值会不一样。

`<commitHash>`也可以用分支名代替，则会将分支的最新一次提交转移到当前分支



同时也支持多个提交一起转移

```bash
git cherry-pick <HashA> <HashB>
```

支持一系列提交一起转移

```bash
git cherry-pick A..B
可以转移从 A 到 B 的所有提交。它们必须按照正确的顺序放置：提交 A 必须早于提交 B，否则命令将失败，但不会报错。同时，提交 A 将不会包含在 Cherry pick 中。

如果要包含提交 A
git cherry-pick A^..B 
```

在`cherry pick`过程中发生代码冲突，可以采取以下操作

1. `--continue`

   用户在解决冲突后，将修改文件重新加入暂存区`git add .`，执行操作继续执行

   `git cherry-pick --continue`

2. `--abort`

   发生代码冲突后，放弃合并，回到操作前的样子

3. `--quit`

   发生代码冲突后，退出 cherry pick，但是不回到操作前的样子



除了转移分支，同时也支持转移另一个代码库的提交

首先将该库加为远程仓库 target

`git remote add target git://gitUrl`

然后将远程代码抓取到本地

`git fetch target`

然后检查一下从远程仓库转移的提交，获取它的哈希值

`git log target/master`

最后使用命令转移提交

`git cherry-pick <commitHash>`



`git cherry-pick`指令后还可以跟一些常用配置项

+ `-e`, `--edit`		

  打开外部编辑器编辑提交信息

+ `-n`, `--no-commit`

  只更新工作区和暂存区，不产生新的提交

+ `-x`

  在提交信息的末尾追加一行`(cherry picked from commit ...)`，方便以后查到这个提交是如何产生的。

+ `-s`, `--signoff`

  在提交信息的末尾追加一行操作者的签名，表示是谁进行了这个操作。

+ `-m parent-number`, `--mainline parent-number`

  如果原始提交是一个合并节点，来自于两个分支的合并，那么 Cherry pick 默认将失败，因为它不知道应该采用哪个分支的代码变动。

  `-m`配置项告诉 Git，应该采用哪个分支的变动。它的参数`parent-number`是一个从`1`开始的整数，代表原始提交的父分支编号。

  `git cherry-pick -m 1 <commitHash>`

  Cherry pick 采用提交`commitHash`来自编号1的父分支的变动。

  一般来说，1号父分支是接受变动的分支（the branch being merged into），2号父分支是作为变动来源的分支（the branch being merged from）。