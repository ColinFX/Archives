# Git

本文阐述 Git 分布式版本控制系统在 Ubuntu 系统下的安装和使用。

## Cheat sheet

<img src="https://user-images.githubusercontent.com/45534476/169618224-03bae472-c9ad-483e-b125-75fb5eb71b92.png" width="1000">

## Git 工作模式

<img src="https://user-images.githubusercontent.com/45534476/168778955-0316f532-8ded-4561-aa04-25b250ea7c4c.png" width="500">

Workspace：工作区

Index/Stage：暂存区

Repository：本地仓库

Remote：远程仓库

## 安装和配置

```
colinfx@local:~$ sudo apt-get update
colinfx@local:~$ sudo apt-get install git
colinfx@local:~$ git --version
colinfx@local:~$ git config --global user.name "xiao.fei"
colinfx@local:~$ git config --global user.email "xiao.fei@polytechnique.edu"
```

注意替换个人用户名和邮箱地址，作为多人协作标识。

## 初始化本地仓库（repository）

```
colinfx@local:~$ cd ~/testgit
colinfx@local:~/testgit$ git init
```

注意替换文件地址为实际位置，可以新创建一个工作区（workspace），也可以在现有的代码工作区的基础上初始化本地仓库。命令完成后会在该文件夹内新增一个 .git 子文件夹，用于跟踪管理版本。

## 保存修改

### `add`

``colinfx@local:~/testgit$ git add readme.txt`` 

``colinfx@local:~/testgit$ git add -p``

该命令将 readme.txt 文件添加到暂存区（stage），告诉 Git 在下一次 `commit` 时提交该文件。

`-p` 参数将所有文件（除了在`.gitignore` 列表中）全部添加到暂存区。

该指令仅仅将指定文件或文件夹添加到一个“提交列表“当中，指示下一次执行 `commit` 时需要提交那些文件。因此，如果文件内容在 `add` 指令之后、且在 `commit` 指令之前被修改，这些修改的内容同样会在下一次 `commit` 时被提交。

### `status`

``colinfx@local:~/testgit$ git status``

该命令检查当前的等待提交状态，分类显示以下三种文件：已经被添加到暂存区、等待提交的文件（Changes to be committed）+ 被 Git 追踪、内容被修改但是没有被添加到暂存区的文件（Changed but not updated）+ 未被 Git 追踪的文件（Untracked files）。

### `diff`

``colinfx@local:~/testgit$ git diff readme.txt``

该指令将会比较当前工作区文件（current workspace）和最近一次提交（previous commited repository）之间的区别。

注意，Git 系统只能比较文本文件（包括 txt 文件、程序代码等）的改动，而对于其他形式的文件（包括图片等）只能比较其文件大小。

### `commit`

``colinfx@local:~/testgit$ git commit -m "add readme.txt"``

``colinfx@local:~/testgit$ git commit -am "add readme.txt"``

该命令将暂存区中的文件（所谓“待提交的文件列表”）提交到本地仓库，从而创建一个当前状态的快照。

`-m "add readme.txt"` 为本次提交添加一则注释。若没有`-m` 参数则会自动打开一个本地文本编辑器以输入注释。

`-a` 参数将当前工作区所有被追踪的文件改动，无论是否有被添加到暂存区，都考虑在内创建一个快照。所有在之前被 `add` 命令执行的以及不在 `.gitignore` 列表中文件都被视为被追踪的（Tracked files）。注意，待提交的改动（Changes to be committed）不同于被追踪的改动（Changes to be committed + Changed but not updated）。

### `log`

``colinfx@local:~/testgit$ git log``

该命令显示由近到远 `commit` 记录（包括分支记录）。

## 撤销和删除修改

### `reflog`

``colinfx@local:~/testgit$ git reflog``

该命令是 `log` 命令的升级版，由近到远显示 `commit` 记录，并额外显示每次提交的版本号。

示例：`6fcfc89 HEAD@{0}: commit: add readme.txt` 说明最近一次提交的版号为 6fcfc89。

### `reset --hard`

``colinfx@local:~/testgit$ git reset --hard HEAD^``

``colinfx@local:~/testgit$ git reset --hard HEAD~2``

该命令会将整个工作区回退到某一次 `commit` 时的快照。`HEAD^` 表示回撤到前一次提交，`HEAD~2` 表示回退两次，可以修改回退次数。

### `checkout --`

``colinfx@local:~/testgit$ git checkout -- readme.txt``

该命令将丢弃工作区中未被保存的修改。如果文件此时不在暂存区内，则恢复到上一次 `commit` 时的状态；如果此时文件在暂存区内，则恢复到上一次 `add` 时的状态。

注意，此时如果该文件已经在工作区内被 `rm` 完全删除，也能撤回删除操作。

## 分支

通常，主分支上的文件会直接应用于生产环境，应该尽量保持稳定，不允许随意更改。为了发布新的版本或者为了修复某些 bug，我们需要在其他的分支上修改代码，然后在需要发布的时候合并到主分支上来。

### `branch`

``colinfx@local:~/testgit$ git branch``

该命令将显示所有的分支并且在当前分支前添加一个星号突出显示。

``colinfx@local:~/testgit$ git branch dev``

该命令将会新创建一个名为 `dev` 的分支，但是并不会切换当前 HEAD 位置。

### `checkout`

``colinfx@local:~/testgit$ git checkout dev``

``colinfx@local:~/testgit$ git checkout -b dev``

该命令将会切换到 `dev` 分支上。注意，此时 Git 将会切换工作区内的文件到 `master` 分支的最后一个快照。

注意，如果在切换分支的时候有修改的内容在工作区或暂存区没有被提交，这些内容会被同步迁移到切换后的分支的工作区或暂存区上。

注意，切换分支命令并不负责分支创建。添加 `-b` 参数会在该分支并未被创建时自动创建一个分支并切换到该分支上。`git checkout -b dev` 等同于先 `git branch dev` 然后 `git checkout dev`。

### `merge`

``colinfx@local:~/testgit$ git merge dev``

该命令将会合并所指定的分支 `dev` 到当前所在的分支上。注意，合并分支命令并不会删除被合并的分支 `dev`。

如果只有被合并的分支上有新的修改，而当前分支没有，则 Git 会进入 Fast-forward 模式，直接应用被合并分支上的修改内容到主分支上。

如果当前分支上的工作区或暂存区有新的修改没有被提交，则合并命令不会被开始。

如果在两个分支上分别进行了不同的修改，Git 将会检测到冲突（conflict）并且终止分支的合并。更多信息详见下文**解决分支冲突**内容。

### `branch -d`

``colinfx@local:~/testgit$ git branch -d dev``

该命令将会完全删除指定的 `dev` 分支。

### `stash`

有时，在某一个分支上，我们忘记在最近一个的 `commit` 中修改一些内容 A，但是用于下一个 `commit` 的修改工作 B 已经开始了。此时，我们希望先应用修改 A，然后再继续修改 B 的工作。因此，我们需要将完成一半的修改 B 暂时隐藏：

``colinfx@local:~/testgit$ git stash save``

此时工作区的文件将会回到最近一次 `commit` 时的状态，我们由此可以执行修改 A 并覆盖该 `commit`：

```
colinfx@local:~/testgit$ git add -u
colinfx@local:~/testgit$ git commit --amend
```

其中 `-u` 表示更新提交列表，`--amend` 表示修改最近的一次 `commit` 而不是新建立一个。

最后，我们可以再次调出之前进行到一半的修改 B 的内容：

``colinfx@local:~/testgit$ git stash pop``

## 解决分支冲突

例如：`dev` 分支从 `master` 分支上创建以后，用户在 `dev` 和 `master` 两个分支上都对 `readme.txt` 进行了不同的修改。

此时在 `master` 分支上执行 `git merge dev` 程序将会报错，并且此时分支状态将会从 `(master)` 进入到 `(master|MERGING)` 模式。可以执行 `git merge --abort` 命令退出合并模式，这将会让当前工作区文件回到 `master` 最近一次提交的快照状态。

此时可以执行 `git status` 查看冲突的文件（Unmerged paths, both modified）。

此时工作区中的文件 readme.txt 已经被修改到一个中间状态，格式如下，： `<<<<<<< HEAD` 标注的是当前 `master` 分支的修改内容，`>>>>>>> dev` 标注的是被合并 `dev` 分支的修改内容。例如：以下的示例表示该文件在 `master` 分支上添加了 `77777` 内容，而在 `dev` 上添加了 `88888` 内容，导致了冲突。

```
<<<<<<< HEAD
77777
=======
88888
>>>>>>> dev
```

我们希望`dev` 分支服从 `master` 分支的修改，因此需要更改 `dev` 分支上的内容以解决冲突。

此时只要修改当前工作区中的文件到 `77777` 的状态，并且在当前 `master` 分支上 `commit`，冲突就会被解决，分支状态从 `(master|MERGING)` 回到 `(master)` 模式，合并结束。注意，此次 `commit` 并不是一个普通的 `commit` 而是一个 `commit (merge)`。

## 远程仓库

### 创建 Github 仓库

1. ``colinfx@local:~/.ssh$ ssh-keygen -t rsa -C "xiao.fei@polytechnique.edu"`` 创建 SSH 密钥。将会生成 `id_rsa` 私钥文件和 `id_rsa.pub` 公钥文件。

2. 在网页打开并登录 Github，在设置中选择 SSH Keys 并点击 Add SSH key。

3. 任意填写名称，并将本地 `~/.ssh/id_rsa.pub` 文件中的内容复制到网页上密钥部分，创建密钥。

4. 在 Github 上点击 Create a new repo 新建一个仓库 `/ColinFX/testgit`。

注意，一台电脑只需要一个 SSH 密钥，因此在 Github 上添加公钥也只需要每台主机操作一次。

### 从本地开始初始化

```
colinfx@local:~/testgit$ git remote add origin https://github.com/ColinFX/testgit.git
colinfx@local:~/testgit$ git push -u origin master
```

其中 Github 链接可以通过网页查看。注意，http 协议是只读的，而 ssh 协议允许读写。

首次推送增加 `-u` 参数将会关联本地 `master` 分支和远程 `main` 分支。

`origin` 将会从此之后成为这个 URL 的别称，在其他的 Git 命令中可以以这个名字替换这个 URL。名字可以取任何的字符串，也可以通过 `git remote rename <old-name> <new-name>` 命令更改别称。

### 从远程开始初始化

``colinfx@local:~$ git clone https://github.com/ColinFX/testgit2.git``

该命令将会把 Github 上的整个快照克隆到本地。注意，该命令会在当前目录下创建一个和远程仓库名称相同的文件夹 `~/testgit2`，并且这个新的文件夹内不需要重新初始化本地仓库。

### `remote`

``colinfx@local:~/testgit$ git remote -v``

该命令查看当前本地仓库所连接的其他仓库，`-v` 参数额外显示远程仓库的 URL。`remote` 命令（包括 `remote add` 等）的所有配置都会保存在 `./.git/config` 文件中，可以手动修改。

### `fetch`

```
colinfx@local:~/testgit$ git fetch origin
colinfx@local:~/testgit$ git merge origin/master
```

第一个命令将会从远程仓库抓取本地仓库中没有的分支和 `commit`，执行完成后显示 `... main -> origin/master` 表示数据已经从云端下载到了本地，且 `main` 分支中的内容被暂存在 `origin/master` 分支下。注意这个分支的名字看起来像是在云端，但是经过 `fetch` 操作后所有的数据已经被下载到了本地。

第二个命令将会把下载好的远程仓库的 `origin/master` 分支更新信息合并到当前分支。

### `pull`

``colinfx@local:~/testgit$ git pull``

该命令将会自动将远程仓库上所有的修改内容下载并合并到本地仓库的各个分支。相当于先 `fetch` 后对每一个分支进行 `merge`。

### `checkout`

``colinfx@local:~/testgit$ git checkout -b dev origin/dev``

该命令将远程仓库上的 `dev` 分支复制到本地仓库的 `dev` 分支上，如果本地仓库没有 `dev` 分支，则会自动创建该分支。

### `push`

``colinfx@local:~/testgit$ git push origin dev:dev``

该命令将会把本地 `dev` 分支的最新一次 `commit` 内容推送到远程仓库 Github 的 `dev` 分支上，如果远程仓库没有 `dev` 分支，则会自动创建该分支。如果命令没有明确远程仓库的目标分支，则默认为 `main`。

注意，此时 `origin` 是在初始化时定下的一个 URL 别称。

## 参考链接

https://www.eet-china.com/mp/a85036.html

https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud

https://www.runoob.com/git/git-tutorial.html

## 个人思考

1. Git 整个系统的核心是一次次 `commit`。

2. workspace 中我们所接触到的文件是一个个 `commit` 的“影子文件”，而 HEAD 指示当前工作区中的文件是由哪个 `commit` 而来。我们可以在这些工作区文件的基础上进行修改，然后提交形成一个新的 `commit`。

3. 只有一条主线时，所有的 `commit` 按照顺序串在一条线上，即为主分支。创建了其他的分支以后，这些分支也是由串起来的一次次 `commit` 构成的，而合并分支所作的也是合并两个分支上各自最新的 `commit`。
