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
`$ git add 文件名`  add 是添加到本地缓存区
`$ git commit` commit 是将缓存区内容提交到仓库
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

`$ git reset HEAD file` 把暂存区的修改撤销并放回到工作区，此后可进一步用上一个方法把工作区修改撤销

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

`$ git push origin dev`即推送到 dev 分支

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

```
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

**修改 BUG**
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



###拓展：
隐藏目录：
`.git` Git 版本库，内部有一个暂存区（叫 stage 或 index），和第一个分支 master，以及指针 HEAD
```
git diff    #是工作区(work dict)和暂存区(stage)的比较
git diff --cached    #是暂存区(stage)和分支(master)的比较
```
```
git add -A   // 添加所有改动

git add *     // 添加新建文件和修改，但是不包括删除

git add .    // 添加新建文件和修改，但是不包括删除

git add -u   // 添加修改和删除，但是不包括新建文件
```

`$ git remote` 查看远程库的信息(默认 origin)
`$ git remote -v` 详细信息

建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`


`$ git pull`
从远程抓取分支

无论是那个分支都会发生变化，所以可以对某个 commit 创建标签，就可以固定取那个点的版本

`$ git tag <name>` 默认标签是打在最新提交的commit上
若要对某个id 打标签，可以通过`git log`查看历史提交的 commit id，再使用
`$ git tag <name> commit id`

带说明，用`-a`指定标签名，`-m`指定说明文字
`$ git tag -a <name> -m "content" commit id`
`git show <tagname>`查看标签信息


###忽略特殊文件
在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。