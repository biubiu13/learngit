# Git使用教程

Git是目前世界上最先进的分布式版本控制系统，能够自动记录每次文件的改动，同时能够让同事协同编辑。

## 基础命令

- `git init` 在一个文件夹中执行该命令，目的是将该文件夹初始化为一个`git`仓库，之后`git`会记录该仓库内的所有行为。
- `git add file` 将该文件夹下的一个文件添加到`git`仓库中，可一次添加多个文件。注意，`git`只能记录`txt`文件的版本变化，最好用`vscode`创建`txt`文件，不要用`windows`自带的文本编辑器。
- `git commit -m "message"` 输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样就能从历史记录里方便地找到改动记录。
- `git status` 查看仓库中修改了，但是还没提交（`add`）的文件。
- `git diff` 查看`git status`还没提交的文件的具体修改内容。

```
文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。一旦把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失。具体操作如下：
```

- `git log / git log --pretty=oneline` 显示从最近到最远的提交日志。
- `git reset --hard HEAD^` 回退到上一个版本，`HEAD^^`上上一个版本。
- `git reset --hard 1094a` 回到未来的`1094a`版本，`1094a`代表版本`id`的前几位。
- `git reflog` 记录每一次命令的版本`id`。

```
工作区与缓存区：git add 是把文件先添加到缓存区，git commit才是把修改之后的文件添加到master分支上去。
```

- `git diff HEAD -- readme.txt` 命令可以查看`工作区`和`版本库`里面最新版本的区别。因为有时候你在工作区修改了，但是没有`add->commit`到版本库中，所以二者可能是不一样的。`HEAD`表示当前版本。
- `git checkout -- readme.txt` 让这个文件回到最近一次`git commit`或`git add`时的状态，即撤销修改。
- `git reset HEAD readme.txt` 把暂存区的修改回退到工作区。即撤销最近的`add`操作。注意，可使用`git status`查看缓存区的状态。

```
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
```

- `git rm` 用于删除一个文件。

```
如果一个文件已经被提交到版本库，那么你永远不用担心误删，可以使用git checkout -- file还原已删除的文件。
```

## 远程仓库（Github）

```
将本地仓库添加到远程，首先在github上创建一个和本地同名的仓库（reposity），然后通过git remote add origin git@github.com:biubiu13/learngit.git将本地与远程关联起来，其中，origin为远程库的名字，可以随便取。
再通过git push -u origin master将本地文件推送到远程仓库，其中，当远程库为空时，需要加上-u参数。
之后只要本地做了commit，就可以通过git push origin master上传到远程。
解除本地仓库与远程库的关联：git remote rm origin。
```

- `git remote add origin git@github.com:biubiu13/learngit.git`
- `git push -u origin master`
- `git push origin master`

```
error: failed to push some refs to 'https://myusername@bitbucket.org/repo_user/repo_name.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

出现上述错误是因为：其他地方向同一远端库推送了代码，导致本地不是最新的。先从远端pull一下，执行：git pull，即可。
```

## 分支管理

```
假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。
```

- 只有一个人的时候不常用到，之后需要多人协作时再学，[网址](https://www.liaoxuefeng.com/wiki/896043488029600/896954848507552/)。

## 标签管理

```
发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

场景：
“请把上周一的那个版本打包发布，版本号是v1.2”
“好的，按照tag v1.2查找commit就行！”

所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。
```

- `git tag v1.0` 默认为最新一次的commit创建标签v1.0。
- `git tag` 查看所有标签。
- `git tag v0.9 f52c633` 为id为`f52c633`的commit创建标签v0.9。可通过`git log`查看所有commit的id。
- `git show <tagname>` 查看标签信息。
- `git tag -d v0.1` 删除标签。
- `git push origin v1.0` 推送标签到远程。
- `git push origin --tags` 推送所有未提交的标签到远程。
- 一般都是和分支（branch）绑定到一起的，所以也不常用，详细的教程在[该网站](https://www.liaoxuefeng.com/wiki/896043488029600/900788941487552)