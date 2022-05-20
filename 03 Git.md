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

该命令显示由近到远的三次 `commit` 记录。

## 撤销和删除修改

### `reflog`

``colinfx@local:~/testgit$ git reflog``

该命令是 `log` 命令的升级版，由近到远显示所有 `commit` 记录，并额外显示每次提交的版本号。

示例：`6fcfc89 HEAD@{0}: commit: add readme.txt` 说明最近一次提交的版号为 6fcfc89。

### `reset --hard`

``colinfx@local:~/testgit$ git reset --hard HEAD^``

``colinfx@local:~/testgit$ git reset --hard HEAD~2``

该命令会将整个工作区回退到某一次 `commit` 时的快照。`HEAD^` 表示回撤到前一次提交，`HEAD~2` 表示回退两次，可以修改回退次数。

### `checkout --`

``colinfx@local:~/testgit$ git checkout -- readme.txt``

该命令将丢弃工作区中未被保存的修改。如果文件此时不在暂存区内，则恢复到上一次 `commit` 时的状态；如果此时文件在暂存区内，则恢复到上一次 `add` 时的状态。

注意，此时如果该文件已经在工作区内被 `rm` 完全删除，也能撤回删除操作。

## 远程仓库

### 创建 Github 仓库

1. ``colinfx@local:~/.ssh$ ssh-keygen -t rsa -C "xiao.fei@polytechnique.edu"`` 创建 SSH 密钥。将会生成 `id_rsa` 私钥文件和 `id_rsa.pub` 公钥文件。

2. 在网页打开并登录 Github，在设置中选择 SSH Keys 并点击 Add SSH key。

3. 任意填写名称，并将本地 `~/.ssh/id_rsa.pub` 文件中的内容复制到网页上密钥部分，创建密钥。

4. 在 Github 上点击 Create a new repo 新建一个仓库 `/ColinFX/testgit`。

### 初始化

```
colinfx@local:~/testgit$ git remote add origin https://github.com/ColinFX/testgit.git
colinfx@local:~/testgit$ git push -u origin master
```

其中 Github 链接可以通过网页查看。注意，http 协议是只读的，而 ssh 协议允许读写。

首次推送增加 `-u` 参数将会关联本地 master 分支和远程 master 分支。关于分支请详见下文**分支**部分。

`origin` 将会从此之后成为这个 URL 的别称，在其他的 Git 命令中可以以这个名字替换这个 URL。名字可以取任何的字符串，也可以通过 `git remote rename <old-name> <new-name>` 命令更改别称。

### `remote`

``colinfx@local:~/testgit$ git remote -v``

该命令查看当前本地仓库所连接的其他仓库，`-v` 参数额外显示远程仓库的 URL。`remote` 命令（包括 `remote add` 等）的所有配置都会保存在 `./.git/config` 文件中，可以手动修改。

### `push`

``colinfx@local:~/testgit$ git push origin master``

该命令将会把本地 master 分支的最新一次 `commit` 内容推送到 Github 上。注意，此时 `origin` 是在初始化时定下的一个 URL 别称。

### `clone`

``colinfx@local:~$ git clone https://github.com/ColinFX/testgit2.git``

该命令将会把 Github 上的整个快照克隆到本地。注意，该命令会在当前目录下创建一个和远程仓库名称相同的文件夹 `~/testgit2`，并且这个新的文件夹内不需要重新初始化本地仓库。

## 分支

## 多人合作

## 参考教程

https://www.eet-china.com/mp/a85036.html

https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud
